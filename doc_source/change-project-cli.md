# Change a build project's settings \(AWS CLI\)<a name="change-project-cli"></a>

For information about using the AWS CLI with AWS CodeBuild, see the [Command line reference](cmd-ref.md)\.

To update a CodeBuild project with the AWS CLI, you create a JSON file with the updated properties and pass that file to the [https://docs.aws.amazon.com/cli/latest/reference/codebuild/update-project.html](https://docs.aws.amazon.com/cli/latest/reference/codebuild/update-project.html) command\. Any properties not contained in the update file remain unchanged\.

In the update JSON file, only the `name` property and the modified properties are required\. The `name` property identifies the project to modify\. For any modified structures, the required parameters for those structures must also be included\. For example, to modify the environment for the project, the `environment/type` and `environment/computeType` properties are required\. Here is an example that updates the environment image:

```
{
  "name": "<project-name>",
  "environment": {
    "type": "LINUX_CONTAINER",
    "computeType": "BUILD_GENERAL1_SMALL",
    "image": "aws/codebuild/amazonlinux2-x86_64-standard:3.0"
  }
}
```

If you need to obtain the current property values for a project, use the [https://docs.aws.amazon.com/cli/latest/reference/codebuild/batch-get-projects.html](https://docs.aws.amazon.com/cli/latest/reference/codebuild/batch-get-projects.html) command to obtain the current properties of the project you are modifying, and write the output to a file\.

```
aws codebuild batch-get-projects --names "<project-name>" > project-info.json
```

The *project\-info\.json* file contains an array of projects, so it cannot be used directly to update a project\. You can, however, copy the properties that you want to modify from the *project\-info\.json* file and paste them into your update file as a baseline for the properties you want to modify\. For more information, see [View a build project's details \(AWS CLI\)](view-project-details.md#view-project-details-cli)\.

Modify the update JSON file as described in [Create a build project \(AWS CLI\)](create-project-cli.md), and save your results\. When you are finished modifying the update JSON file, run the [https://docs.aws.amazon.com/cli/latest/reference/codebuild/update-project.html](https://docs.aws.amazon.com/cli/latest/reference/codebuild/update-project.html) command, passing the update JSON file\.

```
aws codebuild update-project --cli-input-json file://<update-project-file>
```

If successful, the updated project JSON appears in the output\. If any required parameters are missing, an error message is displayed in the output that identifies the missing parameters\. For example, this is the error message displayed if the `environment/type` parameter is missing:

```
aws codebuild update-project --cli-input-json file://update-project.json

Parameter validation failed:
Missing required parameter in environment: "type"
```