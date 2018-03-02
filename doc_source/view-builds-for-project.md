# View a List of Build IDs for a Build Project in AWS CodeBuild<a name="view-builds-for-project"></a>

You can use the AWS CodeBuild console, AWS CLI, or AWS SDKs to view a list of build IDs for a build project in AWS CodeBuild\.


+ [View a List of Build IDs for a Build Project \(Console\)](#view-builds-for-project-console)
+ [View a List of Build IDs for a Build Project \(AWS CLI\)](#view-builds-for-project-cli)
+ [View a List of Build IDs for a Build Project \(AWS SDKs\)](#view-builds-for-project-sdks)

## View a List of Build IDs for a Build Project \(Console\)<a name="view-builds-for-project-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. In the navigation pane, choose **Build projects**\. In the list of build projects, in the **Project** column, choose the build project\. 

**Note**  
By default, only the ten most recent builds or build projects are displayed\. To view more builds or build projects, select a different value for **Builds per page** or **Projects per page** or select the back and forward arrows for **Viewing builds** or **Viewing projects**\.

## View a List of Build IDs for a Build Project \(AWS CLI\)<a name="view-builds-for-project-cli"></a>

For more information about using the AWS CLI with AWS CodeBuild, see the [Command Line Reference](cmd-ref.md)\.

Run the `list-builds-for-project` command, as follows:

```
aws codebuild list-builds-for-project --project-name project-name --sort-order sort-order --next-token next-token
```

In the preceding command, replace the following placeholders:

+ *project\-name*: Required string\. The name of the build project to list builds IDs for\. To get a list of build projects, see [View a List of Build Project Names \(AWS CLI\)](view-project-list.md#view-project-list-cli)\.

+ *sort\-order*: Optional string\. How to list the build IDs\. Valid values include `ASCENDING` and `DESCENDING`\.

+ *next\-token*: Optional string\. During a previous run, if there were more than 100 items in the list, only the first 100 items are returned, along with a unique string called a *next token*\. To get the next batch of items in the list, run this command again, adding the next token to the call\. To get all of the items in the list, keep running this command with each subsequent next token that is returned, until no more next tokens are returned\.

For example, if you run this command similar to this:

```
aws codebuild list-builds-for-project --project-name codebuild-demo-project --sort-order ASCENDING
```

A result like the following might appear in the output:

```
{
  "nextToken": "4AEA6u7J...The full token has been omitted for brevity...MzY2OA==",
  "ids": [
    "codebuild-demo-project:9b175d16-66fd-4e71-93a0-50a08EXAMPLE"
    "codebuild-demo-project:a9d1bd09-18a2-456b-8a36-7d65aEXAMPLE"
    ... The full list of build IDs has been omitted for brevity ...
    "codebuild-demo-project:fe70d102-c04f-421a-9cfa-2dc15EXAMPLE"
  ]
}
```

If you run this command again:

```
aws codebuild list-builds-for-project --project-name codebuild-demo-project --sort-order ASCENDING --next-token 4AEA6u7J...The full token has been omitted for brevity...MzY2OA==
```

A result like the following might be output:

```
{
  "ids": [
    "codebuild-demo-project:98253670-7a8a-4546-b908-dc890EXAMPLE"
    "codebuild-demo-project:ad5405b2-1ab3-44df-ae2d-fba84EXAMPLE"
    ... The full list of build IDs has been omitted for brevity ...
    "codebuild-demo-project:f721a282-380f-4b08-850a-e0ac1EXAMPLE"
  ]
}
```

## View a List of Build IDs for a Build Project \(AWS SDKs\)<a name="view-builds-for-project-sdks"></a>

For more information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.