# Create a pipeline that uses CodeBuild \(AWS CLI\)<a name="how-to-create-pipeline-cli"></a>

Use the following procedure to create a pipeline that uses CodeBuild to build your source code\.

To use the AWS CLI to create a pipeline that deploys your built source code or that only tests your source code, you can adapt the instructions in [Edit a pipeline \(AWS CLI\)](https://docs.aws.amazon.com/codepipeline/latest/userguide/how-to-edit-pipelines.html#how-to-edit-pipelines-cli) and the [CodePipeline pipeline structure reference](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipeline-structure.html) in the *AWS CodePipeline User Guide*\.

1. Create or identify a build project in CodeBuild\. For more information, see [Create a build project](create-project.md)\.
**Important**  
The build project must define build output artifact settings \(even though CodePipeline overrides them\)\. For more information, see the description of `artifacts` in [Create a build project \(AWS CLI\)](create-project-cli.md)\.

1. Make sure you have configured the AWS CLI with the AWS access key and AWS secret access key that correspond to one of the IAM entities described in this topic\. For more information, see [Getting set up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

1. Create a JSON\-formatted file that represents the structure of the pipeline\. Name the file `create-pipeline.json` or similar\. For example, this JSON\-formatted structure creates a pipeline with a source action that references an S3 input bucket and a build action that uses CodeBuild:

   ```
   {
     "pipeline": {
       "roleArn": "arn:aws:iam::<account-id>:role/<AWS-CodePipeline-service-role-name>",
       "stages": [
         {
           "name": "Source",
           "actions": [
             {
               "inputArtifacts": [],
               "name": "Source",
               "actionTypeId": {
                 "category": "Source",
                 "owner": "AWS",
                 "version": "1",
                 "provider": "S3"
               },
               "outputArtifacts": [
                 {
                   "name": "MyApp"
                 }
               ],
               "configuration": {
                 "S3Bucket": "<bucket-name>",
                 "S3ObjectKey": "<source-code-file-name.zip>"
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
                   "name": "MyApp"
                 }
               ],
               "name": "Build",
               "actionTypeId": {
                 "category": "Build",
                 "owner": "AWS",
                 "version": "1",
                 "provider": "CodeBuild"
               },
               "outputArtifacts": [
                 {
                   "name": "default"
                 }
               ],
               "configuration": {
                 "ProjectName": "<build-project-name>"
               },
               "runOrder": 1
             }
           ]
         }
       ],
       "artifactStore": {
         "type": "S3",
         "location": "<CodePipeline-internal-bucket-name>"
       },
       "name": "<my-pipeline-name>",
       "version": 1
     }
   }
   ```

   In this JSON\-formatted data:
   + The value of `roleArn` must match the ARN of the CodePipeline service role you created or identified as part of the prerequisites\.
   + The values of `S3Bucket` and `S3ObjectKey` in `configuration` assume the source code is stored in an S3 bucket\. For settings for other source code repository types, see the [CodePipeline pipeline structure reference](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipeline-structure.html) in the *AWS CodePipeline User Guide*\.
   + The value of `ProjectName` is the name of the CodeBuild build project you created earlier in this procedure\.
   + The value of `location` is the name of the S3 bucket used by this pipeline\. For more information, see [Create a policy for an S3 Bucket to use as the artifact store for CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/access-permissions.html#how-to-create-bucket-policy) in the *AWS CodePipeline User Guide*\.
   + The value of `name` is the name of this pipeline\. All pipeline names must be unique to your account\.

   Although this data describes only a source action and a build action, you can add actions for activities related to testing, deploying the build output artifact, invoking AWS Lambda functions, and more\. For more information, see the [AWS CodePipeline pipeline structure reference](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipeline-structure.html) in the *AWS CodePipeline User Guide*\.

1. Switch to the folder that contains the JSON file, and then run the CodePipeline [create\-pipeline](https://docs.aws.amazon.com/cli/latest/reference/codepipeline/create-pipeline.html) command, specifying the file name:

   ```
   aws codepipeline create-pipeline --cli-input-json file://create-pipeline.json
   ```
**Note**  
You must create the pipeline in an AWS Region where CodeBuild is supported\. For more information, see [AWS CodeBuild](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the *Amazon Web Services General Reference*\.

   The JSON\-formatted data appears in the output, and CodePipeline creates the pipeline\.

1. To get information about the pipeline's status, run the CodePipeline [get\-pipeline\-state](https://docs.aws.amazon.com/cli/latest/reference/codepipeline/get-pipeline-state.html) command, specifying the name of the pipeline:

   ```
   aws codepipeline get-pipeline-state --name <my-pipeline-name>
   ```

   In the output, look for information that confirms the build was successful\. Ellipses \(`...`\) are used to show data that has been omitted for brevity\.

   ```
   {
     ...
     "stageStates": [
       ...  
       {
         "actionStates": [
           {
             "actionName": "CodeBuild",
             "latestExecution": {
               "status": "SUCCEEDED",
               ...
             },
             ...
           }
         ]
       }
     ]
   }
   ```

   If you run this command too early, you might not see any information about the build action\. You might need to run this command multiple times until the pipeline has finished running the build action\.

1. After a successful build, follow these instructions to get the build output artifact\. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.
**Note**  
You can also get the build output artifact by choosing the **Build artifacts** link on the related build details page in the CodeBuild console\. To get to this page, skip the rest of the steps in this procedure, and see [View build details \(console\)](view-build-details.md#view-build-details-console)\.

1. In the list of buckets, open the bucket used by the pipeline\. The name of the bucket should follow the format `codepipeline-<region-ID>-<random-number>`\. You can get the bucket name from the `create-pipeline.json` file or you can run the CodePipeline get\-pipeline command to get the bucket's name\.

   ```
   aws codepipeline get-pipeline --name <pipeline-name>
   ```

    In the output, the `pipeline` object contains an `artifactStore` object, which contains a `location` value with the name of the bucket\.

1. Open the folder that matches the name of your pipeline \(for example, `<pipeline-name>`\)\.

1. In that folder, open the folder named `default`\.

1. Extract the contents of the file\. If there are multiple files in that folder, extract the contents of the file with the latest **Last Modified** timestamp\. \(You might need to give the file a `.zip` extension so that you can work with it in your system's ZIP utility\.\) The build output artifact is in the extracted contents of the file\.