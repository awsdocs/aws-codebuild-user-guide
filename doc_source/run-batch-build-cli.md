# Run a batch build \(AWS CLI\)<a name="run-batch-build-cli"></a>

1. Run the `start-build-batch` command in one of the following ways:

   ```
   aws codebuild start-build-batch --project-name <project-name>
   ```

   Use this if you want to run a build that uses the latest version of the build input artifact and the build project's existing settings\.

   ```
   aws codebuild start-build-batch --generate-cli-skeleton > <json-file>
   ```

   Use this if you want to run a build with an earlier version of the build input artifact or if you want to override the settings for the build output artifacts, environment variables, buildspec, or default build timeout period\.

1. If you run the start\-build\-batch command with the `--project-name` option, replace *<project\-name>* with the name of the build project, and then skip to step 6 of this procedure\. To get a list of build projects, see [View a list of build project names](view-project-list.md)\.

1. If you run the start\-build\-batch command with the `--idempotency-token` option, a unique case\-sensitive identifier, or token, is included with the `start-build-batch` request\. The token is valid for 5 minutes after the request\. If you repeat the `start-build-batch` request with the same token, but change a parameter, CodeBuild returns a parameter mismatch error\.

1. If you run the start\-build\-batch command with the `--generate-cli-skeleton` option, JSON\-formatted data is output to the *<json\-file>* file\. This file is similar to the skelton produced by the start\-build command, with the addition of the following object\. For more information about the common objects, see [Run a build \(AWS CLI\)](run-build-cli.md)\.

   Modify this file to add any build overrides, and save your results\.

   ```
     "buildBatchConfigOverride": {
       "combineArtifacts": combineArtifacts,
       "restrictions": {
         "computeTypesAllowed": [
           allowedComputeTypes
         ],
         "maximumBuildsAllowed": maximumBuildsAllowed
       },
       "serviceRole": "batchServiceRole",
       "timeoutInMins": batchTimeout
     }
   ```

   The `buildBatchConfigOverride` object is a [ProjectBuildBatchConfig](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_ProjectBuildBatchConfig.html) structure that contains the batch build configuration overides for this build\.  
*combineArtifacts*  
A boolean that specifies if the build artifacts for the batch build should be combined into a single artifact location\.  
*allowedComputeTypes*  
An array of strings that specify the compute types that are allowed for the batch build\. See [Build environment compute types](https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref-compute-types.html) for these values\.   
*maximumBuildsAllowed*  
Specifies the maximum number of builds allowed\.  
*batchServiceRole*  
Specifies the service role ARN for the batch build project\.  
*batchTimeout*  
Specifies the maximum amount of time, in minutes, that the batch build must be completed in\.

1. Switch to the directory that contains the file you just saved, and run the `start-build` command again\.

   ```
   aws codebuild start-build-batch --cli-input-json file://start-build.json
   ```

1. If successful, the JSON representation of a [BuildBatch](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_BuildBatch.html) object appears in the console output\. See the [StartBuildBatch Response Syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_ResponseSyntax) for an example of this data\.