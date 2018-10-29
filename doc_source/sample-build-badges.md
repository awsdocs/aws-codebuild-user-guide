--------

A new console design is available for this service\. Although the procedures in this guide were written for the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\.

--------

# Build Badges Sample with AWS CodeBuild<a name="sample-build-badges"></a>

AWS CodeBuild now supports the use of build badges, which provide an embeddable, dynamically generated image \(*badge*\) that displays the status of the latest build for a project\. This image is accessible through a publicly available URL generated for your AWS CodeBuild project\. This allows anyone to view the status of an AWS CodeBuild project\. Build badges do not contain any security information, so they do not require authentication\.

## Create a Build Project with Build Badges Enabled \(Console\)<a name="sample-build-badges-request-running"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. If a welcome page is displayed, choose **Get started**\. If a welcome page is not displayed, on the navigation pane, choose **Build projects**, and then choose **Create project**\.

1. On the **Configure your project** page, for **Project name**, type a name for this build project\. Build project names must be unique across each AWS account\.

1. In **Source: What to build**, for **Source provider**, choose the source code provider type, and then do one of the following:
   + If you chose **Amazon S3**, then for **Bucket**, choose the name of the input bucket that contains the source code\. For **S3 object key**, type the name of the ZIP file that contains the source code\.
   + If you chose **AWS CodeCommit**, then for **Repository**, choose the name of the repository\. Select the **Build Badge** check box to make your project's build status visible and embeddable\.
   + If you chose **GitHub**, follow the instructions to connect \(or reconnect\) with GitHub\. On the GitHub **Authorize application** page, for **Organization access**, choose **Request access** next to each repository you want AWS CodeBuild to be able to access\. After you choose **Authorize application**, back in the AWS CodeBuild console, for **Repository**, choose the name of the repository that contains the source code\. Select the **Build Badge** check box to make your project's build status visible and embeddable\.
   + If you chose **Bitbucket**, follow the instructions to connect \(or reconnect\) with Bitbucket\. On the Bitbucket **Confirm access to your account** page, for **Organization access**, choose **Grant access**\. After you choose **Grant access**, back in the AWS CodeBuild console, for **Repository**, choose the name of the repository that contains the source code\. Select the **Build Badge** check box to make your project's build status visible and embeddable\.
**Important**  
If you update your project source, then this could affect the accuracy of the project's build badges\.

1. In **Environment: How to build**:

   For **Environment image**, do one of the following:
   + To use a Docker image managed by AWS CodeBuild, choose **Use an image managed by AWS CodeBuild**, and then make selections from **Operating system**, **Runtime**, and **Version**\.
   + To use another Docker image, choose **Specify a Docker image**\. For **Custom image type**, choose **Other** or **Amazon ECR**\. If you choose **Other**, then for **Custom image ID**, type the name and tag of the Docker image in Docker Hub, using the format `repository-name/image-name:image-tag`\. If you choose **Amazon ECR**, then use **Amazon ECR repository** and **Amazon ECR image** to choose the Docker image in your AWS account\. 

   For **Build specification**, do one of the following:
   + Use the buildspec\.yml file in the source code root directory\.
   + Override the build specification by inserting the build commands\.

   For more information, see the [Build Spec Reference](build-spec-ref.md)\.

1. In **Artifacts: Where to put the artifacts from this build project**, for **Artifacts type**, do one of the following:
   + If you do not want to create any build output artifacts, choose **No artifacts**\.
   + To store the build output in an Amazon S3 bucket, choose **Amazon S3**, and then do the following:
     + If you want to use your project name for the build output ZIP file or folder, leave **Artifacts name** blank\. Otherwise, type the name in the **Artifacts name** box\. \(By default, the artifact name is the project name\. If you want to specify a different name, type it in the artifacts name box\. If you want to output a ZIP file, then include the zip extension\.
     + For **Bucket name**, choose the name of the output bucket\.
     + If you chose **Insert build commands** earlier in this procedure, then for **Output files**, type the locations of the files from the build that you want to put into the build output ZIP file or folder\. For multiple locations, separate each location with a comma \(for example, `appspec.yml, target/my-app.jar`\)\. For more information, see the description of `files` in [Build Spec Syntax](build-spec-ref.md#build-spec-ref-syntax)\.

1. In **Service role**, do one of the following:
   + If you do not have an AWS CodeBuild service role, choose **Create a service role in your account**\. In **Role name**, accept the default name or type your own\.
   + If you have an AWS CodeBuild service role, choose **Choose an service existing role from your account**\. In **Role name**, choose the service role\.
**Note**  
When you use the console to create or update a build project, you can create an AWS CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. Expand **Show advanced settings** and set the other advanced settings as appropriate\.

1. Choose **Continue**\. On the **Review** page, choose **Save and build** or, to run the build later, choose **Save**\.

## Create a Build Project with Build Badges Enabled \(CLI\)<a name="sample-build-badges-request-running-cli"></a>

For information on creating a build project, see [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli)\. To include build badges with your AWS CodeBuild project, you must specify *badgeEnabled* with a value of `true` \.

## Access Your AWS CodeBuild Build Badges<a name="access-badges"></a>

You can use the AWS CodeBuild console or AWS CLI to access build badges\.
+ In the AWS CodeBuild console, in the list of build projects, in the **Project** column, choose the link that corresponds to the build project\. On the **Build project: *project\-name*** page, expand **Project details**\. The build badge URL appears under **Advanced**\. For more information, see [View a Build Project's Details \(Console\)](view-project-details.md#view-project-details-console)\.
+ In the AWS CLI, run the `batch-get-projects` command\. The build badge URL is included in the project environment details section of the output\. For more information, see [View a Build Project's Details \(AWS CLI\)](view-project-details.md#view-project-details-cli)\.

**Important**  
The given build badge request URL is for the master branch, but you can specify any branch in your source repository with which you have run a build\.

## Publish Your AWS CodeBuild Build Badges<a name="publish-badges"></a>

You can include your build badge request URL in a markdown file in your preferred repository \(for example, GitHub or AWS CodeCommit\) to display the status of the latest build\.

Sample markdown code:

```
![Build Status](https://codebuild.us-east-1.amazon.com/badges?uuid=...&branch=master)
```

## AWS CodeBuild Badge Statuses<a name="badge-statuses"></a>
+ **PASSING** The most recent build on the given branch passed\. 
+ **FAILING** The most recent build on the given branch timed out, failed, faulted, or was stopped\.
+ **IN\_PROGRESS** The most recent build on the given branch is in progress\.
+ **UNKNOWN** The project has not yet run a build for the given branch or at all\. Also, the build badges feature might have been disabled\.