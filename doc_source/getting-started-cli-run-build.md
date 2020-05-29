# Step 6: Run the build<a name="getting-started-cli-run-build"></a>

\(Previous step: [Step 5: Create the build project](getting-started-cli-create-build-project.md)\)

In this step, you instruct AWS CodeBuild to run the build with the settings in the build project\.<a name="getting-started-run-build-cli"></a>

**To run the build**

1. Use the AWS CLI to run the start\-build command:

   ```
   aws codebuild start-build --project-name project-name
   ```

   Replace *project\-name* with your build project name from the previous step \(for example, `codebuild-demo-project`\)\.

1. If successful, data similar to the following appears in the output:

   ```
   {
     "build": { 
       "buildComplete": false,
       "initiator": "user-name",   
       "artifacts": { 
         "location": "arn:aws:s3:::codebuild-region-ID-account-ID-output-bucket/message-util.zip"
       },
       "projectName": "codebuild-demo-project",
       "timeoutInMinutes": 60, 
       "buildStatus": "IN_PROGRESS",
       "environment": {
         "computeType": "BUILD_GENERAL1_SMALL",
         "image": "aws/codebuild/standard:4.0",
         "type": "LINUX_CONTAINER",
         "environmentVariables": []
       },
       "source": {
         "type": "S3",
         "location": "codebuild-region-ID-account-ID-input-bucket/MessageUtil.zip"
       },
       "currentPhase": "SUBMITTED",
       "startTime": 1472848787.882,
       "id": "codebuild-demo-project:0cfbb6ec-3db9-4e8c-992b-1ab28EXAMPLE",
       "arn": "arn:aws:codebuild:region-ID:account-ID:build/codebuild-demo-project:0cfbb6ec-3db9-4e8c-992b-1ab28EXAMPLE"    
     }
   }
   ```
   + `build` represents information about this build\.
     + `buildComplete` represents whether the build was completed \(`true`\)\. Otherwise, `false`\.
     + `initiator` represents the entity that started the build\.
     + `artifacts` represents information about the build output, including its location\.
     + `projectName` represents the name of the build project\.
     + `buildStatus` represents the current build status when the start\-build command was run\.
     + `currentPhase` represents the current build phase when the start\-build command was run\.
     + `startTime` represents the time, in Unix time format, when the build process started\.
     + `id` represents the ID of the build\.
     + `arn` represents the ARN of the build\.

   Make a note of the `id` value\. You need it in the next step\.

## Next step<a name="getting-started-cli-run-build-next"></a>

[Step 7: View summarized build information](getting-started-cli-monitor-build.md)