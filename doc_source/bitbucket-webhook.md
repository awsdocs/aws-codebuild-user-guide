# Bitbucket webhook events<a name="bitbucket-webhook"></a>

 You can use webhook filter groups to specify which Bitbucket webhook events trigger a build\. For example, you can specify that a build is triggered for specified branches only\. 

 You can specify more than one webhook filter group\. A build is triggered if the filters on one or more filter groups evaluate to true\. When you create a filter group, you specify: 

**An event**  
For Bitbucket, you can choose one or more of the following events: `PUSH`, `PULL_REQUEST_CREATED`, `PULL_REQUEST_UPDATED`, and `PULL_REQUEST_MERGED`\. The webhook's event type is in its header in the `X-Event-Key` field\. The following table shows how `X-Event-Key` header values map to the event types\.  
You must enable the `merged` event in your Bitbucket webhook setting if you create a webhook filter group that uses the `PULL_REQUEST_MERGED` event type\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/bitbucket-webhook.html)

**One or more optional filters**  
Use a regular expression to specify a filter\. For an event to trigger a build, every filter associated with it must evaluate to true\.     
`ACTOR_ACCOUNT_ID` \(`ACTOR_ID` in the console\)  
A webhook event triggers a build when a Bitbucket account ID matches the regular expression pattern\. This value appears in the `account_id` property of the `actor` object in the webhook filter payload\.  
`HEAD_REF`  
A webhook event triggers a build when the head reference matches the regular expression pattern \(for example, `refs/heads/branch-name` and `refs/tags/tag-name`\)\. A `HEAD_REF` filter evaluates the Git reference name for the branch or tag\. The branch or tag name appears in the `name` field of the `new` object in the `push` object of the webhook payload\. For pull request events, the branch name appears in the `name` field in the `branch` object of the `source` object in the webhook payload\.  
`BASE_REF`  
A webhook event triggers a build when the base reference matches the regular expression pattern\. A `BASE_REF` filter works with pull request events only \(for example, `refs/heads/branch-name`\)\. A `BASE_REF` filter evaluates the Git reference name for the branch\. The branch name appears in the `name` field of the `branch` object in the `destination` object in the webhook payload\.  
`FILE_PATH`  
A webhook triggers a build when the path of a changed file matches the regular expression pattern\.  
`COMMIT_MESSAGE`  
A webhook triggers a build when the head commit message matches the regular expression pattern\.

**Note**  
You can find the webhook payload in the webhook settings of your Bitbucket repository\. 

