# Source version sample with AWS CodeBuild<a name="sample-source-version"></a>

 This sample demonstrates how to specify a version of your source using a format other than a commit ID \(also known as a commit SHA\)\. You can specify the version of your source in the following ways: 
+  For an Amazon S3 source provider, use the version ID of the object that represents the build input ZIP file\. 
+  For CodeCommit, Bitbucket, GitHub, and GitHub Enterprise Server, use one of the following: 
  +  Pull request as a pull request reference \(for example, `refs/pull/1/head`\)\. 
  +  Branch as a branch name\. 
  +  Commit ID\. 
  +  Tag\. 
  +  Reference and a commit ID\. The reference can be one of the following:
    +  A tag \(for example, `refs/tags/mytagv1.0^{full-commit-SHA}`\)\. 
    +  A branch \(for example, `refs/heads/mydevbranch^{full-commit-SHA}`\)\. 
    +  A pull request \(for example, `refs/pull/1/head^{full-commit-SHA}`\)\. 

**Note**  
 You can specify the version of a pull request source only if your repository is GitHub or GitHub Enterprise Server\. 

 If you use a reference and a commit ID to specify a version, the `DOWNLOAD_SOURCE` phase of your build is faster than if you provide the version only\. This is because when you add a reference, CodeBuild does not need to download the entire repository to find the commit\. 
+ You can specify a source version with only a commit ID, such as `12345678901234567890123467890123456789`\. If you do this, CodeBuild must download the entire repository to find the version\.
+ You can specify a source version with a reference and a commit ID in this format: `refs/heads/branchname^{full-commit-SHA}` \(for example, `refs/heads/main^{12345678901234567890123467890123456789}`\)\. If you do this, CodeBuild downloads only the specified branch to find the version\. \.

**Note**  
To speed up the `DOWNLOAD_SOURCE` phase of your build, you can also to set **Git clone depth** to a low number\. CodeBuild downloads fewer versions of your repository\.

**To specify a GitHub repository version with a commit ID**

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. Create a build project\. For information, see [Create a build project \(console\)](create-project-console.md) and [Run a build \(console\)](run-build-console.md)\. Leave all settings at their default values, except for these settings:
   +  In **Source**: 
     +  For **Source provider**, choose **GitHub**\. If you are not connected to GitHub, follow the instructions to connect\. 
     +  For **Repository**, choose **Public repository**\. 
     +  For **Repository URL**, enter **https://github\.com/aws/aws\-sdk\-ruby\.git**\. 
   + In **Environment**:
     + For **Environment image**, choose **Managed image**\.
     + For **Operating system**, choose **Amazon Linux 2**\.
     + For **Runtime\(s\)**, choose **Standard**\.
     + For **Image**, choose **aws/codebuild/amazonlinux2\-x86\_64\-standard:2\.0**\.

1.  For **Build specifications**, choose **Insert build commands**, and then choose **Switch to editor**\. 

1.  In **Build commands**, replace the placeholder text with the following: 

   ```
   version: 0.2
   
   phases:
     install:
       runtime-versions:
         ruby: 2.6
     build:
       commands:
          - echo $CODEBUILD_RESOLVED_SOURCE_VERSION
   ```

    The `runtime-versions` section is required when you use the Ubuntu standard image 2\.0\. Here, the Ruby version 2\.6 runtime is specified, but you can use any runtime\. The `echo` command displays the version of the source code stored in the `CODEBUILD_RESOLVED_SOURCE_VERSION` environment variable\. 

1.  On **Build configuration**, accept the defaults, and then choose **Start build**\. 

1.  For **Source version**, enter **046e8b67481d53bdc86c3f6affdd5d1afae6d369**\. This is the SHA of a commit in the `https://github.com/aws/aws-sdk-ruby.git` repository\. 

1.  Choose **Start build**\. 

1.  When the build is complete, you should see the following: 
   +  On the **Build logs** tab, which version of the project source was used\. Here is an example\.

     ```
     [Container] Date Time Running command echo $CODEBUILD_RESOLVED_SOURCE_VERSION 
     046e8b67481d53bdc86c3f6affdd5d1afae6d369
      
     [Container] Date Time Phase complete: BUILD State: SUCCEEDED
     ```
   +  On the **Environment variables** tab, the **Resolved source version** matches the commit ID used to create the build\. 
   +  On the **Phase details** tab, the duration of the `DOWNLOAD_SOURCE` phase\. 

 These steps show you how to create a build using the same version of the source\. This time, the version of the source is specified using a reference with the commit ID\. 

**To specify a GitHub repository version with a commit ID and reference**

1.  From the left navigation pane, choose **Build projects**, and then choose the project you created earlier\. 

1.  Choose **Start build**\. 

1.  In **Source version**, enter **refs/heads/main^\{046e8b67481d53bdc86c3f6affdd5d1afae6d369\}**\. This is the same commit ID and a reference to a branch in the format `refs/heads/branchname^{full-commit-SHA}`\. 

1.  Choose **Start build**\. 

1. When the build is complete, you should see the following: 
   +  On the **Build logs** tab, which version of the project source was used\. Here is an example\.

     ```
     [Container] Date Time Running command echo $CODEBUILD_RESOLVED_SOURCE_VERSION 
     046e8b67481d53bdc86c3f6affdd5d1afae6d369
      
     [Container] Date Time Phase complete: BUILD State: SUCCEEDED
     ```
   +  On the **Environment variables** tab, the **Resolved source version** matches the commit ID used to create the build\. 
   +  On the **Phase details** tab, the duration of the `DOWNLOAD_SOURCE` phase should be shorter than the duration when you used only the commit ID to specify the version of your source\.