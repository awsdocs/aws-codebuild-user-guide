# Command line reference for AWS CodeBuild<a name="cmd-ref"></a>

The AWS CLI provides commands for automating AWS CodeBuild\. Use the information in this topic as a supplement to the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/) and the [AWS CLI Reference for AWS CodeBuild](https://docs.aws.amazon.com/cli/latest/reference/codebuild/)\.

Not what you're looking for? If you want to use the AWS SDKs to call CodeBuild, see the [AWS SDKs and tools reference](sdk-ref.md)\.

To use the information in this topic, you should have already installed the AWS CLI and configured it for use with CodeBuild, as described in [Install and configure the AWS CLI](setting-up.md#setting-up-cli)\.

 To use the AWS CLI to specify the endpoint for CodeBuild, see [Specify the AWS CodeBuild endpoint \(AWS CLI\)](endpoint-specify.md#endpoint-specify-cli)\. 

Run this command to get a list of CodeBuild commands\.

```
aws codebuild help
```

Run this command to get information about a CodeBuild command, where *command\-name* is the name of the command\.

```
aws codebuild command-name help
```

CodeBuild commands include:
+ `batch-delete-builds`: Deletes one or more builds in CodeBuild\. For more information, see [Delete builds \(AWS CLI\)](delete-builds.md#delete-builds-cli)\.
+ `batch-get-builds`: Gets information about multiple builds in CodeBuild\. For more information, see [View build details \(AWS CLI\)](view-build-details.md#view-build-details-cli)\.
+ `batch-get-projects`: Gets information about one or more specified build projects\. For more information, see [View a build project's details \(AWS CLI\)](view-project-details.md#view-project-details-cli)\.
+ `create-project`: Creates a build project\. For more information, see [Create a build project \(AWS CLI\)](create-project-cli.md)\.
+ `delete-project`: Deletes a build project\. For more information, see [Delete a build project \(AWS CLI\)](delete-project.md#delete-project-cli)\.
+ `list-builds`: Lists Amazon Resource Names \(ARNs\) for builds in CodeBuild\. For more information, see [View a list of build IDs \(AWS CLI\)](view-build-list.md#view-build-list-cli)\.
+ `list-builds-for-project`: Gets a list of build IDs that are associated with a specified build project\. For more information, see [View a list of build IDs for a build project \(AWS CLI\)](view-builds-for-project.md#view-builds-for-project-cli)\.
+ `list-curated-environment-images`: Gets a list of Docker images managed by CodeBuild that you can use for your builds\. For more information, see [Docker images provided by CodeBuild](build-env-ref-available.md)\.
+ `list-projects`: Gets a list of build project names\. For more information, see [View a list of build project names \(AWS CLI\)](view-project-list.md#view-project-list-cli)\.
+ `start-build`: Starts running a build\. For more information, see [Run a build \(AWS CLI\)](run-build-cli.md)\.
+ `stop-build`: Attempts to stop the specified build from running\. For more information, see [Stop a build \(AWS CLI\)](stop-build.md#stop-build-cli)\.
+ `update-project`: Changes information about the specified build project\. For more information, see [Change a build project's settings \(AWS CLI\)](change-project-cli.md)\.