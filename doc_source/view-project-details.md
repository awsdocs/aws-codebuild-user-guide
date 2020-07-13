# View a build project's details in AWS CodeBuild<a name="view-project-details"></a>

You can use the AWS CodeBuild console, AWS CLI, or AWS SDKs to view the details of a build project in CodeBuild\.

**Topics**
+ [View a build project's details \(console\)](#view-project-details-console)
+ [View a build project's details \(AWS CLI\)](#view-project-details-cli)
+ [View a build project's details \(AWS SDKs\)](#view-project-details-sdks)

## View a build project's details \(console\)<a name="view-project-details-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. In the navigation pane, choose **Build projects**\.
**Note**  
By default, only the 10 most recent build projects are displayed\. To view more build projects, choose the gear icon, and then choose a different value for **Projects per page** or use the back and forward arrows\.

1. In the list of build projects, in the **Name** column, choose the link for the build project\.

1. On the **Build project: *project\-name*** page, choose **Build details**\.

## View a build project's details \(AWS CLI\)<a name="view-project-details-cli"></a>

Run the batch\-get\-projects command:

```
aws codebuild batch-get-projects --names names
```

In the preceding command, replace the following placeholder:
+ *names*: Required string used to indicate one or more build project names to view details about\. To specify more than one build project, separate each build project's name with a space\. You can specify up to 100 build project names\. To get a list of build projects, see [View a list of build project names \(AWS CLI\)](view-project-list.md#view-project-list-cli)\.

For example, if you run this command:

```
aws codebuild batch-get-projects --names codebuild-demo-project codebuild-demo-project2 my-other-demo-project
```

A result similar to the following might appear in the output\. Ellipses \(`...`\) are used to represent data omitted for brevity\.

```
{
  "projectsNotFound": [
    "my-other-demo-project"
  ],
  "projects": [
    {
      ...
      "name": codebuild-demo-project,
      ...
    },
    {
      ...
      "name": codebuild-demo-project2",
      ...
    }
  ]
}
```

In the preceding output, the `projectsNotFound` array lists any build project names that were specified, but not found\. The `projects` array lists details for each build project where information was found\. Build project details have been omitted from the preceding output for brevity\. For more information, see the output of [Create a build project \(AWS CLI\)](create-project-cli.md)\.

For more information about using the AWS CLI with AWS CodeBuild, see the [Command line reference](cmd-ref.md)\.

## View a build project's details \(AWS SDKs\)<a name="view-project-details-sdks"></a>

For more information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and tools reference](sdk-ref.md)\.