# Command Line Reference for AWS CodeBuild<a name="cmd-ref"></a>

The AWS CLI provides commands for automating AWS CodeBuild\. Use the information in this topic as a supplement to the [AWS Command Line Interface User Guide](http://docs.aws.amazon.com/cli/latest/userguide/) and the [AWS CLI Reference for AWS CodeBuild](http://docs.aws.amazon.com/cli/latest/reference/codebuild/)\.

Not what you're looking for? If you want to use the AWS SDKs to call AWS CodeBuild, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.

To use the information in this topic, you should have already installed the AWS CLI and configured it for use with AWS CodeBuild, as described in [Install and Configure the AWS CLI](setting-up.md#setting-up-cli)\.

Run this command to get a list of AWS CodeBuild commands\.

```
aws codebuild help
```

Run this command to get information about a AWS CodeBuild command, where *command\-name* is the name of the command\.

```
aws codebuild command-name help
```

AWS CodeBuild commands include:

+ `batch-delete-builds`: Deletes one or more builds in AWS CodeBuild\. For more information, see [Delete Builds \(AWS CLI\)](delete-builds.md#delete-builds-cli)\.

+ `batch-get-builds`: Gets information about multiple builds in AWS CodeBuild\. For more information, see [View Build Details \(AWS CLI\)](view-build-details.md#view-build-details-cli)\.

+ `batch-get-projects`: Gets information about one or more specified build projects\. For more information, see [View a Build Project's Details \(AWS CLI\)](view-project-details.md#view-project-details-cli)\.

+ `create-project`: Creates a build project\. For more information, see [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli)\.

+ `delete-project`: Deletes a build project\. For more information, see [Delete a Build Project \(AWS CLI\)](delete-project.md#delete-project-cli)\.

+ `list-builds`: Lists Amazon Resource Names \(ARNs\) for builds in AWS CodeBuild\. For more information, see [View a List of Build IDs \(AWS CLI\)](view-build-list.md#view-build-list-cli)\.

+ `list-builds-for-project`: Gets a list of build IDs that are associated with a specified build project\. For more information, see [View a List of Build IDs for a Build Project \(AWS CLI\)](view-builds-for-project.md#view-builds-for-project-cli)\.

+ `list-curated-environment-images`: Gets a list of Docker images managed by AWS CodeBuild that you can use for your builds\. For more information, see [Docker Images Provided by AWS CodeBuild](build-env-ref-available.md)\.

+ `list-projects`: Gets a list of build project names\. For more information, see [View a List of Build Project Names \(AWS CLI\)](view-project-list.md#view-project-list-cli)\.

+ `start-build`: Starts running a build\. For more information, see [Run a Build \(AWS CLI\)](run-build.md#run-build-cli)\.

+ `stop-build`: Attempts to stop the specified build from running\. For more information, see [Stop a Build \(AWS CLI\)](stop-build.md#stop-build-cli)\.

+ `update-project`: Changes information about the specified build project\. For more information, see [Change a Build Project's Settings \(AWS CLI\)](change-project.md#change-project-cli)\.