**Topics**
+ [Filter Bitbucket webhook events \(console\)](#bitbucket-webhook-events-console)
+ [Filter Bitbucket webhook events \(SDK\)](#bitbucket-webhook-events-sdk)
+ [Filter Bitbucket webhook events \(AWS CloudFormation\)](#bitbucket-webhook-events-cfn)

## Filter Bitbucket webhook events \(console\)<a name="bitbucket-webhook-events-console"></a>

 To use the AWS Management Console to filter webhook events: 

1.  Select **Rebuild every time a code change is pushed to this repository** when you create your project\. 

1.  From **Event type**, choose one or more events\. 

1.  To filter when an event triggers a build, under **Start a build under these conditions**, add one or more optional filters\. 

1.  To filter when an event is not triggered, under **Don't start a build under these conditions**, add one or more optional filters\. 

1.  Choose **Add filter group** to add another filter group\. 

 For more information, see [Create a build project \(console\)](create-project-console.md) and [WebhookFilter](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_WebhookFilter.html) in the *AWS CodeBuild API Reference*\. 

In this example, a webhook filter group triggers a build for pull requests only:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter-bitbucket.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

Using an example of two filter groups, a build is triggered when one or both evaluate to true:
+ The first filter group specifies pull requests that are created or updated on branches with Git reference names that match the regular expression `^refs/heads/main$` and head references that match `^refs/heads/branch1!`\. 
+ The second filter group specifies push requests on branches with Git reference names that match the regular expression `^refs/heads/branch1$`\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter-head-base-regexes-bitbucket.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

In this example, a webhook filter group triggers a build for all requests except tag events\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter-exclude-bitbucket.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

In this example, a webhook filter group triggers a build only when files with names that match the regular expression `^buildspec.*` change\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter-file-name-regex.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

In this example, a webhook filter group triggers a build only when a change is made by a Bitbucket user who does not have an account ID that matches the regular expression `actor-account-id`\. 

**Note**  
 For information about how to find your Bitbucket account ID, see https://api\.bitbucket\.org/2\.0/users/*user\-name*, where *user\-name* is your Bitbucket user name\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter-actor-bitbucket.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

In this example, a webhook filter group triggers a build for a push event when the head commit message matches the regular expression `\[CodeBuild\]`\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter-commit-message.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

## Filter Bitbucket webhook events \(SDK\)<a name="bitbucket-webhook-events-sdk"></a>

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

 To create a webhook filter that triggers a build for specified branches only, use the `pattern` parameter to specify a regular expression to filter branch names\. Using an example of two filter groups, a build is triggered when one or both evaluate to true:
+ The first filter group specifies pull requests that are created or updated on branches with Git reference names that match the regular expression `^refs/heads/main$` and head references that match `^refs/heads/myBranch$`\. 
+ The second filter group specifies push requests on branches with Git reference names that match the regular expression `^refs/heads/myBranch$`\. 

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
      "pattern": "^refs/heads/main$"
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

You can create a filter that triggers a build only when files with names that match the regular expression in the `pattern` argument change\. In this example, the filter group specifies that a build is triggered only when files with a name that matches the regular expression `^buildspec.*` change\. 

```
"filterGroups": [
  [
    {
      "type": "EVENT",
      "pattern": "PUSH"
    },
    {
      "type": "FILE_PATH",
      "pattern": "^buildspec.*"
    }
  ]
]
```

You can create a filter that triggers a build only when the head commit message matches the regular expression in the pattern argument\. In this example, the filter group specifies that a build is triggered only when the head commit message of the push event matches the regular expression `\[CodeBuild\]`\. 

```
  "filterGroups": [
    [
      {
        "type": "EVENT",
        "pattern": "PUSH"
      },
      {
        "type": "COMMIT_MESSAGE",
        "pattern": "\[CodeBuild\]"
      }
    ]
  ]
```

## Filter Bitbucket webhook events \(AWS CloudFormation\)<a name="bitbucket-webhook-events-cfn"></a>

 To use an AWS CloudFormation template to filter webhook events, use the AWS CodeBuild project's `FilterGroups` property\. The following YAML\-formatted portion of an AWS CloudFormation template creates two filter groups\. Together, they trigger a build when one or both evaluate to true: 
+  The first filter group specifies pull requests are created or updated on branches with Git reference names that match the regular expression `^refs/heads/main$` by a Bitbucket user who does not have account ID `12345`\. 
+  The second filter group specifies push requests are created on branches with Git reference names that match the regular expression `^refs/heads/.*`\. 
+ The third filter group specifies a push request with a head commit message matching the regular expression `\[CodeBuild\]`\.

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
      Image: aws/codebuild/standard:4.0
    Source:
      Type: BITBUCKET
      Location: source-location
    Triggers:
      Webhook: true
      FilterGroups:
        - - Type: EVENT
            Pattern: PULL_REQUEST_CREATED,PULL_REQUEST_UPDATED
          - Type: BASE_REF
            Pattern: ^refs/heads/main$
            ExcludeMatchedPattern: false
          - Type: ACTOR_ACCOUNT_ID
            Pattern: 12345
            ExcludeMatchedPattern: true
        - - Type: EVENT
            Pattern: PUSH
          - Type: HEAD_REF
            Pattern: ^refs/heads/.*
        - - Type: EVENT
            Pattern: PUSH
          - Type: COMMIT_MESSAGE
          - Pattern: \[CodeBuild\]
```