# View a Build Project's Details in AWS CodeBuild<a name="view-project-details"></a>

To view the details of a build project in AWS CodeBuild, you can use the AWS CodeBuild console, AWS CLI, or AWS SDKs\.


+ [View a Build Project's Details \(Console\)](#view-project-details-console)
+ [View a Build Project's Details \(AWS CLI\)](#view-project-details-cli)
+ [View a Build Project's Details \(AWS SDKs\)](#view-project-details-sdks)

## View a Build Project's Details \(Console\)<a name="view-project-details-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. In the navigation pane, choose **Build projects**\.
**Note**  
By default, only the ten most recent build projects are displayed\. To view more build projects, select a different value for **Projects per page** or select the back and forward arrows for **Viewing projects**\.

1. In the list of build projects, in the **Project** column, choose the link that corresponds to the build project\.

1. On the **Build project: *project\-name*** page, expand **Project details**\.

## View a Build Project's Details \(AWS CLI\)<a name="view-project-details-cli"></a>

For more information about using the AWS CLI with AWS CodeBuild, see the [Command Line Reference](cmd-ref.md)\.

Run the `batch-get-projects` command:

```
aws codebuild batch-get-projects --names names
```

In the preceding command, replace the following placeholder:

+ *names*: Required string\. One or more build project names to view details about\. To specify more than one build project, separate each build project's name with a space\. You can specify up to 100 build project names\. To get a list of build projects, see [View a List of Build Project Names \(AWS CLI\)](view-project-list.md#view-project-list-cli)\.

For example, if you run this command:

```
aws codebuild batch-get-projects --names codebuild-demo-project codebuild-demo-project2 my-other-demo-project
```

A result similar to the following might appear in the output\. Ellipses \(`...`\) represent data omitted for brevity\.

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

In the preceding output, the `projectsNotFound` array lists any build project names that were specified, but no information was found\. The `projects` array lists details for each build project where information was found\. Build project details have been omitted from the preceding output for brevity\. For more information, see the output of [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli)\.

## View a Build Project's Details \(AWS SDKs\)<a name="view-project-details-sdks"></a>

For more information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.