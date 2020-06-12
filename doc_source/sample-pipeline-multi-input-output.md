# AWS CodePipeline integration with CodeBuild and multiple input sources and output artifacts sample<a name="sample-pipeline-multi-input-output"></a>

An AWS CodeBuild project can take more than one input source\. It can also create more than one output artifact\. This sample demonstrates how to use AWS CodePipeline to create a build project that uses multiple input sources to create multiple output artifacts\. For more information, see [Multiple input sources and output artifacts sample](sample-multi-in-out.md)\.

 You can use a JSON\-formatted file that defines the structure of your pipeline, and then use it with the AWS CLI to create the pipeline\. Use the following JSON file as an example of a pipeline structure that creates a build with more than one input source and more than one output artifact\. Later in this sample you see how this file specifies the multiple inputs and outputs\. For more information, see [AWS CodePipeline Pipeline structure reference](https://docs.aws.amazon.com/codepipeline/latest/userguide/reference-pipeline-structure.html) in the *AWS CodePipeline User Guide*\. 

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
              "name": "artifact1"
            },
            {
              "name": "artifact2"
            }
          ],
          "configuration": {
            "ProjectName": "my-build-project-name",
            "PrimarySource": "source1"
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

 In this JSON file: 
+ One of your input sources must be designated the `PrimarySource`\. This source is the directory where CodeBuild looks for and runs your buildspec file\. The keyword `PrimarySource` is used to specify the primary source in the `configuration` section of the CodeBuild stage in the JSON file\. 
+ Each input source is installed in its own directory\. This directory is stored in the built\-in environment variable `$CODEBUILD_SRC_DIR` for the primary source and `$CODEBUILD_SRC_DIR_yourInputArtifactName` for all other sources\. For the pipeline in this sample, the two input source directories are `$CODEBUILD_SRC_DIR` and `$CODEBUILD_SRC_DIR_source2`\. For more information, see [Environment variables in build environments](build-env-ref-env-vars.md)\. 
+ The names of the output artifacts specified in the pipeline's JSON file must match the names of the secondary artifacts defined in your buildspec file\. This pipeline uses the following buildspec file\. For more information, see [Buildspec syntax](build-spec-ref.md#build-spec-ref-syntax)\. 
+ 

  ```
  version: 0.2
  
  phases:
    build:
      commands:
        - touch source1_file
        - cd $CODEBUILD_SRC_DIR_source2
        - touch source2_file
  
  artifacts:
    secondary-artifacts:
      artifact1:
        base-directory: $CODEBUILD_SRC_DIR
        files:
          - source1_file
      artifact2:
        base-directory: $CODEBUILD_SRC_DIR_source2
        files:
          - source2_file
  ```

 After you create the JSON file, you can create your pipeline\. Use the AWS CLI to run the **create\-pipeline** command and pass the file to the `--cli-input-json` parameter\. For more information, see [Create a pipeline \(CLI\)](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipelines-create.html#pipelines-create-cli) in the *AWS CodePipeline User Guide*\. 