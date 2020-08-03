# View a list of build IDs for a build project in AWS CodeBuild<a name="view-builds-for-project"></a>

You can use the AWS CodeBuild console, AWS CLI, or AWS SDKs to view a list of build IDs for a build project in CodeBuild\.

**Topics**
+ [View a list of build IDs for a build project \(console\)](#view-builds-for-project-console)
+ [View a list of build IDs for a build project \(AWS CLI\)](#view-builds-for-project-cli)
+ [View a list of batch build IDs for a build project \(AWS CLI\)](#view-batch-builds-for-project-cli)
+ [View a list of build IDs for a build project \(AWS SDKs\)](#view-builds-for-project-sdks)

## View a list of build IDs for a build project \(console\)<a name="view-builds-for-project-console"></a>

1. Open the CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. In the navigation pane, choose **Build projects**\. In the list of build projects, in the **Name** column, choose the build project\. 

**Note**  
By default, only the most recent 100 builds or build projects are displayed\. To view more builds or build projects, choose the gear icon, and then choose a different value for **Builds per page** or **Projects per page** or use the back and forward arrows\.

## View a list of build IDs for a build project \(AWS CLI\)<a name="view-builds-for-project-cli"></a>

For more information about using the AWS CLI with AWS CodeBuild, see the [Command line reference](cmd-ref.md)\.

Run the list\-builds\-for\-project command, as follows:

```
aws codebuild list-builds-for-project --project-name project-name --sort-order sort-order --next-token next-token
```

In the preceding command, replace the following placeholders:
+ *project\-name*: Required string used to indicate the name of the build project to list builds IDs for\. To get a list of build projects, see [View a list of build project names \(AWS CLI\)](view-project-list.md#view-project-list-cli)\.
+ *sort\-order*: Optional string used to indicate how to list the build IDs\. Valid values include `ASCENDING` and `DESCENDING`\.
+ *next\-token*: Optional string\. During a previous run, if there were more than 100 items in the list, only the first 100 items are returned, along with a unique string called *next token*\. To get the next batch of items in the list, run this command again, adding the next token to the call\. To get all of the items in the list, keep running this command with each subsequent next token that is returned, until no more next tokens are returned\.

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

You might see a result like the following in the output:

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

## View a list of batch build IDs for a build project \(AWS CLI\)<a name="view-batch-builds-for-project-cli"></a>

For more information about using the AWS CLI with AWS CodeBuild, see the [Command line reference](cmd-ref.md)\.

Run the list\-build\-batches\-for\-project command, as follows:

```
aws codebuild list-build-batches-for-project --project-name project-name --sort-order sort-order --next-token next-token
```

In the preceding command, replace the following placeholders:
+ *project\-name*: Required string used to indicate the name of the build project to list builds IDs for\. To get a list of build projects, see [View a list of build project names \(AWS CLI\)](view-project-list.md#view-project-list-cli)\.
+ *sort\-order*: Optional string used to indicate how to list the build IDs\. Valid values include `ASCENDING` and `DESCENDING`\.
+ *next\-token*: Optional string\. During a previous run, if there were more than 100 items in the list, only the first 100 items are returned, along with a unique string called *next token*\. To get the next batch of items in the list, run this command again, adding the next token to the call\. To get all of the items in the list, keep running this command with each subsequent next token that is returned, until no more next tokens are returned\.

For example, if you run this command similar to this:

```
aws codebuild list-build-batches-for-project --project-name codebuild-demo-project --sort-order ASCENDING
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
aws codebuild list-build-batches-for-project --project-name codebuild-demo-project --sort-order ASCENDING --next-token 4AEA6u7J...The full token has been omitted for brevity...MzY2OA==
```

You might see a result like the following in the output:

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

## View a list of build IDs for a build project \(AWS SDKs\)<a name="view-builds-for-project-sdks"></a>

For more information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and tools reference](sdk-ref.md)\.