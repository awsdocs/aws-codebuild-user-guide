# Step 7: View summarized build information<a name="getting-started-cli-monitor-build"></a>

\(Previous step: [Step 6: Run the build](getting-started-cli-run-build.md)\)

In this step, you view summarized information about the status of your build\.

## To view summarized build information<a name="getting-started-cli-monitor-build-cli"></a>

Use the AWS CLI to run the batch\-get\-builds command\.

```
aws codebuild batch-get-builds --ids id
```

Replace *id* with the `id` value that appeared in the output of the previous step\.

If successful, data similar to this appears in the output\.

```
{
  "buildsNotFound": [],
  "builds": [
    {
      "buildComplete": true,
      "phases": [
        {
          "phaseStatus": "SUCCEEDED",
          "endTime": 1472848788.525,
          "phaseType": "SUBMITTED",
          "durationInSeconds": 0,
          "startTime": 1472848787.882
        },
        ... The full list of build phases has been omitted for brevity ...
        {
          "phaseType": "COMPLETED",
          "startTime": 1472848878.079
        }
      ],
      "logs": {
        "groupName": "/aws/codebuild/codebuild-demo-project",
        "deepLink": "https://console.aws.amazon.com/cloudwatch/home?region=region-ID#logEvent:group=/aws/codebuild/codebuild-demo-project;stream=38ca1c4a-e9ca-4dbc-bef1-d52bfEXAMPLE",
        "streamName": "38ca1c4a-e9ca-4dbc-bef1-d52bfEXAMPLE"
      },
      "artifacts": {
        "md5sum": "MD5-hash",
        "location": "arn:aws:s3:::codebuild-region-ID-account-ID-output-bucket/message-util.zip",
        "sha256sum": "SHA-256-hash"
      },
      "projectName": "codebuild-demo-project",
      "timeoutInMinutes": 60,
      "initiator": "user-name",
      "buildStatus": "SUCCEEDED",
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
      "currentPhase": "COMPLETED",
      "startTime": 1472848787.882,
      "endTime": 1472848878.079,
      "id": "codebuild-demo-project:38ca1c4a-e9ca-4dbc-bef1-d52bfEXAMPLE",
      "arn": "arn:aws:codebuild:region-ID:account-ID:build/codebuild-demo-project:38ca1c4a-e9ca-4dbc-bef1-d52bfEXAMPLE"      
    }
  ]
}
```
+ `buildsNotFound` represents the build IDs for any builds where information is not available\. In this example, it should be empty\.
+ `builds` represents information about each build where information is available\. In this example, information about only one build appears in the output\.
  + `phases` represents the set of build phases CodeBuild runs during the build process\. Information about each build phase is listed separately as `startTime`, `endTime`, and `durationInSeconds` \(when the build phase started and ended, expressed in Unix time format, and how long it lasted, in seconds\), and `phaseType` such as \(`SUBMITTED`, `PROVISIONING`, `DOWNLOAD_SOURCE`, `INSTALL`, `PRE_BUILD`, `BUILD`, `POST_BUILD`, `UPLOAD_ARTIFACTS`, `FINALIZING`, or `COMPLETED`\) and `phaseStatus` \(such as `SUCCEEDED`, `FAILED`, `FAULT`, `TIMED_OUT`, `IN_PROGRESS`, or `STOPPED`\)\. The first time you run the batch\-get\-builds command, there might not be many \(or any\) phases\. After subsequent runs of the batch\-get\-builds command with the same build ID, more build phases should appear in the output\.
  + `logs` represents information in Amazon CloudWatch Logs about the build's logs\.
  + `md5sum` and `sha256sum` represent MD5 and SHA\-256 hashes of the build's output artifact\. These appear in the output only if the build project's `packaging` value is set to `ZIP`\. \(You did not set this value in this tutorial\.\) You can use these hashes along with a checksum tool to confirm file integrity and authenticity\.
**Note**  
You can also use the Amazon S3 console to view these hashes\. Select the box next to the build output artifact, choose **Actions**, and then choose **Properties**\. In the **Properties** pane, expand **Metadata**, and view the values for **x\-amz\-meta\-codebuild\-content\-md5** and **x\-amz\-meta\-codebuild\-content\-sha256**\. \(In the Amazon S3 console, the build output artifact's **ETag** value should not be interpreted to be either the MD5 or SHA\-256 hash\.\)  
If you use the AWS SDKs to get these hashes, the values are named `codebuild-content-md5` and `codebuild-content-sha256`\. 
  + `endTime` represents the time, in Unix time format, when the build process ended\.

## Next step<a name="getting-started-cli-monitor-build-next"></a>

[Step 8: View detailed build information](getting-started-cli-build-log.md)