# Create a pipeline that uses CodeBuild \(CodePipeline console\)<a name="how-to-create-pipeline-console"></a>

Use the following procedure to create a pipeline that uses CodeBuild to build and deploy your source code\.

To create a pipeline that only tests your source code:
+ Use the following procedure to create the pipeline, and then delete the Build and Beta stages from the pipeline\. Then use the [Add a CodeBuild test action to a pipeline \(CodePipeline console\)](how-to-create-pipeline-add-test.md) procedure in this topic to add to the pipeline a test action that uses CodeBuild\.
+ Use one of the other procedures in this topic to create the pipeline, and then use the [Add a CodeBuild test action to a pipeline \(CodePipeline console\)](how-to-create-pipeline-add-test.md) procedure in this topic to add to the pipeline a test action that uses CodeBuild\.

**To use the create pipeline wizard in CodePipeline to create a pipeline that uses CodeBuild**

1. Sign in to the AWS Management Console by using:
   + Your AWS root account\. This is not recommended\. For more information, see [The account root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in the *IAM User Guide*\.
   + An administrator IAM user in your AWS account\. For more information, see [Creating your first IAM admin user and group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
   + An IAM user in your AWS account with permission to use the following minimum set of actions:

     ```
     codepipeline:*
     iam:ListRoles
     iam:PassRole
     s3:CreateBucket
     s3:GetBucketPolicy
     s3:GetObject
     s3:ListAllMyBuckets
     s3:ListBucket
     s3:PutBucketPolicy
     codecommit:ListBranches
     codecommit:ListRepositories
     codedeploy:GetApplication
     codedeploy:GetDeploymentGroup
     codedeploy:ListApplications
     codedeploy:ListDeploymentGroups
     elasticbeanstalk:DescribeApplications
     elasticbeanstalk:DescribeEnvironments
     lambda:GetFunctionConfiguration
     lambda:ListFunctions
     opsworks:DescribeStacks
     opsworks:DescribeApps
     opsworks:DescribeLayers
     ```

1. Open the AWS CodePipeline console at [https://console\.aws\.amazon\.com/codesuite/codepipeline/home](https://console.aws.amazon.com/codesuite/codepipeline/home)\.

1. In the AWS Region selector, choose the AWS Region where your build project AWS resources are located\. This must be an AWS Region where CodeBuild is supported\. For more information, see [AWS CodeBuild](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the *Amazon Web Services General Reference*\.

1. Create a pipeline\. If a CodePipeline information page is displayed, choose **Create pipeline**\. If a **Pipelines** page is displayed, choose **Create pipeline**\.

1. On the **Step 1: Choose pipeline settings** page, for **Pipeline name**, enter a name for the pipeline \(for example, **CodeBuildDemoPipeline**\)\. If you choose a different name, be sure to use it throughout this procedure\.

1. For **Role name**, do one of the following:

   Choose **New service role**, and in **Role Name**, enter the name for your new service role\.

   Choose **Existing service role**, and then choose the CodePipeline service role you created or identified as part of this topic's prerequisites\.

1. For **Artifact store**, do one of the following:
   + Choose **Default location** to use the default artifact store, such as the S3 artifact bucket designated as the default, for your pipeline in the AWS Region you have selected for your pipeline\.
   + Choose **Custom location** if you already have an existing artifact store you have created, such as an S3 artifact bucket, in the same AWS Region as your pipeline\.
**Note**  
This is not the source bucket for your pipeline's source code\. This is the artifact store for your pipeline\. A separate artifact store, such as an S3 bucket, is required for each pipeline, in the same AWS Region as the pipeline\.

1. Choose **Next**\.

1. On the **Step 2: Add source stage** page, for **Source provider**, do one of the following:
   + If your source code is stored in an S3 bucket, choose **Amazon S3**\. For **Bucket**, select the S3 bucket that contains your source code\. For **S3 object key**, enter the name of the file the contains the source code \(for example, `file-name.zip`\)\. Choose **Next**\.
   + If your source code is stored in an AWS CodeCommit repository, choose **CodeCommit**\. For **Repository name**, choose the name of the repository that contains the source code\. For **Branch name**, choose the name of the branch that contains the version of the source code you want to build\. Choose **Next**\.
   + If your source code is stored in a GitHub repository, choose **GitHub**\. Choose **Connect to GitHub**, and follow the instructions to authenticate with GitHub\. For **Repository**, choose the name of the repository that contains the source code\. For **Branch**, choose the name of the branch that contains the version of the source code you want to build\. 

   Choose **Next**\.

1. On the **Step 3: Add build stage** page, for **Build provider**, choose **CodeBuild**\.

1. If you already have a build project you want to use, for **Project name**, choose the name of the build project and skip to the next step in this procedure\. 

   If you need to create a new CodeBuild build project, follow the instructions in [Create a build project \(console\)](create-project-console.md) and return to this procedure\.

   If you choose an existing build project, it must have build output artifact settings already defined \(even though CodePipeline overrides them\)\. For more information, see [Change a build project's settings \(console\)](change-project-console.md)\.
**Important**  
If you enable webhooks for a CodeBuild project, and the project is used as a build step in CodePipeline, then two identical builds are created for each commit\. One build is triggered through webhooks, and one through CodePipeline\. Because billing is on a per\-build basis, you are billed for both builds\. Therefore, if you are using CodePipeline, we recommend that you disable webhooks in CodeBuild\. In the AWS CodeBuild console, clear the **Webhook** box\. For more information, see [Change a build project's settings \(console\)](change-project-console.md)\.

1. On the **Step 4: Add deploy stage** page, do one of the following:
   + If you do not want to deploy the build output artifact, choose **Skip**, and confirm this choice when prompted\. 
   + If you want to deploy the build output artifact, for **Deploy provider**, choose a deployment provider, and then specify the settings when prompted\.

   Choose **Next**\.

1. On the ** Review** page, review your choices, and then choose **Create pipeline**\.

1. After the pipeline runs successfully, you can get the build output artifact\. With the pipeline displayed in the CodePipeline console, in the **Build** action, choose the tooltip\. Make a note of the value for **Output artifact** \(for example, **MyAppBuild**\)\.
**Note**  
You can also get the build output artifact by choosing the **Build artifacts** link on the build details page in the CodeBuild console\. To get to this page, skip the rest of the steps in this procedure, and see [View build details \(console\)](view-build-details.md#view-build-details-console)\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the list of buckets, open the bucket used by the pipeline\. The name of the bucket should follow the format `codepipeline-region-ID-random-number`\. You can use the AWS CLI to run the CodePipeline get\-pipeline command to get the name of the bucket, where *my\-pipeline\-name* is the display name of your pipeline:

   ```
   aws codepipeline get-pipeline --name my-pipeline-name
   ```

    In the output, the `pipeline` object contains an `artifactStore` object, which contains a `location` value with the name of the bucket\.

1. Open the folder that matches the name of your pipeline \(depending on the length of the pipeline's name, the folder name might be truncated\), and then open the folder that matches the value for **Output artifact** that you noted earlier\.

1. Extract the contents of the file\. If there are multiple files in that folder, extract the contents of the file with the latest **Last Modified** timestamp\. \(You might need to give the file the `.zip` extension so that you can work with it in your system's ZIP utility\.\) The build output artifact is in the extracted contents of the file\.

1. If you instructed CodePipeline to deploy the build output artifact, use the deployment provider's instructions to get to the build output artifact on the deployment targets\.