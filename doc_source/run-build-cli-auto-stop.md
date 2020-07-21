# Stop running builds automatically \(AWS CLI\)<a name="run-build-cli-auto-stop"></a>

If your source code is stored in a GitHub or a GitHub Enterprise Server repository, you can set up GitHub webhooks to have AWS CodeBuild rebuild your source code whenever a code change is pushed to the repository\. For more information, see [Start running builds automatically \(AWS CLI\)](run-build-cli-auto-start.md)\.

If you have enabled this behavior, you can turn it off by running the `delete-webhook` command as follows:

```
aws codebuild delete-webhook --project-name <project-name>
```
+ where *<project\-name>* is the name of the build project that contains the source code to be rebuilt\.

If this command is successful, no information and no errors appear in the output\.

**Note**  
This deletes the webhook from your CodeBuild project only\. You should also delete the webhook from your GitHub or GitHub Enterprise Server repository\.