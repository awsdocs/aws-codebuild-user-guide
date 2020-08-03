# View a running build in Session Manager<a name="session-manager"></a>

In AWS CodeBuild, you can pause a running build and then use AWS Systems Manager Session Manager to connect to the build container and view the state of the container\.

**Topics**
+ [Prerequisites](#ssm.prerequisites)
+ [Pause the build](#ssm-pause-build)
+ [Start the build](#ssm-start-build)
+ [Connect to the build container](#ssm-connect)
+ [Resume the build](#ssm-resume-build)

## Prerequisites<a name="ssm.prerequisites"></a>

To allow Session Manager to be used with the build session, you must enable session connection for the build\. There are two prerequisites:
+ CodeBuild Linux standard curated images already have the SSM agent installed and the SSM agent ContainerMode enabled\. 

  If you are using a custom image for your build, do the following:

  1. Install the SSM Agent\. For more information, see [Manually install SSM Agent on EC2 instances for Linux](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-manual-agent-install.html) in the AWS Systems Manager User Guide\.

  1. Copy the file [https://github\.com/aws/aws\-codebuild\-docker\-images/blob/master/ubuntu/standard/4\.0/amazon\-ssm\-agent\.json](https://github.com/aws/aws-codebuild-docker-images/blob/master/ubuntu/standard/4.0/amazon-ssm-agent.json) to the `/etc/amazon/ssm/` directory in your image\. This enables Container Mode in the SSM agent\.
+ The CodeBuild service role must have the following SSM policy:

  ```
  {
    "Effect": "Allow",
    "Action": [
      "ssmmessages:CreateControlChannel",
      "ssmmessages:CreateDataChannel",
      "ssmmessages:OpenControlChannel",
      "ssmmessages:OpenDataChannel"
    ],
    "Resource": "*"
  }
  ```

  You can have the CodeBuild console automatically attach this policy to your service role when you start the build\. Alternatively, you can attach this policy to your service role manually\.
+ If you have **Auditing and logging session activity** enabled in Systems Manager preferences, the CodeBuild service role must also have additional permissions\. The permissions are different, depending on where the logs are stored\.  
CloudWatch Logs  
If using CloudWatch Logs to store your logs, add the following permission to the CodeBuild service role:  

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": "logs:DescribeLogGroups",
        "Resource": "arn:aws:logs:<region-id>:<account-id>:log-group:*:*"
      },
      {
        "Effect": "Allow",
        "Action": [
          "logs:CreateLogStream",
          "logs:PutLogEvents"
        ],
        "Resource": "arn:aws:logs:<region-id>:<account-id>:log-group:<log-group-name>:*"
      }
    ]
  }
  ```  
Amazon S3  
If using Amazon S3 to store your logs, add the following permission to the CodeBuild service role:  

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "s3:GetEncryptionConfiguration",
          "s3:PutObject"
        ],
        "Resource": [
          "arn:aws:s3:::<bucket-name>",
          "arn:aws:s3:::<bucket-name>/*"
        ]
      }
    ]
  }
  ```

  For more information, see [Auditing and logging session activity](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-logging-auditing.html) in the *AWS Systems Manager User Guide*\.

## Pause the build<a name="ssm-pause-build"></a>

To pause the build, insert the codebuild\-breakpoint command in any of the build phases in your buildspec file\. The build will be paused at this point, which allows you to connect to the build container and view the container in its current state\. 

For example, add the following to the build phases in your buildspec file\.

```
phases:
  pre_build:
    commands:
      - echo Entered the pre_build phase...
      - echo "Hello World" > /tmp/hello-world
      - codebuild-breakpoint
```

This code creates the `/tmp/hello-world` file and then pauses the build at this point\.

## Start the build<a name="ssm-start-build"></a>

To allow Session Manager to be used with the build session, you must enable session connections for the build\. To do this, when starting the build, follow these steps:

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. In the navigation pane, choose **Build projects**\. Choose the build project, and then choose **Start build**\.

1. Choose **Advanced build overrides**\.

1. In the **Environment** section, choose the **Enable session connection** option\. If this option is not selected, all of the codebuild\-breakpoint and codebuild\-resume commands are ignored\.

1. In the **Environment** section, choose the **Allow AWS CodeBuild to modify this service role so it can be used with this build project** option to allow the CodeBuild console to automatically attach the session manager policy to your service role\. If you have already added the session manager policy to your role, you do not need to select this option\.

1. Make any other desired changes, and choose **Start build**\. 

1. Monitor the build status in the console\. When the session is available, the **AWS Session Manager** link appears in the **Build status** section\.

## Connect to the build container<a name="ssm-connect"></a>

You can connect to the build container in one of two ways:

CodeBuild console  
In a web browser, open the **AWS Session Manager** link to connect to the build container\. A terminal session opens that allows you to browse and control the build container\. 

AWS CLI  
Your local machine must have the Session Manager plugin installed for this procedure\. For more information, see [Install the Session Manager Plugin for the AWS CLI](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html) in the AWS Systems Manager User Guide\. 

1. Call the batch\-get\-builds api with the build ID to get information about the build\. 

   ```
   aws codebuild batch-get-builds --ids <buildID> --region <region>
   ```

1. Copy the `sessionTarget` property value\.

1. Use the following command to connect to the build container\.

   ```
   aws ssm start-session --target <sessionTarget> --region <region>
   ```

For this example, verify that the `/tmp/hello-world` file exists and contains the text `Hello World`\.

## Resume the build<a name="ssm-resume-build"></a>

After you finish examining the build container, issue the codebuild\-resume command from the container shell\.

```
$ codebuild-resume
```