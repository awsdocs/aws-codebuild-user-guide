# Build badges sample with CodeBuild<a name="sample-build-badges"></a>

AWS CodeBuild now supports the use of build badges, which provide an embeddable, dynamically generated image \(*badge*\) that displays the status of the latest build for a project\. This image is accessible through a publicly available URL generated for your CodeBuild project\. This allows anyone to view the status of a CodeBuild project\. Build badges do not contain any security information, so they do not require authentication\.

## Create a build project with build badges enabled \(console\)<a name="sample-build-badges-request-running"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  If a CodeBuild information page is displayed, choose **Create build project**\. Otherwise, on the navigation pane, expand **Build**, choose **Build projects**, and then choose **Create build project**\. 

1. In **Project name**, enter a name for this build project\. Build project names must be unique across each AWS account\. You can also include an optional description of the build project to help other users understand what this project is used for\.

1. In **Source**, for **Source provider**, choose the source code provider type, and then do one of the following:
**Note**  
 CodeBuild does not support build badges with the Amazon S3 source provider\. Because AWS CodePipeline uses Amazon S3 for artifact transfers, build badges are not supported for build projects that are part of a pipeline created in CodePipeline\. 
   + If you chose **CodeCommit**, then for **Repository**, choose the name of the repository\. Select **Enable build badge** to make your project's build status visible and embeddable\.
   + If you chose **GitHub**, follow the instructions to connect \(or reconnect\) with GitHub\. On the GitHub **Authorize application** page, for **Organization access**, choose **Request access** next to each repository you want AWS CodeBuild to be able to access\. After you choose **Authorize application**, back in the AWS CodeBuild console, for **Repository**, choose the name of the repository that contains the source code\. Select **Enable build badge** to make your project's build status visible and embeddable\.
   + If you chose **Bitbucket**, follow the instructions to connect \(or reconnect\) with Bitbucket\. On the Bitbucket **Confirm access to your account** page, for **Organization access**, choose **Grant access**\. After you choose **Grant access**, back in the AWS CodeBuild console, for **Repository**, choose the name of the repository that contains the source code\. Select **Enable build badge** to make your project's build status visible and embeddable\.
**Important**  
Updating your project source might affect the accuracy of the project's build badges\.

1. In **Environment**:

   For **Environment image**, do one of the following:
   + To use a Docker image managed by AWS CodeBuild, choose **Managed image**, and then make selections from **Operating system**, **Runtime\(s\)**, **Image**, and **Image version**\. Make a selection from **Environment type** if it is available\.
   + To use another Docker image, choose **Custom image**\. For **Environment type**, choose **ARM**, **Linux**, **Linux GPU**, or **Windows**\. If you choose **Other registry**, for **External registry URL**, enter the name and tag of the Docker image in Docker Hub, using the format `docker repository/docker image name`\. If you choose **Amazon ECR**, use **Amazon ECR repository** and **Amazon ECR image** to choose the Docker image in your AWS account\.
   + To use a private Docker image, choose **Custom image**\. For **Environment type**, choose **ARM**, **Linux**, **Linux GPU**, or **Windows**\. For **Image registry**, choose **Other registry**, and then enter the ARN of the credentials for your private Docker image\. The credentials must be created by Secrets Manager\. For more information, see [What Is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/) in the *AWS Secrets Manager User Guide*\.

1. In **Service role**, do one of the following:
   + If you do not have a CodeBuild service role, choose **New service role**\. In **Role name**, enter a name for the new role\.
   + If you have a CodeBuild service role, choose **Existing service role**\. In **Role ARN**, choose the service role\.
**Note**  
When you use the console to create or update a build project, you can create a CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. In **Buildspec**, do one of the following:
   + Choose **Use a buildspec file** to use the buildspec\.yml file in the source code root directory\.
   + Choose **Insert build commands** to use the console to insert build commands\.

   For more information, see the [Buildspec reference](build-spec-ref.md)\.

1. In **Artifacts**, for **Type**, do one of the following:
   + If you do not want to create build output artifacts, choose **No artifacts**\.
   + To store the build output in an S3 bucket, choose **Amazon S3**, and then do the following:
     + If you want to use your project name for the build output ZIP file or folder, leave **Name** blank\. Otherwise, enter the name\. By default, the artifact name is the project name\. If you want to use a different name, enter it in the artifacts name box\. If you want to output a ZIP file, include the zip extension\.
     + For **Bucket name**, choose the name of the output bucket\.
     + If you chose **Insert build commands** earlier in this procedure, for **Output files**, enter the locations of the files from the build that you want to put into the build output ZIP file or folder\. For multiple locations, separate each location with a comma \(for example, `appspec.yml, target/my-app.jar`\)\. For more information, see the description of `files` in [Buildspec syntax](build-spec-ref.md#build-spec-ref-syntax)\.

1. Expand **Additional configuration** and choose options as appropriate\.

1. Choose **Create build project**\. On the **Review** page, choose **Start build** to run the build\.

## Create a build project with build badges enabled \(CLI\)<a name="sample-build-badges-request-running-cli"></a>

For information about creating a build project, see [Create a build project \(AWS CLI\)](create-project-cli.md)\. To include build badges with your AWS CodeBuild project, you must specify *badgeEnabled* with a value of `true`\.

## Access your AWS CodeBuild build badges<a name="access-badges"></a>

You can use AWS CodeBuild console or the AWS CLI to access build badges\.
+ In the CodeBuild console, in the list of build projects, in the **Name** column, choose the link that corresponds to the build project\. On the **Build project: *project\-name*** page, in **Configuration**, choose **Copy badge URL**\. For more information, see [View a build project's details \(console\)](view-project-details.md#view-project-details-console)\.
+ In the AWS CLI, run the `batch-get-projects` command\. The build badge URL is included in the project environment details section of the output\. For more information, see [View a build project's details \(AWS CLI\)](view-project-details.md#view-project-details-cli)\.

**Important**  
The build badge request URL is for the default branch, but you can specify any branch in your source repository that you have used to run a build\.

## Publish your CodeBuild build badges<a name="publish-badges"></a>

You can include your build badge request URL in a markdown file in your preferred repository \(for example, GitHub or CodeCommit\) to display the status of the latest build\.

Sample markdown code:

```
![Build Status](https://codebuild.us-east-1.amazon.com/badges?uuid=...&branch=main)
```

## CodeBuild badge statuses<a name="badge-statuses"></a>
+ **PASSING** The most recent build on the given branch passed\. 
+ **FAILING** The most recent build on the given branch timed out, failed, faulted, or was stopped\.
+ **IN\_PROGRESS** The most recent build on the given branch is in progress\.
+ **UNKNOWN** The project has not yet run a build for the given branch or at all\. Also, the build badges feature might have been disabled\.