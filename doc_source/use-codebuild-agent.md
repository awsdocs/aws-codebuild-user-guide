# Run builds locally with the AWS CodeBuild agent<a name="use-codebuild-agent"></a>

You can use the AWS CodeBuild agent to run CodeBuild builds on a local machine\. You can also subscribe to notifications about new versions of the agent\. 

## Prerequisites<a name="use-codebuild-agent.prerequisites"></a>

Before you begin, you need to do the following:
+ Install Git on your local machine\.
+ Install and setup [Docker](https://www.docker.com/) on your local machine\.

## Setup the build image<a name="use-codebuild-agent.setup-image"></a>

You only need to set up the build image the first time you run the agent, or when the image has changed\.

**To set up the build image**

1. Clone the CodeBuild image repo:

   ```
   $ git clone https://github.com/aws/aws-codebuild-docker-images.git
   ```

1. Build the image\. For this example, use the `aws/codebuild/standard:4.0` image\. This will take several minutes\.

   ```
   $ cd aws-codebuild-docker-images/ubuntu/standard/4.0
   $ docker build -t aws/codebuild/standard:4.0 .
   ```

1. Run the following Docker command to download the local CodeBuild agent\.:

   ```
   $ docker pull amazon/aws-codebuild-local:latest --disable-content-trust=false
   ```

1. The CodeBuild agent is available from [https://hub\.docker\.com/r/amazon/aws\-codebuild\-local/](https://hub.docker.com/r/amazon/aws-codebuild-local/)\. Its Secure Hash Algorithm \(SHA\) signature is:

   ```
   78f5c1a205604c39cd8c797fd8447f590428c0908ba1fbdbd3dcf8712af5e325
   ```

   You can use this to identify the version of the agent\. To see the agent's SHA signature, run the following command: 

   ```
   $ docker inspect amazon/aws-codebuild-local
   ```

## Run the CodeBuild agent<a name="use-codebuild-agent.run-agent"></a>

**To run the CodeBuild agent**

1. Change to the directory that contains your build project source\.

1. Download the [codebuild\_build\.sh](https://github.com/aws/aws-codebuild-docker-images/blob/master/local_builds/codebuild_build.sh) script:

   ```
   $ wget https://raw.githubusercontent.com/aws/aws-codebuild-docker-images/master/local_builds/codebuild_build.sh
   $ chmod +x codebuild_build.sh
   ```

1. Run the `codebuild_build.sh` script and specify your container image and the output directory: 

   ```
   $ ./codebuild_build.sh -i aws/codebuild/standard:4.0 -a <output directory>
   ```

   The script launches the build image and runs the build on the project in the current directory\. To specify the location of the build project, add the `-s <build project directory>` option to the script command\.

## Receive notifications for new CodeBuild agent versions<a name="receive-codebuild-agent-notifications"></a>

You can subscribe to Amazon SNS notifications so you know when new versions of the AWS CodeBuild agent are released\. 

**To subscribe to CodeBuild agent notifications**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\. 

1. In the navigation bar, if it's not already selected, change the AWS Region to **US East \(N\. Virginia\)**\. You must select this AWS Region because the Amazon SNS notifications that you are subscribing to are created in this Region\. 

1. In the navigation pane, choose **Subscriptions**\. 

1. Choose **Create subscription**\. 

1. In **Create subscription**, do the following: 

   1. For **Topic ARN**, use the following Amazon Resource Name \(ARN\): 

      ```
      arn:aws:sns:us-east-1:850632864840:AWS-CodeBuild-Local-Agent-Updates
      ```

   1. For **Protocol**, choose **Email** or **SMS**\. 

   1. For **Endpoint**, choose where \(email or SMS\) to receive the notifications\. Enter an email or address or phone number, including area code\. 

   1. Choose **Create subscription**\. 

   1. Choose **Email** to receive an email asking you to confirm your subscription\. Follow the directions in the email to complete your subscription\. 

      If you no longer want to receive these notifications, use the following procedure to unsubscribe\. 

**To unsubscribe from CodeBuild agent notifications**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\. 

1. In the navigation pane, choose **Subscriptions**\. 

1. Select the subscription and from **Actions**, choose **Delete subscriptions**\. When you are prompted to confirm, choose **Delete**\. 