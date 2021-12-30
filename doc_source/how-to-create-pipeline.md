# Use AWS CodePipeline with AWS CodeBuild to test code and run builds<a name="how-to-create-pipeline"></a>

You can automate your release process by using AWS CodePipeline to test your code and run your builds with AWS CodeBuild\.

The following table lists tasks and the methods available for performing them\. Using the AWS SDKs to accomplish these tasks is outside the scope of this topic\. 


****  

| Task | Available approaches | Approaches described in this topic | 
| --- | --- | --- | 
| Create a continuous delivery \(CD\) pipeline with CodePipeline that automates builds with CodeBuild |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/how-to-create-pipeline.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/how-to-create-pipeline.html)  | 
| Add test and build automation with CodeBuild to an existing pipeline in CodePipeline |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/how-to-create-pipeline.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/how-to-create-pipeline.html)  | 

## Prerequisites<a name="how-to-create-pipeline-prerequisites"></a>

1. Answer the questions in [Plan a build](planning.md)\.

1. If you are using an IAM user to access CodePipeline instead of an AWS root account or an administrator IAM user, attach the managed policy named `AWSCodePipelineFullAccess` to the user \(or to the IAM group to which the user belongs\)\. Using an AWS root account is not recommended\. This policy grants the user permission to create the pipeline in CodePipeline\. For more information, see [Attaching managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html#attach-managed-policy-console) in the *IAM User Guide*\.
**Note**  
The IAM entity that attaches the policy to the user \(or to the IAM group to which the user belongs\) must have permission in IAM to attach policies\. For more information, see [Delegating permissions to administer IAM users, groups, and credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_delegate-permissions.html) in the *IAM User Guide*\.

1. Create a CodePipeline service role, if you do not already have one available in your AWS account\. CodePipeline uses this service role to interact with other AWS services, including AWS CodeBuild, on your behalf\. For example, to use the AWS CLI to create a CodePipeline service role, run the IAM `create-role` command:

   For Linux, macOS, or Unix:

   ```
   aws iam create-role --role-name AWS-CodePipeline-CodeBuild-Service-Role --assume-role-policy-document '{"Version":"2012-10-17","Statement":{"Effect":"Allow","Principal":{"Service":"codepipeline.amazonaws.com"},"Action":"sts:AssumeRole"}}'
   ```

   For Windows:

   ```
   aws iam create-role --role-name AWS-CodePipeline-CodeBuild-Service-Role --assume-role-policy-document "{\"Version\":\"2012-10-17\",\"Statement\":{\"Effect\":\"Allow\",\"Principal\":{\"Service\":\"codepipeline.amazonaws.com\"},\"Action\":\"sts:AssumeRole\"}}"
   ```
**Note**  
The IAM entity that creates this CodePipeline service role must have permission in IAM to create service roles\.

1. After you create a CodePipeline service role or identify an existing one, you must add the default CodePipeline service role policy to the service role as described in [Review the default CodePipeline service role policy](https://docs.aws.amazon.com/codepipeline/latest/userguide/iam-identity-based-access-control.html#how-to-custom-role) in the *AWS CodePipeline User Guide*, if it isn't already a part of the policy for the role\.
**Note**  
The IAM entity that adds this CodePipeline service role policy must have permission in IAM to add service role policies to service roles\.

1. Create and upload the source code to a repository type supported by CodeBuild and CodePipeline, such as CodeCommit, Amazon S3, or GitHub\. The source code should contain a buildspec file, but you can declare one when you define a build project later in this topic\. For more information, see the [Buildspec reference](build-spec-ref.md)\.
**Important**  
If you plan to use the pipeline to deploy built source code, the build output artifact must be compatible with the deployment system you use\.   
For CodeDeploy, see the [AWS CodeDeploy sample](sample-codedeploy.md) in this guide and [Prepare a revision for CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-prepare-revision.html) in the *AWS CodeDeploy User Guide*\.
For AWS Elastic Beanstalk, see the [AWS Elastic Beanstalk sample](sample-elastic-beanstalk.md) in this guide and [Create an application source bundle](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deployment.source.html) in the *AWS Elastic Beanstalk Developer Guide*\.
For AWS OpsWorks, see [Application source](https://docs.aws.amazon.com/opsworks/latest/userguide/workingapps-creating.html#workingapps-creating-source) and [Using CodePipeline with AWS OpsWorks](https://docs.aws.amazon.com/opsworks/latest/userguide/other-services-cp.html) in the *AWS OpsWorks User Guide*\.

**Topics**
+ [Prerequisites](#how-to-create-pipeline-prerequisites)
+ [Create a pipeline that uses CodeBuild \(CodePipeline console\)](how-to-create-pipeline-console.md)
+ [Create a pipeline that uses CodeBuild \(AWS CLI\)](how-to-create-pipeline-cli.md)
+ [Add a CodeBuild build action to a pipeline \(CodePipeline console\)](how-to-create-pipeline-add.md)
+ [Add a CodeBuild test action to a pipeline \(CodePipeline console\)](how-to-create-pipeline-add-test.md)