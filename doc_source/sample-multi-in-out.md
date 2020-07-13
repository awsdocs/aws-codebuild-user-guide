# Multiple input sources and output artifacts sample<a name="sample-multi-in-out"></a>

You can create an AWS CodeBuild build project with more than one input source and more than one set of output artifacts\. This sample shows you how to set up a build project that: 
+ Uses multiple sources and repositories of varying types\.
+ Publishes build artifacts to multiple S3 buckets in a single build\.

 In this sample, you create a build project and use it to run a build\. The sample uses the build project's buildspec file to show you how to incorporate more than one source and create more than one set of artifacts\. 

1.  Upload your sources to one or more S3 buckets, CodeCommit, GitHub, GitHub Enterprise Server, or Bitbucket repositories\. 

1.  Choose which source is the primary source\. This is the source in which CodeBuild looks for and executes your buildspec file\. 

1.  Create a build project\. For more information, see [Create a build project in AWS CodeBuild](create-project.md)\. 

1.  Follow the instructions in [Run AWS CodeBuild directly](how-to-run.md) to create your build project, run the build, and get information about the build\. 

1.  If you use the AWS CLI to create the build project, the JSON\-formatted input to the `create-project` command might look similar to the following: 

   ```
   {
     "name": "sample-project",
     "source": {
       "type": "S3",
       "location": "bucket/sample.zip"
     },
     "secondarySources": [
       {
         "type": "CODECOMMIT",
         "location": "https://git-codecommit.us-west-2.amazonaws.com/v1/repos/repo"
         "sourceIdentifier": "source1"
       },
       {
         "type": "GITHUB",
         "location": "https://github.com/awslabs/aws-codebuild-jenkins-plugin"
         "sourceIdentifier": "source2"
       }
     ],
     "secondaryArtifacts": [
       {
         "type": "S3",
         "location": "output-bucket",
         "artifactIdentifier": "artifact1"
       },
       {
         "type": "S3",
         "location": "other-output-bucket",
         "artifactIdentifier": "artifact2"
       }
     ],
     "environment": {
       "type": "LINUX_CONTAINER",
       "image": "aws/codebuild/standard:4.0",
       "computeType": "BUILD_GENERAL1_SMALL"
     },
     "serviceRole": "arn:aws:iam::account-ID:role/role-name",
     "encryptionKey": "arn:aws:kms:region-ID:account-ID:key/key-ID"
   }
   ```

 Your primary source is defined under the `source` attribute\. All other sources are called secondary sources and appear under `secondarySources`\. All secondary sources are installed in their own directory\. This directory is stored in the built\-in environment variable `CODEBUILD_SRC_DIR_sourceIdentifer`\. For more information, see [Environment variables in build environments](build-env-ref-env-vars.md)\. 

 The `secondaryArtifacts` attribute contains a list of artifact definitions\. These artifacts use the `secondary-artifacts` block of the buildspec file that is nested inside the `artifacts` block\. 

 Secondary artifacts in the buildspec file have the same structure as artifacts and are separated by their artifact identifier\. 

**Note**  
 In the [CodeBuild API](https://docs.aws.amazon.com/codebuild/latest/APIReference/), the `artifactIdentifier` on a secondary artifact is a required attribute in `CreateProject` and `UpdateProject`\. It must be used to reference a secondary artifact\. 

 Using the preceding JSON\-formatted input, the buildspec file for the project might look like: 

```
version: 0.2

phases:
  install:
    runtime-versions:
      java: openjdk11
  build:
    commands:
      - cd $CODEBUILD_SRC_DIR_source1
      - touch file1
      - cd $CODEBUILD_SRC_DIR_source2
      - touch file2

artifacts:
  secondary-artifacts:
    artifact1:
      base-directory: $CODEBUILD_SRC_DIR_source1
      files:
        - file1
    artifact2:
      base-directory: $CODEBUILD_SRC_DIR_source2
      files:
        - file2
```

 You can override the version of the primary source using the API with the `sourceVersion` attribute in `StartBuild`\. To override one or more secondary source versions, use the `secondarySourceVersionOverride` attribute\. 

 The JSON\-formatted input to the the `start-build` command in the AWS CLI might look like: 

```
{
   "projectName": "sample-project",
   "secondarySourcesVersionOverride": [
      {
        "sourceIdentifier": "source1",
        "sourceVersion": "codecommit-branch"
      },
      {
        "sourceIdentifier": "source2",
        "sourceVersion": "github-branch"
      },
   ]
}
```

## Project without a source sample<a name="no-source"></a>

 You can configure a CodeBuild project by choosing the **NO\_SOURCE** source type when you configure your source\. When your source type is **NO\_SOURCE**, you cannot specify a buildspec file because your project does not have a source\. Instead, you must specify a YAML\-formatted buildspec string in the `buildspec` attribute of the JSON\-formatted input to the `create-project` CLI command\. It might look like this: 

```
{
  "name": "project-name",
  "source": {
    "type": "NO_SOURCE",
    "buildspec": "version: 0.2\n\nphases:\n  build:\n    commands:\n      - command"
   },
  "environment": {
    "type": "LINUX_CONTAINER",
    "image": "aws/codebuild/standard:4.0",
    "computeType": "BUILD_GENERAL1_SMALL",    
  },
  "serviceRole": "arn:aws:iam::account-ID:role/role-name",
  "encryptionKey": "arn:aws:kms:region-ID:account-ID:key/key-ID"
}
```

For more information, see [Create a build project \(AWS CLI\)](create-project-cli.md)\.

To learn how to to create a pipeline that uses multiple source inputs to CodeBuild to create multiple output artifacts, see [ AWS CodePipeline integration with CodeBuild and multiple input sources and output artifacts sample ](sample-pipeline-multi-input-output.md)\.