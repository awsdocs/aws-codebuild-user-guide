# Use AWS CodeBuild with Jenkins<a name="jenkins-plugin"></a>

You can use the Jenkins plugin for AWS CodeBuild to integrate CodeBuild with your Jenkins build jobs\. Instead of sending your build jobs to Jenkins build nodes, you use the plugin to send your build jobs to CodeBuild\. This eliminates the need for you to provision, configure, and manage Jenkins build nodes\.

## Setting up Jenkins<a name="setup-jenkins"></a>

For information about setting up Jenkins with the AWS CodeBuild plugin, and to download the plugin source code, see [ https://github\.com/awslabs/aws\-codebuild\-jenkins\-plugin](https://github.com/awslabs/aws-codebuild-jenkins-plugin)\.

## Installing the plugin<a name="plugin-installation"></a>

If you already have a Jenkins server set up and would like to only install the AWS CodeBuild plugin, on your Jenkins instance, in the Plugin Manager, search for **CodeBuild Plugin for Jenkins**\.

## Using the plugin<a name="plugin-usage"></a><a name="source-available-outside-of-your-vpc"></a>

**To use AWS CodeBuild with sources from outside of a VPC**

1. Create a project in the CodeBuild console\. For more information, see [Create a build project \(console\)](create-project-console.md)\. 
   + Choose the AWS Region where you want to run the build\.
   + \(Optional\) Set the Amazon VPC configuration to allow the CodeBuild build container to access resources in your VPC\.
   + Write down the name of your project\. You need it in step 3\.
   + \(Optional\) If your source repository is not natively supported by CodeBuild, you can set Amazon S3 as the input source type for your project\.

1. In the IAMconsole, create an IAM user to be used by the Jenkins plugin\. 
   + When you create credentials for the user, choose **Programmatic Access**\.
   + Create a policy similar to the following and then attach the policy to your user\.

     ```
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Resource": ["arn:aws:logs:{{region}}:{{awsAccountId}}:log-group:/aws/codebuild/{{projectName}}:*"],
           "Action": ["logs:GetLogEvents"]
         },
         {
           "Effect": "Allow",
           "Resource": ["arn:aws:s3:::{{inputBucket}}"],
           "Action": ["s3:GetBucketVersioning"]
         },
         {
           "Effect": "Allow",
           "Resource": ["arn:aws:s3:::{{inputBucket}}/{{inputObject}}"],
           "Action": ["s3:PutObject"]
         },
         {
           "Effect": "Allow",
           "Resource": ["arn:aws:s3:::{{outputBucket}}/*"],
           "Action": ["s3:GetObject"]
         },
         {
           "Effect": "Allow",
           "Resource": ["arn:aws:codebuild:{{region}}:{{awsAccountId}}:project/{{projectName}}"],
           "Action": ["codebuild:StartBuild",
             "codebuild:BatchGetBuilds",
             "codebuild:BatchGetProjects"]
         }
       ]
     }
     ```

1. Create a freestyle project in Jenkins\.
   + On the **Configure** page, choose **Add build step**, and then choose **Run build on CodeBuild**\.
   + Configure your build step\.
     + Provide values for **Region**, **Credentials**, and **Project Name**\.
     + Choose **Use Project source**\.
     + Save the configuration and run a build from Jenkins\.

1. For **Source Code Management**, choose how you want to retrieve your source\. You might need to install the GitHub plugin \(or the Jenkins plugin for your source repository provider\) on your Jenkins server\.
   + On the **Configure** page, choose **Add build step**, and then choose **Run build on AWS CodeBuild**\.
   + Configure your build step\.
     + Provide values for **Region**, **Credentials**, and **Project Name**\.
     + Choose **Use Jenkins source**\.
     + Save the configuration and run a build from Jenkins\.<a name="jenkins-pipeline-plugin"></a>

**To use the AWS CodeBuild plugin with the Jenkins pipeline plugin**
+ On your Jenkins pipeline project page, use the snippet generator to generate a pipeline script that adds CodeBuild as a step in your pipeline\. It should generate a script similar to this:

  ```
  awsCodeBuild projectName: 'project', credentialsType: 'keys', region: 'us-west-2', sourceControlType: 'jenkins'
  ```