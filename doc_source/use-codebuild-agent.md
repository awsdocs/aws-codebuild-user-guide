# Run builds locally with the AWS CodeBuild agent<a name="use-codebuild-agent"></a>

You can use the AWS CodeBuild agent to run CodeBuild builds on a local machine\. There are agents available for x86\_64 and ARM platforms\.

You can also subscribe to receive notifications when new versions of the agent are published\. 

## Prerequisites<a name="use-codebuild-agent.prerequisites"></a>

Before you begin, you need to do the following:
+ Install Git on your local machine\.
+ Install and set up [Docker](https://www.docker.com/) on your local machine\.

## Set up the build image<a name="use-codebuild-agent.setup-image"></a>

You only need to set up the build image the first time you run the agent, or when the image has changed\.

**To set up the build image**

1. If you want to use a curated Amazon Linux 2 image, you can pull it from the CodeBuild public Amazon ECR repository at [https://gallery\.ecr\.aws/codebuild/amazonlinux2\-x86\_64\-standard](https://gallery.ecr.aws/codebuild/amazonlinux2-x86_64-standard) with the following command:

   ```
   $ docker pull public.ecr.aws/codebuild/amazonlinux2-x86_64-standard:3.0
   ```

   Alternatively, if you want to use another Linux image, perform the following steps:

   1. Clone the CodeBuild image repo:

      ```
      $ git clone https://github.com/aws/aws-codebuild-docker-images.git
      ```

   1. Change to the image directory\. For this example, use the `aws/codebuild/standard:5.0` image:

      ```
      $ cd aws-codebuild-docker-images/ubuntu/standard/5.0
      ```

   1. Build the image\. This will take several minutes\. 

      ```
      $ docker build -t aws/codebuild/standard:5.0 .
      ```

1. Download the agent\.

   To download the x86\_64 version of the agent, run the following command:

   ```
   $ docker pull public.ecr.aws/codebuild/local-builds:latest
   ```

   To download the ARM version of the agent, run the following command:

   ```
   $ docker pull public.ecr.aws/codebuild/local-builds:aarch64
   ```

1. <a name="codebuild-agent-sha"></a>The CodeBuild agent is available from [https://gallery\.ecr\.aws/codebuild/local\-builds](https://gallery.ecr.aws/codebuild/local-builds)\. 

   The Secure Hash Algorithm \(SHA\) signature for the x86\_64 version of the agent is:

   ```
   sha256:6d4aca40229b0e21232198016e82b3d0e2027fb8ee639276b6d64ff995065f0c
   ```

   The SHA signature for the ARM version of the agent is:

   ```
   sha256:3fb32e40b65d38a1ea800bf05eba0bf05b36c954d9cd72fa008c74aaf2fb6577
   ```

   You can use the SHA to identify the version of the agent\. To see the agent's SHA signature, run the following command and look for the SHA under `RepoDigests`: 

   ```
   $ docker inspect public.ecr.aws/codebuild/local-builds:latest
   ```

## Run the CodeBuild agent<a name="use-codebuild-agent.run-agent"></a>

**To run the CodeBuild agent**

1. Change to the directory that contains your build project source\.

1. Download the [codebuild\_build\.sh](https://github.com/aws/aws-codebuild-docker-images/blob/master/local_builds/codebuild_build.sh) script:

   ```
   $ wget https://raw.githubusercontent.com/aws/aws-codebuild-docker-images/master/local_builds/codebuild_build.sh
   $ chmod +x codebuild_build.sh
   ```

1. Run the `codebuild_build.sh` script and specify your container image and the output directory\.

   To run an x86\_64 build, run the following command:

   ```
   $ ./codebuild_build.sh -i <container-image> -a <output directory>
   ```

   To run an ARM build, run the following command:

   ```
   $ ./codebuild_build.sh -i <container-image> -a <output directory> -l public.ecr.aws/codebuild/local-builds:aarch64
   ```

   Replace *<container\-image>* with the name of the container image, such as `aws/codebuild/standard:5.0` or `public.ecr.aws/codebuild/amazonlinux2-x86_64-standard:3.0`\.

   The script launches the build image and runs the build on the project in the current directory\. To specify the location of the build project, add the `-s <build project directory>` option to the script command\.

## Receive notifications for new CodeBuild agent versions<a name="receive-codebuild-agent-notifications"></a>

You can subscribe to Amazon SNS notifications so you will be notified when new versions of the AWS CodeBuild agent are released\. 

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