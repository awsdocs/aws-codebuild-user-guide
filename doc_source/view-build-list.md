# View a list of build IDs in AWS CodeBuild<a name="view-build-list"></a>

You can use the AWS CodeBuild console, AWS CLI, or AWS SDKs to view a list of build IDs for builds managed by CodeBuild\.

**Topics**
+ [View a list of build IDs \(console\)](#view-build-list-console)
+ [View a list of build IDs \(AWS CLI\)](#view-build-list-cli)
+ [View a list of batch build IDs \(AWS CLI\)](#view-batch-build-list-cli)
+ [View a list of build IDs \(AWS SDKs\)](#view-build-list-sdks)

## View a list of build IDs \(console\)<a name="view-build-list-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. In the navigation pane, choose **Build history**\. 
**Note**  
By default, only the 10 most recent builds are displayed\. To view more builds, choose the gear icon, and then choose a different value for **Builds per page** or use the back and forward arrows\.

## View a list of build IDs \(AWS CLI\)<a name="view-build-list-cli"></a>

For more information about using the AWS CLI with CodeBuild, see the [Command line reference](cmd-ref.md)\.
+ Run the list\-builds command:

  ```
  aws codebuild list-builds --sort-order sort-order --next-token next-token
  ```

  In the preceding command, replace the following placeholders:
  + *sort\-order*: Optional string used to indicate how to list the build IDs\. Valid values include `ASCENDING` and `DESCENDING`\.
  + *next\-token*: Optional string\. During a previous run, if there were more than 100 items in the list, only the first 100 items are returned, along with a unique string called *next token*\. To get the next batch of items in the list, run this command again, adding the next token to the call\. To get all of the items in the list, keep running this command with each subsequent next token, until no more next tokens are returned\.

  For example, if you run this command:

  ```
  aws codebuild list-builds --sort-order ASCENDING
  ```

  A result similar to the following might appear in the output:

  ```
  {
    "nextToken": "4AEA6u7J...The full token has been omitted for brevity...MzY2OA==",
    "ids": [
      "codebuild-demo-project:815e755f-bade-4a7e-80f0-efe51EXAMPLE"
      "codebuild-demo-project:84a7f3d1-d40e-4956-b4cf-7a9d4EXAMPLE"
      ... The full list of build IDs has been omitted for brevity ...
      "codebuild-demo-project:931d0b72-bf6f-4040-a472-5c707EXAMPLE"
    ]
  }
  ```

  If you run this command again:

  ```
  aws codebuild list-builds --sort-order ASCENDING --next-token 4AEA6u7J...The full token has been omitted for brevity...MzY2OA==
  ```

  A result similar to the following might appear in the output:

  ```
  {
    "ids": [
      "codebuild-demo-project:49015049-21cf-4b50-9708-df115EXAMPLE",
      "codebuild-demo-project:543e7206-68a3-46d6-a4da-759abEXAMPLE",
      ... The full list of build IDs has been omitted for brevity ...
      "codebuild-demo-project:c282f198-4582-4b38-bdc0-26f96EXAMPLE"
    ]
  }
  ```

## View a list of batch build IDs \(AWS CLI\)<a name="view-batch-build-list-cli"></a>

For more information about using the AWS CLI with CodeBuild, see the [Command line reference](cmd-ref.md)\.
+ Run the list\-build\-batches command:

  ```
  aws codebuild list-build-batches --sort-order sort-order --next-token next-token
  ```

  In the preceding command, replace the following placeholders:
  + *sort\-order*: Optional string used to indicate how to list the batch build IDs\. Valid values include `ASCENDING` and `DESCENDING`\.
  + *next\-token*: Optional string\. During a previous run, if there were more than 100 items in the list, only the first 100 items are returned, along with a unique string called *next token*\. To get the next batch of items in the list, run this command again, adding the next token to the call\. To get all of the items in the list, keep running this command with each subsequent next token, until no more next tokens are returned\.

  For example, if you run this command:

  ```
  aws codebuild list-build-batches --sort-order ASCENDING
  ```

  A result similar to the following might appear in the output:

  ```
  {
    "nextToken": "4AEA6u7J...The full token has been omitted for brevity...MzY2OA==",
    "ids": [
      "codebuild-demo-project:815e755f-bade-4a7e-80f0-efe51EXAMPLE"
      "codebuild-demo-project:84a7f3d1-d40e-4956-b4cf-7a9d4EXAMPLE"
      ... The full list of build IDs has been omitted for brevity ...
      "codebuild-demo-project:931d0b72-bf6f-4040-a472-5c707EXAMPLE"
    ]
  }
  ```

  If you run this command again:

  ```
  aws codebuild list-build-batches --sort-order ASCENDING --next-token 4AEA6u7J...The full token has been omitted for brevity...MzY2OA==
  ```

  A result similar to the following might appear in the output:

  ```
  {
    "ids": [
      "codebuild-demo-project:49015049-21cf-4b50-9708-df115EXAMPLE",
      "codebuild-demo-project:543e7206-68a3-46d6-a4da-759abEXAMPLE",
      ... The full list of build IDs has been omitted for brevity ...
      "codebuild-demo-project:c282f198-4582-4b38-bdc0-26f96EXAMPLE"
    ]
  }
  ```

## View a list of build IDs \(AWS SDKs\)<a name="view-build-list-sdks"></a>

For more information about using CodeBuild with the AWS SDKs, see the [AWS SDKs and tools reference](sdk-ref.md)\.