# Private registry with AWS Secrets Manager sample for CodeBuild<a name="sample-private-registry"></a>

 This sample shows you how to use a Docker image that is stored in a private registry as your AWS CodeBuild runtime environment\. The credentials for the private registry are stored in AWS Secrets Manager\. Any private registry works with CodeBuild\. This sample uses Docker Hub\. 

## Private registry sample requirements<a name="sample-private-registry-requirements"></a>

 To use a private registry with AWS CodeBuild, you must have the following: 
+  A Secrets Manager secret that stores your Docker Hub credentials\. The credentials are used to access your private repository\. 
+  A private repository or account\. 
+  A CodeBuild service role IAM policy that grants access to your Secrets Manager secret\. 

 Follow these steps to create these resources and then create a CodeBuild build project using the Docker images stored in your private registry\. 

## Create a CodeBuild project with a private registry<a name="private-registry-sample-create-project"></a>

1.  For information about how to create a free private repository, see [Repositories on Docker Hub](https://docs.docker.com/docker-hub/repos/)\. You can also run the following commands in a terminal to pull an image, get its ID, and push it to a new repository\. 

   ```
   docker pull amazonlinux
   docker images amazonlinux --format {{.ID}}
   docker tag image-id your-username/repository-name:latest
   docker login
   docker push your-username/repository-name
   ```

1.  Follow the steps in [Creating a basic secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html) in the *AWS Secrets Manager User Guide*\. In step 3, in **Select secret type**, do the following: 

   1.  Choose **Other type of secrets**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/private-registry-sample-secrets-manager.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

   1.  In **Secret key/value**, create one key\-value pair for your Docker Hub user name and one key\-value pair for your Docker Hub password\. 

   1.  For **Secret name**, enter a name, such as **dockerhub**\. You can enter an optional description to help you remember that this is a secret for Docker Hub\. 

   1.  Leave **Disable automatic rotation** selected because the keys correspond to your Docker Hub credentials\. 

   1.  Choose **Store secret**\. 

   1.  When you review your settings, write down the ARN to use later in this sample\. 

    For more information, see [What is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/) 

1.  When you create an AWS CodeBuild project in the console, CodeBuild attaches the required permission for you\. If you use an AWS KMS key other than `DefaultEncryptionKey`, you must add it to the service role\. For more information, see [Modifying a role \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html#roles-managingrole-editing-console) in the *AWS Identity and Management User Guide*\. 

    For your service role to work with Secrets Manager, it must have, at a minimum, the `secretsmanager:GetSecretValue` permission\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/private-registry-sample-iam.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1.  To use the console to create a project with an environment stored in a private registry, do the following while you create a project\. For information, see [Create a build project \(console\)](create-project-console.md)\. 
**Note**  
 If your private registry is in your VPC, it must have public internet access\. CodeBuild cannot pull an image from a private IP address in a VPC\. 

   1.  In **Environment**, choose **Custom image**\. 

   1.  For **Environment type**, choose **Linux** or **Windows**\. 

   1.  For **Custom image type**, choose **Other location**\. 

   1.  In **Other location**, enter the image location and the ARN or name of your Secrets Manager credentials\. 
**Note**  
 If your credentials do not exist in your current Region, then you must use the ARN\. You cannot use the credential name if the credentials exist in a different Region\. 