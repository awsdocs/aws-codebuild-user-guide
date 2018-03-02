# View a List of Build Project Names in AWS CodeBuild<a name="view-project-list"></a>

To view a list of build projects in AWS CodeBuild, you can use the AWS CodeBuild console, AWS CLI, or AWS SDKs\.


+ [View a List of Build Project Names \(Console\)](#view-project-list-console)
+ [View a List of Build Project Names \(AWS CLI\)](#view-project-list-cli)
+ [View a List of Build Project Names \(AWS SDKs\)](#view-project-list-sdks)

## View a List of Build Project Names \(Console\)<a name="view-project-list-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. In the navigation pane, choose **Build projects**\.
**Note**  
By default, only the ten most recent build projects are displayed\. To view more build projects, select a different value for **Projects per page** or select the back and forward arrows for **Viewing projects**\.

## View a List of Build Project Names \(AWS CLI\)<a name="view-project-list-cli"></a>

Run the `list-projects` command:

```
aws codebuild list-projects --sort-by sort-by --sort-order sort-order --next-token next-token
```

In the preceding command, replace the following placeholders:

+ *sort\-by*: Optional string\. The criterion to be used to list build project names\. Valid values include:

  + `CREATED_TIME`: List the build project names based on when each build project was created\. 

  + `LAST_MODIFIED_TIME`: List the build project names based on when information about each build project was last changed\. 

  + `NAME`: List the build project names based on each build project's name\.

+ *sort\-order*: Optional string\. The order in which to list build projects, based on *sort\-by*\. Valid values include `ASCENDING` and `DESCENDING`\.

+ *next\-token*: Optional string\. During a previous run, if there were more than 100 items in the list, only the first 100 items are returned, along with a unique string called a *next token*\. To get the next batch of items in the list, run this command again, adding the next token to the call\. To get all of the items in the list, keep running this command with each subsequent next token, until no more next tokens are returned\.

For example, if you run this command:

```
aws codebuild list-projects --sort-by NAME --sort-order ASCENDING
```

A result similar to the following might appear in the output:

```
{
  "nextToken": "Ci33ACF6...The full token has been omitted for brevity...U+AkMx8=",
  "projects": [
    "codebuild-demo-project",
    "codebuild-demo-project2",
    ... The full list of build project names has been omitted for brevity ...
    "codebuild-demo-project99"
  ]
}
```

If you run this command again:

```
aws codebuild list-projects  --sort-by NAME --sort-order ASCENDING --next-token Ci33ACF6...The full token has been omitted for brevity...U+AkMx8=
```

A result similar to the following might appear in the output:

```
{
  "projects": [
    "codebuild-demo-project100",
    "codebuild-demo-project101",
    ... The full list of build project names has been omitted for brevity ...
    "codebuild-demo-project122"
  ]
}
```

## View a List of Build Project Names \(AWS SDKs\)<a name="view-project-list-sdks"></a>

For more information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.