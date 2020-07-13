# Use access tokens with your source provider in CodeBuild<a name="sample-access-tokens"></a>

 This sample shows you how to connect to GitHub or Bitbucket with an access token\. For GitHub or GitHub Enterprise Server, you use a personal access token\. For Bitbucket, you use an app password\. 

## Access token prerequisites<a name="sample-access-tokens-prerequisites"></a>

 Before you begin, you must add the proper permission scopes to your access token\. 

 For GitHub, your personal access token must have the following scopes\. 
+  **repo**: Grants full control of private repositories\. 
+  **repo:status**: Grants access to commit statuses\. 
+  **admin:repo\_hook**: Grants full control of repository hooks\. This scope is not required if your token has the `repo` scope\. 

For more information, see [Understanding scopes for OAuth apps](https://developer.github.com/apps/building-oauth-apps/understanding-scopes-for-oauth-apps/) on the GitHub website\.

 For Bitbucket, your app password must have the following scopes\. 
+  **repository:read**: Grants read access to all the repositories to which the authorizing user has access\. 
+  **pullrequest:read**: Grants read access to pull requests\. If your project has a Bitbucket webhook, then your app password must have this scope\. 
+  **webhook**: Grants access to webhooks\. If your project has a webhook operation, then your app password must have this scope\. 

For more information, see [Scopes for Bitbucket Cloud REST API](https://developer.atlassian.com/cloud/bitbucket/bitbucket-cloud-rest-api-scopes/) and [OAuth on Bitbucket Cloud](https://confluence.atlassian.com/bitbucket/oauth-on-bitbucket-cloud-238027431.html) on the Bitbucket website\.

## Connect source providers with access tokens \(console\)<a name="sample-access-tokens-console"></a>

 To use the console to connect your project to GitHub or Bitbucket using access tokens, do the following while you create a project\. For information, see [Create a build project \(console\)](create-project-console.md)\. 

For GitHub:

1.  For **Source provider**, choose **GitHub**\. 

1.  For **Repository**, choose **Connect with a GitHub personal access token**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/github-access-token-console.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1.  In **GitHub personal access token**, enter your GitHub personal access token\. 

1.  Choose **Save token**\. 

For Bitbucket:

1.  For **Source provider**, choose **Bitbucket**\. 
**Note**  
CodeBuild does not support Bitbucket Server\.

1.  For **Repository**, choose **Connect with a Bitbucket app password**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/bitbucket-access-token-console.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1.  In **Bitbucket username**, enter your Bitbucket user name\. 

1.  In **Bitbucket app password**, enter your Bitbucket app password\. 

1.  Choose **Save Bitbucket credentials**\. 

## Connect source providers with access tokens \(CLI\)<a name="sample-access-tokens-cli"></a>

Follow these steps to use the AWS CLI to connect your project to GitHub or Bitbucket using access tokens\. For information about using the AWS CLI with AWS CodeBuild, see the [Command line reference](cmd-ref.md)\. 

1.  Run the import\-source\-credentials command: 

   ```
   aws codebuild import-source-credentials --generate-cli-skeleton
   ```

    JSON\-formatted data appears in the output\. Copy the data to a file \(for example, `import-source-credentials.json`\) in a location on the local computer or instance where the AWS CLI is installed\. Modify the copied data as follows, and save your results\. 

   ```
   {
     "serverType": "server-type",
     "authType": "auth-type",
     "shouldOverwrite": "should-overwrite",
     "token": "token",
     "username": "username"
   }
   ```

    Replace the following: 
   +  *server\-type*: Required value\. The source provider used for this credential\. Valid values are GITHUB, GITHUB\_ENTERPRISE, and BITBUCKET\. 
   +  *auth\-type*: Required value\. The type of authentication used to connect to a GitHub, GitHub Enterprise Server, or Bitbucket repository\. Valid values include PERSONAL\_ACCESS\_TOKEN and BASIC\_AUTH\. You cannot use the CodeBuild API to create an OAUTH connection\. You must use the CodeBuild console instead\. 
   +  *should\-overwrite*: Optional value\. Set to `false` to prevent overwriting the repository source credentials\. Set to `true` to overwrite the repository source credentials\. The default value is `true`\.
   +  *token*: Required value\. For GitHub or GitHub Enterprise Server, this is the personal access token\. For Bitbucket, this is the app password\. 
   +  *username*: Optional value\. The Bitbucket user name when authType is BASIC\_AUTH\. This parameter is ignored for other types of source providers or connections\. 

1. To connect your account with an access token, switch to the directory that contains the `import-source-credentials.json` file you saved in step 1 and run the import\-source\-credentials command again\. 

   ```
   aws codebuild import-source-credentials --cli-input-json file://import-source-credentials.json
   ```

    JSON\-formatted data appears in the output with an Amazon Resource Name \(ARN\)\. 

   ```
   {
     "arn": "arn:aws:codebuild:region:account-id:token/server-type"
   }
   ```
**Note**  
 If you run the import\-source\-credentials command with the same server type and auth type a second time, the stored access token is updated\. 

    After your account is connected with an access token, you can use `create-project` to create your CodeBuild project\. For more information, see [Create a build project \(AWS CLI\)](create-project-cli.md)\. 

1.  To view the connected access tokens, run the list\-source\-credentials command\. 

   ```
   aws codebuild list-source-credentials
   ```

    A JSON\-formatted `sourceCredentialsInfos` object appears in the output: 

   ```
   {
       "sourceCredentialsInfos": [
           {
               "authType": "auth-type",
               "serverType": "server-type", 
               "arn": "arn"
           }
       ]
   }
   ```

    The `sourceCredentialsObject` contains a list of connected source credentials information: 
   +  The `authType` is the type of authentication used by credentials\. This can be `OAUTH`, `BASIC_AUTH`, or `PERSONAL_ACCESS_TOKEN`\. 
   +  The `serverType` is the type of source provider\. This can be `GITHUB`, `GITHUB_ENTERPRISE`, or `BITBUCKET`\. 
   +  The `arn` is the ARN of the token\. 

1.  To disconnect from a source provider and remove its access tokens, run the delete\-source\-credentials command with its ARN\. 

   ```
   aws codebuild delete-source-credentials --arn arn-of-your-credentials
   ```

    JSON\-formatted data is returned with an ARN of the deleted credentials\. 

   ```
   {
     "arn": "arn:aws:codebuild:region:account-id:token/server-type"
   }
   ```