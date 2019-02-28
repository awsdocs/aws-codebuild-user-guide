# GitHub Pull Request and Webhook Filter Sample for CodeBuild<a name="sample-github-pull-request"></a>

AWS CodeBuild now supports webhooks, when the source repository is GitHub\. This means that for an AWS CodeBuild build project that has its source code stored in a private GitHub repository, webhooks enable AWS CodeBuild to rebuild the source code every time a code change is pushed to the private repository\.

## Create a Build Project with GitHub as the Source Repository and Enable Webhooks \(Console\)<a name="sample-github-pull-request-running"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  If a CodeBuild information page is displayed, choose **Create project**\. Otherwise, on the navigation pane, expand **Build**, and then choose **Build projects**\. 

1.  Choose **Create build project**\. 

1. In **Project configuration**:

   On the **Create build project** page, in **Project configuration**, for **Project name**, enter a name for this build project\. Build project names must be unique across each AWS account\. You can also include an optional description of the build project to help other users understand what this project is used for\.

1. In **Source**, for **Source provider**, choose **GitHub**\. Follow the instructions to connect \(or reconnect\) with GitHub and then choose **Authorize**\.

   Choose **Repository in my GitHub account**\.

   In **GitHub repository**, enter the URL for your GitHub repository\.

   Expand **Additional configuration**\.

   For **Webhook**, select **Rebuild every time a code change is pushed to this repository**\. You can select this check box only if you chose **Repository in my GitHub account**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/webhook.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. In **Environment**:

   For **Environment image**, do one of the following:
   + To use a Docker image managed by AWS CodeBuild, choose **Managed image**, and then make selections from **Operating system**, **Runtime**, and **Runtime version**\.
   + To use another Docker image, choose **Custom image**\. For **Environment type**, choose **Linux** or **Windows**\. For **Custom image type**, choose **Amazon ECR** or **Other location**\. If you choose **Other location**, enter the name and tag of the Docker image in Docker Hub, using the format `docker repository/docker image name`\. If you choose **Amazon ECR**, then use **Amazon ECR repository** and **Amazon ECR image** to choose the Docker image in your AWS account\.
   + To use private Docker image, choose **Custom image**\. For **Environment type**, choose **Linux** or **Windows**\. For **Custom image type**, choose **Other location**, and then enter the Amazon Resource Name \(ARN\) of the credentials for your private Docker image\. The credentials must be created by AWS Secrets Manager\. For more information, see [What Is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/)

1. In **Service role**, do one of the following:
   + If you do not have a CodeBuild service role, choose **New service role**\. In **Role name**, accept the default name or enter your own\.
   + If you have a CodeBuild service role, choose **Existing service role**\. In **Role name**, choose the service role\.
**Note**  
When you use the console to create or update a build project, you can create a CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. For **Buildspec**, do one of the following:
   + Choose **Use a buildspec file** to use the buildspec\.yml file in the source code root directory\.
   + Choose **Insert build commands** to use the console to insert build commands\.

   For more information, see the [Build Spec Reference](build-spec-ref.md)\.

