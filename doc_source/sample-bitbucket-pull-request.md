# Bitbucket Pull Request and Webhook Filter Sample for CodeBuild<a name="sample-bitbucket-pull-request"></a>

This sample shows you how to create a pull request using a Bitbucket repository\. It also shows you how to use a Bitbucket webhook to trigger CodeBuild to create a build of a project\.

**Topics**
+ [Bitbucket Pull Request Prerequisites](#sample-bitbucket-pull-request-prerequisites)
+ [Create a Build Project with Bitbucket as the Source Repository and Enable Webhooks](#sample-bitbucket-pull-request-create)
+ [Trigger a Build with a Bitbucket Webhook](#sample-bitbucket-pull-request-trigger)
+ [Filter Bitbucket Webhook Events](#sample-bitbucket-pull-request-filter-webhook-events)

## Bitbucket Pull Request Prerequisites<a name="sample-bitbucket-pull-request-prerequisites"></a>

 To run this sample you must connect your AWS CodeBuild project with your Bitbucket account\. 

**Note**  
 CodeBuild has updated its permissions with Bitbucket\. If you previously connected your project to Bitbucket and now receive a Bitbucket connection error, you must reconnect to grant CodeBuild permission to manage your webhooks\. 

## Create a Build Project with Bitbucket as the Source Repository and Enable Webhooks<a name="sample-bitbucket-pull-request-create"></a>

 The following steps describe how to create an AWS CodeBuild project with Bitbucket as a source repository and enable webhooks\. 

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  If a CodeBuild information page is displayed, choose **Create build project**\. Otherwise, on the navigation pane, expand **Build**, choose **Build projects**, and then choose **Create build project**\. 

1. On the **Create build project** page, in **Project configuration**, for **Project name**, enter a name for this build project\. Build project names must be unique across each AWS account\. You can also include an optional description of the build project to help other users understand what this project is used for\.

1.  In **Source**, for **Source provider**, choose **Bitbucket**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/bitbucket-pr-sample-source.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

    Follow the instructions to connect or reconnect, and then choose **Grant access**\. 
**Note**  
CodeBuild does not support Bitbucket Server\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/bitbucket-webhook-prerequisite.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1.  Choose **Use a repository in my account**\. You cannot use a webhook if you use a public Bitbucket repository\. 

1. In **Primary source webhook events** select **Rebuild every time a code change is pushed to this repository**\. You can select this check box only if you chose **Repository in my Bitbucket account**\.
**Note**  
 If a build is triggered by a Bitbucket webhook, the **Report build status** setting is ignored\. The build status is always sent to Bitbucket\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/github-pr-webhook.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1.  Choose other settings for your project\. For more information about source provider options and settings, see [Choose source provider](create-project.md#create-project-source-provider)\. 

1. Choose **Create build project**\. On the **Review** page, choose **Start build** to run the build\.

## Trigger a Build with a Bitbucket Webhook<a name="sample-bitbucket-pull-request-trigger"></a>

 For a project that uses Bitbucket webhooks, AWS CodeBuild creates a build when the Bitbucket repository detects a change in your source code\. 

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. On the navigation pane, choose **Build projects**, and then choose a project associated with a Bitbucket repository with webhooks\. For information about creating a Bitbucket webhook project, see [Create a Build Project with Bitbucket as the Source Repository and Enable Webhooks](#sample-bitbucket-pull-request-create)\. 

1.  Make some changes in the code in your project's Bitbucket repository\. 

1.  Create a pull request on your Bitbucket repository\. For more information, see [Making a Pull Request](https://www.atlassian.com/git/tutorials/making-a-pull-request)\. 

1.  On the Bitbucket webhooks page, choose **View request** to see a list of recent events\. 

1.  Choose **View details** to see details about the response returned by CodeBuild\. It might look something like this: 

   ```
   "response":"Webhook received and buld started: https://us-east-1.console.aws.amazon.com/codebuild/home..."
   "statusCode":200
   ```

1.  Navigate to the Bitbucket pull request page to see the status of the build\. 

## Filter Bitbucket Webhook Events<a name="sample-bitbucket-pull-request-filter-webhook-events"></a>

 You can use webhook filter groups to specify which Bitbucket webhook events trigger a build\. For example, you can specify that a build is triggered for specified branches only\. 

 You can specify more than one webhook filter group\. A build is triggered if the filters on one or more filter groups evaluate to true\. When you create a filter group, you specify: 
+  An event\. For Bitbucket, you can choose one or more of the following events: `PUSH`, `PULL_REQUEST_CREATED`, `PULL_REQUEST_UPDATED`, and `PULL_REQUEST_MERGED`\. The webhook's event type is in its header in the `X-Event-Key` field\. The following table shows how `X-Event-Key` header values map to the event types\.
**Note**  
You must enable the `merged` event in your Bitbucket webhook setting if you create a webhook filter group that uses the `PULL_REQUEST_MERGED` event type\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/sample-bitbucket-pull-request.html)
+  One or more optional filters\. Use a regular expression to specify a filter\. For an event to trigger a build, every filter associated with it must evaluate to true\. 
  +  `ACTOR_ACCOUNT_ID` \(`ACTOR_ID` in the console\): A webhook event triggers a build when a Bitbucket account ID matches the regular expression pattern\. This value is found in the `account_id` property of the `actor` object in the webhook filter payload\.
  +  `HEAD_REF`: A webhook event triggers a build when the head reference matches the regular expression pattern \(for example, `refs/heads/branch-name` and `refs/tags/tag-name`\)\. A `HEAD_REF` filter evaluates the Git reference name for the branch or tag\. The branch or tag name is found in the `name` field of the `new` object in the `push` object of the webhook payload\. For pull request events, the branch name is found in the `name` field in the `branch` object of the `source` object in the webhook payload\.
  +  `BASE_REF`: A webhook event triggers a build when the base reference matches the regular expression pattern\. A `BASE_REF` filter works with pull request events only \(for example, `refs/heads/branch-name`\)\. A `BASE_REF` filter evaluates the Git reference name for the branch\. The branch name is found in the `name` field of the `branch` object in the `destination` object in the webhook payload\.

**Note**  
 You can find the webhook payload in the webhook settings of your Bitbucket repository\. 

**Topics**
+ [Filter BitBucket Webhook Events \(Console\)](#sample-bitbucket-pull-request-filter-webhook-events-console)
+ [Filter BitBucket Webhook Events \(SDK\)](#sample-bitbucket-pull-request-filter-webhook-events-sdk)
+ [Filter Bitbucket Webhook Events \(AWS CloudFormation\)](#sample-bitbucket-pull-request-filter-webhook-events-cfn)

### Filter BitBucket Webhook Events \(Console\)<a name="sample-bitbucket-pull-request-filter-webhook-events-console"></a>

 To use the AWS Management Console to filter webhook events: 

1.  Select **Rebuild every time a code change is pushed to this repository** when you create your project\. 

1.  From **Event type**, choose one or more events\. 

1.  To filter when an event triggers a build, under **Start a build under these conditions**, add one or more optional filters\. 

1.  To filter when an event is not triggered, under **Don't start a build under these conditions**, add one or more optional filters\. 

1.  Choose **Add filter group** to add another filter group\. 

 For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console) and [WebhookFilter](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_WebhookFilter.html) in the *AWS CodeBuild API Reference*\. 

In this example, a webhook filter group triggers a build for pull requests only:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter-bitbucket.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

Using an example of two filter groups, a build is triggered when one or both evaluate to true:
+ The first filter group specifies pull requests that are created or updated on branches with Git reference names that match the regular expression `^refs/heads/master$` and head references that matches `^refs/heads/branch1!`\. 
+ The second filter group specifies push requests on branches with Git reference names that match the regular expression `^refs/heads/branch1$`\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter-head-base-regexes-bitbucket.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

In this example, a webhook filter group triggers a build for all requests except tag events\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter-exclude-bitbucket.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

In this example, a webhook filter group triggers a build only when a change is made by a Bitbucket user that does not have an account ID that matches the regular expression `actor-account-id`\. 

**Note**  
 For information about how to find your Bitbucket account ID, see https://api\.bitbucket\.org/2\.0/users/*user\-name*, where *user\-name* is your Bitbucket user name\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter-actor-bitbucket.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

### Filter BitBucket Webhook Events \(SDK\)<a name="sample-bitbucket-pull-request-filter-webhook-events-sdk"></a>

 To use the AWS CodeBuild SDK to filter webhook events, use the `filterGroups` field in the request syntax of the `CreateWebhook` or `UpdateWebhook` API methods\. For more information, see [WebhookFilter](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_WebhookFilter.html) in the *CodeBuild API Reference*\. 

 To create a webhook filter that triggers a build for pull requests only, insert the following into the request syntax: 

```
"filterGroups": [
   [
        {
            "type": "EVENT", 
            "pattern": "PULL_REQUEST_CREATED, PULL_REQUEST_UPDATED, PULL_REQUEST_MERGED"
        }
    ]
]
```

 To create a webhook filter that triggers a build for specified branches only, use the `pattern` parameter to specify a regular expression to filter branch names\. Using an example of two filter groups, a build is triggered when one or both are evaluate to true:
+ The first filter group specifies pull requests that are created or updated on branches with Git reference names that match the regular expression `^refs/heads/master$` and head references that match `^refs/heads/myBranch$`\. 
+ The second filter group specifies push requests on branches with Git reference names that match the regular expression `^refs/heads/myBranch$` and \. 

```
"filterGroups": [
    [
        {
            "type": "EVENT", 
            "pattern": "PULL_REQUEST_CREATED, PULL_REQUEST_UPDATED"
        },
        {
            "type": "HEAD_REF", 
            "pattern": "^refs/heads/myBranch$"
        },
        {
            "type": "BASE_REF", 
            "pattern": "^refs/heads/master$"
        }
    ],
    [
        {
            "type": "EVENT", 
            "pattern": "PUSH"
        },
        {
            "type": "HEAD_REF", 
            "pattern": "^refs/heads/myBranch$"
        }
    ]
]
```

 You can use the `excludeMatchedPattern` parameter to specify which events do not trigger a build\. In this example, a build is triggered for all requests except tag events\. 

```
"filterGroups": [
    [
        {
            "type": "EVENT", 
            "pattern": "PUSH, PULL_REQUEST_CREATED, PULL_REQUEST_UPDATED, PULL_REQUEST_MERGED"
        },
        {
            "type": "HEAD_REF", 
            "pattern": "^refs/tags/.*", 
            "excludeMatchedPattern": true
        }
    ]
]
```

You can create a filter that triggers a build only when a change is made by a Bitbucket user with account ID `actor-account-id`\. 

**Note**  
 For information about how to find your Bitbucket account ID, see https://api\.bitbucket\.org/2\.0/users/*user\-name*, where *user\-name* is your Bitbucket user name\. 

```
"filterGroups": [
    [
        {
            "type": "EVENT", 
            "pattern": "PUSH, PULL_REQUEST_CREATED, PULL_REQUEST_UPDATED, PULL_REQUEST_MERGED"
        },
        {
            "type": "ACTOR_ACCOUNT_ID", 
            "pattern": "actor-account-id"
        }
    ]
]
```

### Filter Bitbucket Webhook Events \(AWS CloudFormation\)<a name="sample-bitbucket-pull-request-filter-webhook-events-cfn"></a>

 To use an AWS CloudFormation template to filter webhook events, use the AWS CodeBuild project's `FilterGroups` property\. The following YAML\-formatted portion of a AWS CloudFormation template creates two filter groups\. Together, they trigger a build when one or both evaluate to true: 
+  The first filter group specifies pull requests are created or updated on branches with Git reference names that match the regular expression `^refs/heads/master$` by a Bitbucket user that does not have account ID `12345`\. 
+  The second filter group specifies push requests are created on branches with Git reference names that match the regular expression `^refs/heads/.*`\. 

```
CodeBuildProject:
  Type: AWS::CodeBuild::Project
  Properties:
    Name: MyProject
    ServiceRole: service-role
    Artifacts:
      Type: NO_ARTIFACTS
    Environment:
      Type: LINUX_CONTAINER
      ComputeType: BUILD_GENERAL1_SMALL
      Image: aws/codebuild/standard:2.0
    Source:
      Type: BITBUCKET
      Location: source-location
    Triggers:
      Webhook: true
      FilterGroups:
        - - Type: EVENT
            Pattern: PULL_REQUEST_CREATED,PULL_REQUEST_UPDATED
          - Type: BASE_REF
            Pattern: ^refs/heads/master$
            ExcludeMatchedPattern: false
          - Type: ACTOR_ACCOUNT_ID
            Pattern: 12345
            ExcludeMatchedPattern: true
        - - Type: EVENT
            Pattern: PUSH
          - Type: HEAD_REF
            Pattern: ^refs/heads/.*
```