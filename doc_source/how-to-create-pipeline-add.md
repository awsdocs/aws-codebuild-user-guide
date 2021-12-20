# Add a CodeBuild build action to a pipeline \(CodePipeline console\)<a name="how-to-create-pipeline-add"></a>

1. Sign in to the AWS Management Console by using:
   + Your AWS root account\. This is not recommended\. For more information, see [The account root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in the *IAM User Guide*\.
   + An administrator IAM user in your AWS account\. For more information, see [Creating your first IAM admin user and group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
   + An IAM user in your AWS account with permission to perform the following minimum set of actions:

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

1. Open the CodePipeline console at [https://console\.aws\.amazon\.com/codesuite/codepipeline/home](https://console.aws.amazon.com/codesuite/codepipeline/home)\.

1. In the AWS region selector, choose the AWS Region where your pipeline is located\. This must be a Region where CodeBuild is supported\. For more information, see [CodeBuild](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the *Amazon Web Services General Reference*\.

1. On the **Pipelines** page, choose the name of the pipeline\.

1. On the pipeline details page, in the **Source** action, choose the tooltip\. Make a note of the value for **Output artifact** \(for example, **MyApp**\)\.
**Note**  
This procedure shows you how to add a build action in a build stage between the **Source** and **Beta** stages\. If you want to add the build action somewhere else, choose the tooltip on the action just before the place where you want to add the build action, and make a note of the value for **Output artifact**\.

1. Choose **Edit**\.

1. Between the **Source** and **Beta** stages, choose **Add stage**\.
**Note**  
This procedure shows you how to add a build stage between the **Source** and **Beta** stages to your pipeline\. To add a build action to an existing stage, choose **Edit stage** in the stage, and then skip to step 8 of this procedure\. To add the build stage somewhere else, choose **Add stage** in the desired place\.

     
![\[\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/create-stage.png)

   

1. For **Stage name**, enter the name of the build stage \(for example, **Build**\)\. If you choose a different name, use it throughout this procedure\.

1. Inside of the selected stage, choose **Add action**\.
**Note**  
This procedure shows you how to add the build action inside of a build stage\. To add the build action somewhere else, choose **Add action** in the desired place\. You might first need to choose **Edit stage** in the existing stage where you want to add the build action\.

1. In **Edit action**, for **Action name**, enter a name for the action \(for example, **CodeBuild**\)\. If you choose a different name, use it throughout this procedure\.

1. For **Action provider**, choose **CodeBuild**\.

1. If you already have a build project you want to use, for **Project name**, choose the name of the build project and skip to the next step in this procedure\.

   If you need to create a new CodeBuild build project, follow the instructions in [Create a build project \(console\)](create-project-console.md) and return to this procedure\.

   If you choose an existing build project, it must have build output artifact settings already defined \(even though CodePipeline overrides them\)\. For more information, see the description of **Artifacts** in [Create a build project \(console\)](create-project-console.md) or [Change a build project's settings \(console\)](change-project-console.md)\.
**Important**  
If you enable webhooks for a CodeBuild project, and the project is used as a build step in CodePipeline, then two identical builds are created for each commit\. One build is triggered through webhooks and one through CodePipeline\. Because billing is on a per\-build basis, you are billed for both builds\. Therefore, if you are using CodePipeline, we recommend that you disable webhooks in CodeBuild\. In the CodeBuild console, clear the **Webhook** box\. For more information, see [Change a build project's settings \(console\)](change-project-console.md)

1. For **Input artifacts**, choose the output artifact that you noted earlier in this procedure\.

1. For **Output artifacts**, enter a name for the output artifact \(for example, **MyAppBuild**\)\. 

1. Choose **Add action**\.

1. Choose **Save**, and then choose **Save** to save your changes to the pipeline\.

1. Choose **Release change**\.

1. After the pipeline runs successfully, you can get the build output artifact\. With the pipeline displayed in the CodePipeline console, in the **Build** action, choose the tooltip\. Make a note of the value for **Output artifact** \(for example, **MyAppBuild**\)\.
**Note**  
You can also get the build output artifact by choosing the **Build artifacts** link on the build details page in the CodeBuild console\. To get to this page, see [View build details \(console\)](view-build-details.md#view-build-details-console), and then skip to step 31 of this procedure\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the list of buckets, open the bucket used by the pipeline\. The name of the bucket should follow the format `codepipeline-region-ID-random-number`\. You can use the AWS CLI to run the CodePipeline get\-pipeline command to get the name of the bucket:

   ```
   aws codepipeline get-pipeline --name my-pipeline-name
   ```

    In the output, the `pipeline` object contains an `artifactStore` object, which contains a `location` value with the name of the bucket\.

1. Open the folder that matches the name of your pipeline \(depending on the length of the pipeline's name, the folder name might be truncated\), and then open the folder matching the value for **Output artifact** that you noted earlier in this procedure\.

1. Extract the contents of the file\. If there are multiple files in that folder, extract the contents of the file with the latest **Last Modified** timestamp\. \(You might need to give the file the `.zip` extension so that you can work with it in your system's ZIP utility\.\) The build output artifact is in the extracted contents of the file\.

1. If you instructed CodePipeline to deploy the build output artifact, use the deployment provider's instructions to get to the build output artifact on the deployment targets\.