1. In **Artifacts**, for **Type**, do one of the following:
   + If you do not want to create any build output artifacts, choose **No artifacts**\.
   + To store the build output in an Amazon S3 bucket, choose **Amazon S3**, and then do the following:
     + If you want to use your project name for the build output ZIP file or folder, leave **Name** blank\. Otherwise, enter the name\. By default, the artifact name is the project name\. If you want to use a different name, enter it in the artifacts name box\. If you want to output a ZIP file, include the zip extension\.
     + For **Bucket name**, choose the name of the output bucket\.
     + If you chose **Insert build commands** earlier in this procedure, then for **Output files**, enter the locations of the files from the build that you want to put into the build output ZIP file or folder\. For multiple locations, separate each location with a comma \(for example, `appspec.yml, target/my-app.jar`\)\. For more information, see the description of `files` in [Build Spec Syntax](build-spec-ref.md#build-spec-ref-syntax)\.

1. Expand **Additional configuration** and set options as appropriate\.

1. Choose **Create build project**\. On the **Review** page, choose **Start build** to run the build\.

## Verification Checks<a name="verification-checks"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. In the navigation pane, choose **Build projects**\.

1. Do one of the following:
   + Choose the link for the build project with webhooks you want to verify, and then choose **Build details**\.
   + Choose the button next to the build project with webhooks you want to verify, choose **View details**, and then choose **Build details**\.

1. In **Source**, choose the **Webhook** URL link\. 

1. In your GitHub repository, on the **Settings** page, under **Webhooks**, verify that **Pull Requests** and **Pushes** are selected\.

1. In your GitHub profile settings, under **Personal settings**, **Applications**, **Authorized OAuth Apps**, you should see that your AWS CodeBuild region has been authorized\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/github-oauth-apps.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

## Filter GitHub Webhook Events<a name="sample-github-pull-request-filter-webhook-events"></a>

 You can use webhook filter groups to specify which GitHub webhook events trigger a build\. For example, you can specify that a build is triggered for specified branches only\. 

**Note**  
Filter groups work the same way in GitHub and GitHub Enterprise\.

 You can create one or more webhook filter groups to specify which webhook events trigger a build\. A build is triggered if all the filters on one or more filter groups evaluate to true\. When you create a filter group, you specify: 
+  An event\. For GitHub, you can choose one or more of the following events: `PUSH`, `PULL_REQUEST_CREATED`, `PULL_REQUEST_UPDATED`, and `PULL_REQUEST_REOPENED`\. 
**Note**  
 `PULL_REQUEST_REOPENED` works with GitHub and GitHub Enterprise only\. 
+  One or more optional filters\. Use a regular expression to specify a filter\. For an event to trigger a build, every filter associated with it must evaluate to true\. 
  +  `ACTOR_ACCOUNT_ID` \(`ACTOR_ID` in the console\): A webhook event triggers a build when a GitHub or GitHub Enterprise account ID matches the regular expression pattern\. 
  +  `HEAD_REF`: A webhook event triggers a build when the head reference matches the regular expression pattern\. For example, `refs/heads/branch-name` and `refs/tags/tag-name`\.
  +  `BASE_REF`: A webhook event triggers a build when the base reference matches the regular expression pattern\. A `BASE_REF` filter works with pull request events only\. For example, `refs/heads/branch-name`\. 
  +  `FILE_PATH`: A webhook triggers a build when the path of a changed file matches the regular expressions pattern\. A `FILE_PATH` filter works with GitHub and GitHub Enterprise push events only\. 

**Topics**
+ [Filter GitHub Webhook Events \(Console\)](#sample-github-pull-request-filter-webhook-events-console)
+ [Filter GitHub Webhook Events \(SDK\)](#sample-github-pull-request-filter-webhook-events-sdk)
+ [Filter GitHub Webhook Events \(AWS CloudFormation\)](#sample-github-pull-request-filter-webhook-events-cfn)

### Filter GitHub Webhook Events \(Console\)<a name="sample-github-pull-request-filter-webhook-events-console"></a>

 To use the AWS Management Console to filter webhook events: 

1.  Select **Rebuild every time a code change is pushed to this repository** when you create your project\. 

1.  From **Event type**, choose one or more events\. 

1.  To filter when an event triggers a build, under **Start a build under these conditions**, add one or more optional filters\. 

1.  To filter when an event is not triggered, under **Don't start a build under these conditions**, add one or more optional filters\. 

1.  Choose **Add filter group**to add another filter group\. 

 For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console) and [WebhookFilter](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_WebhookFilter.html) in the *AWS CodeBuild API Reference*\. 

In this example, a webhook filter group triggers a build for pull requests only:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

Using an example of two webhook filter groups, a build is triggered when one or both evaluate to true:
+ The first filter group specifies pull requests that are created, updated, or reopened on branches with Git reference names that match the regular expression `^refs/heads/master$` and head references that match `^refs/heads/branch1$`\. 
+ The second filter group specifies push requests on branches with Git reference names that match the regular expression `^refs/heads/branch1$`\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter-head-base-regexes.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

In this example, a webhook filter group triggers a build for all requests except tag events\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter-exclude.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

In this example, a webhook filter group triggers a build only when files with names that match the regular expression `^buildspec.*` change\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter-file-name-regex.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

In this example, a webhook filter group triggers a build only when a change is made by a specified GitHub or GitHub Enterprise user with account ID that matches the regular expression `actor-account-id`\. 

**Note**  
 For information about how to find your GitHub account ID, see https://api\.github\.com/users/*user\-name*, where *user\-name* is your GitHub user name\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pull-request-webhook-filter-actor.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

### Filter GitHub Webhook Events \(SDK\)<a name="sample-github-pull-request-filter-webhook-events-sdk"></a>

To use the AWS CodeBuild SDK to filter webhook events, use the `filterGroups` field in the request syntax of the `CreateWebhook` or `UpdateWebhook` API methods\. For more information, see [WebhookFilter](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_WebhookFilter.html) in the *CodeBuild API Reference*\. 

 To create a webhook filter that triggers a build for pull requests only, insert the following into the request syntax: 

```
"filterGroups": [
   [
        {
            "type": "EVENT", 
            "pattern": "PULL_REQUEST_CREATED, PULL_REQUEST_UPDATED, PULL_REQUEST_REOPENED"
        }
    ]
]
```

 To create a webhook filter that triggers a build for specified branches only, use the `pattern` parameter to specify a regular expression to filter branch names\. Using an example of two filter groups, a build is triggered when one or both evaluate to true:
+ The first filter group specifies pull requests that are created, updated, or reopened on branches with Git reference names that match the regular expression `^refs/heads/master$` and head references that match `^refs/heads/myBranch$`\. 
+ The second filter group specifies push requests on branches with Git reference names that match the regular expression `^refs/heads/myBranch$`\. 

```
"filterGroups": [
    [
        {
            "type": "EVENT", 
            "pattern": "PULL_REQUEST_CREATED, PULL_REQUEST_UPDATED, PULL_REQUEST_REOPENED"
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

 You can use the `excludeMatchedPattern` parameter to specify which events do not trigger a build\. For example, in this example a build is triggered for all requests except tag events\. 

```
"filterGroups": [
    [
        {
            "type": "EVENT", 
            "pattern": "PUSH, PULL_REQUEST_CREATED, PULL_REQUEST_UPDATED, PULL_REQUEST_REOPENED"
        },
        {
            "type": "HEAD_REF", 
            "pattern": "^refs/tags/.*", 
            "excludeMatchedPattern": true
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

You can create a filter that triggers a build only when a change is made by a specified GitHub or GitHub Enterprise user with account ID `actor-account-id`\. 

**Note**  
 For information about how to find your GitHub account ID, see https://api\.github\.com/users/*user\-name*, where *user\-name* is your GitHub user name\. 

```
"filterGroups": [
    [
        {
            "type": "EVENT", 
            "pattern": "PUSH, PULL_REQUEST_CREATED, PULL_REQUEST_UPDATED, PULL_REQUEST_REOPENED"
        },
        {
            "type": "ACTOR_ACCOUNT_ID", 
            "pattern": "actor-account-id"
        }
    ]
]
```

### Filter GitHub Webhook Events \(AWS CloudFormation\)<a name="sample-github-pull-request-filter-webhook-events-cfn"></a>

 To use an AWS CloudFormation template to filter webhook events, use the AWS CodeBuild project's `FilterGroups` property\. The following YAML\-formatted portion of a AWS CloudFormation template creates two filter groups\. Together, they trigger a build when one or both evaluate to true: 
+  The first filter group specifies pull requests are created or updated on branches with Git reference names that match the regular expression `^refs/heads/master$` by a GitHub user that does not have account ID `12345`\. 
+  The second filter group specifies push requests are created on files with names that match the regular expression `READ_ME` in branches with Git reference names that match the regular expression `^refs/heads/.*`\. 

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
      Image: aws/codebuild/java:openjdk-8
    Source:
      Type: GITHUB
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
          - Type: FILE_PATH
            Pattern: READ_ME
            ExcludeMatchedPattern: true
```