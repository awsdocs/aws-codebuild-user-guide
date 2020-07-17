# Test and debug locally with the AWS CodeBuild agent<a name="use-codebuild-agent"></a>

 This topic provides information about how to run the AWS CodeBuild agent and subscribe to notifications about new versions of the agent\. 

## Test and debug on a local machine with the CodeBuild agent<a name="test-and-debug-with-codebuild-agent"></a>

 You can use the AWS CodeBuild agent to test and debug builds on a local machine\. 

**To use the agent**

1.  Download the [codebuild\.sh](https://github.com/aws/aws-codebuild-docker-images/blob/master/local_builds/codebuild_build.sh) script\. 

1.  Run the script and specify your container images and output directory: 

   ```
   codebuild_build.sh [-i image_name] [-a artifact_output_directory] [options]
   ```

 The CodeBuild agent is available from [https://hub\.docker\.com/r/amazon/aws\-codebuild\-local/](https://hub.docker.com/r/amazon/aws-codebuild-local/)\. Its Secure Hash Algorithm \(SHA\) signature is `78f5c1a205604c39cd8c797fd8447f590428c0908ba1fbdbd3dcf8712af5e325`\. You can use this to identify the version of the agent\. To see the agent's SHA signature, run the following command: 

```
docker inspect amazon/aws-codebuild-local
```

## Receive notifications for new CodeBuild agent versions<a name="receive-codebuild-agent-notifications"></a>

 You can subscribe to Amazon SNS notifications so you know when new versions of the AWS CodeBuild agent are released\. Follow the steps in this procedure to subscribe to these notifications\. 

**To subscribe to the CodeBuild agent notifications**

1.  Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\. 

1.  In the navigation bar, if it's not already selected, change the AWS Region to **US East \(N\. Virginia\)**\. You must select this AWS Region because the Amazon SNS notifications that you are subscribing to are created in this Region\. 

1.  In the navigation pane, choose **Subscriptions**\. 

1.  Choose **Create subscription**\. 

1.  In **Create subscription**: 

    For **Topic ARN**, use the following Amazon Resource Name \(ARN\): 

   ```
   arn:aws:sns:us-east-1:850632864840:AWS-CodeBuild-Local-Agent-Updates
   ```

    For **Protocol**, choose **Email** or **SMS**\. 

    For **Endpoint**, choose where \(email or SMS\) to receive the notifications\. Enter an email or address or phone number, including area code\. 

    Choose **Create subscription**\. 

    If you choose **Email**, you receive an email asking you to confirm your subscription\. Follow the directions in the email to complete your subscription\. 

 If you no longer want to receive these notifications, follow the steps in this procedure to unsubscribe\. 

**To unsubscribe from CodeBuild agent notifications**

1.  Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\. 

1.  In the navigation pane, choose **Subscriptions**\. 

1.  Select the subscription and from **Actions**, choose **Delete subscriptions**\. When you are prompted to confirm, choose **Delete**\. 