# AWS CodePipeline integration with CodeBuild and batch builds<a name="sample-pipeline-batch"></a>

AWS CodeBuild now supports batch builds\. This sample demonstrates how to use AWS CodePipeline to create a build project that uses batch builds\. 

You can use a JSON\-formatted file that defines the structure of your pipeline, and then use it with the AWS CLI to create the pipeline\. For more information, see [AWS CodePipeline Pipeline structure reference](https://docs.aws.amazon.com/codepipeline/latest/userguide/reference-pipeline-structure.html) in the *AWS CodePipeline User Guide*\.

## Batch build with individual artifacts<a name="sample-pipeline-batch.separate-artifacts"></a>

Use the following JSON file as an example of a pipeline structure that creates a batch build with separate artifacts\. To enable batch builds in CodePipeline, set the `BatchEnabled` parameter of the `configuration` object to `true`\.

```
{
  "pipeline": {
    "roleArn": "arn:aws:iam::account-id:role/my-AWS-CodePipeline-service-role-name",
    "stages": [
      {
        "name": "Source",
        "actions": [
          {
            "inputArtifacts": [],
            "name": "Source1",
            "actionTypeId": {
              "category": "Source",
              "owner": "AWS",
              "version": "1",
              "provider": "S3"
            },
            "outputArtifacts": [
              {
                "name": "source1"
              }
            ],
            "configuration": {
              "S3Bucket": "my-input-bucket-name",
              "S3ObjectKey": "my-source-code-file-name.zip"
            },
            "runOrder": 1
          },
          {
            "inputArtifacts": [],
            "name": "Source2",
            "actionTypeId": {
              "category": "Source",
              "owner": "AWS",
              "version": "1",
              "provider": "S3"
            },
            "outputArtifacts": [
              {
                "name": "source2"
              }
            ],
            "configuration": {
              "S3Bucket": "my-other-input-bucket-name",
              "S3ObjectKey": "my-other-source-code-file-name.zip"
            },
            "runOrder": 1
          }
        ]
      },
      {
        "name": "Build",
        "actions": [
          {
            "inputArtifacts": [
              {
                "name": "source1"
              },
              {
                "name": "source2"
              }
            ],
            "name": "Build",
            "actionTypeId": {
              "category": "Build",
              "owner": "AWS",
              "version": "1",
              "provider": "AWS CodeBuild"
            },
            "outputArtifacts": [
              {
                "name": "build1"
              },
              {
                "name": "build1_artifact1"
              },
              {
                "name": "build1_artifact2"
              },
              {
                "name": "build2_artifact1"
              },
              {
                "name": "build2_artifact2"
              }
            ],
            "configuration": {
              "ProjectName": "my-build-project-name",
              "PrimarySource": "source1",
              "BatchEnabled": "true"
            },
            "runOrder": 1
          }
        ]
      }
    ],
    "artifactStore": {
      "type": "S3",
      "location": "AWS-CodePipeline-internal-bucket-name"
    },
    "name": "my-pipeline-name",
    "version": 1
  }
}
```

The following is an example of a CodeBuild buildspec file that will work with this pipeline configuration\.

```
version: 0.2
batch:
  build-list:
    - identifier: build1
      env:
        compute-type: BUILD_GENERAL1_SMALL
    - identifier: build2
      env:
        compute-type: BUILD_GENERAL1_MEDIUM

phases:
  build:
    commands:
      - echo 'file' > output_file

artifacts:
  files:
    - output_file
  secondary-artifacts:
    artifact1:
      files:
        - output_file
    artifact2:
      files:
        - output_file
```

The names of the output artifacts specified in the pipeline's JSON file must match the identifier of the builds and artifacts defined in your buildspec file\. The syntax is *buildIdentifier* for the primary artifacts, and *buildIdentifier*\_*artifactIdentifier* for the secondary artifacts\.

For example, for output artifact name `build1`, CodeBuild will upload the primary artifact of `build1` to the location of `build1`\. For output name `build1_artifact1`, CodeBuild will upload the secondary artifact `artifact1` of `build1` to the location of `build1_artifact1`, and so on\. If only one output location is specified, the name should be *buildIdentifier* only\.

After you create the JSON file, you can create your pipeline\. Use the AWS CLI to run the **create\-pipeline** command and pass the file to the `--cli-input-json` parameter\. For more information, see [Create a pipeline \(CLI\)](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipelines-create.html#pipelines-create-cli) in the *AWS CodePipeline User Guide*\. 

## Batch build with combined artifacts<a name="sample-pipeline-batch.combined-artifacts"></a>

Use the following JSON file as an example of a pipeline structure that creates a batch build with combined artifacts\. To enable batch builds in CodePipeline, set the `BatchEnabled` parameter of the `configuration` object to `true`\. To combine the build artifacts into the same location, set the `CombineArtifacts` parameter of the `configuration` object to `true`\.

```
{
 "pipeline": {
  "roleArn": "arn:aws:iam::account-id:role/my-AWS-CodePipeline-service-role-name",
  "stages": [
    {
      "name": "Source",
      "actions": [
        {
          "inputArtifacts": [],
          "name": "Source1",
          "actionTypeId": {
            "category": "Source",
            "owner": "AWS",
            "version": "1",
            "provider": "S3"
          },
          "outputArtifacts": [
            {
              "name": "source1"
            }
          ],
          "configuration": {
            "S3Bucket": "my-input-bucket-name",
            "S3ObjectKey": "my-source-code-file-name.zip"
          },
          "runOrder": 1
        },
        {
          "inputArtifacts": [],
          "name": "Source2",
          "actionTypeId": {
            "category": "Source",
            "owner": "AWS",
            "version": "1",
            "provider": "S3"
          },
          "outputArtifacts": [
            {
              "name": "source2"
            }
          ],
          "configuration": {
            "S3Bucket": "my-other-input-bucket-name",
            "S3ObjectKey": "my-other-source-code-file-name.zip"
          },
          "runOrder": 1
        }
      ]
    },
    {
      "name": "Build",
      "actions": [
        {
          "inputArtifacts": [
            {
              "name": "source1"
            },
            {
              "name": "source2"
            }
          ],
          "name": "Build",
          "actionTypeId": {
            "category": "Build",
            "owner": "AWS",
            "version": "1",
            "provider": "AWS CodeBuild"
          },
          "outputArtifacts": [
            {
              "name": "output1 "
            }
          ],
          "configuration": {
            "ProjectName": "my-build-project-name",
            "PrimarySource": "source1",
             "BatchEnabled": "true",
             "CombineArtifacts": "true"
          },
          "runOrder": 1
        }
      ]
    }
  ],
  "artifactStore": {
    "type": "S3",
    "location": "AWS-CodePipeline-internal-bucket-name"
  },
  "name": "my-pipeline-name",
  "version": 1
 }
}
```

The following is an example of a CodeBuild buildspec file that will work with this pipeline configuration\.

```
version: 0.2
batch:
  build-list:
    - identifier: build1
      env:
        compute-type: BUILD_GENERAL1_SMALL
    - identifier: build2
      env:
        compute-type: BUILD_GENERAL1_MEDIUM

phases:
  build:
    commands:
      - echo 'file' > output_file

artifacts:
  files:
    - output_file
```

If combined artifacts is enabled for the batch build, there is only one output allowed\. CodeBuild will combine the primary artifacts of all the builds into one single ZIP file\.

After you create the JSON file, you can create your pipeline\. Use the AWS CLI to run the **create\-pipeline** command and pass the file to the `--cli-input-json` parameter\. For more information, see [Create a pipeline \(CLI\)](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipelines-create.html#pipelines-create-cli) in the *AWS CodePipeline User Guide*\. 