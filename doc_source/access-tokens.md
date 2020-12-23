# Access your source provider in CodeBuild<a name="access-tokens"></a>

For GitHub or GitHub Enterprise Server, you use a personal access token to access the source provider\. For Bitbucket, you use an app password to access the source provider\. 

**Topics**
+ [GitHub and GitHub Enterprise Server access token](#access-tokens-github)
+ [Bitbucket app password](#access-tokens-bitbucket)

## GitHub and GitHub Enterprise Server access token<a name="access-tokens-github"></a>

### Access token prerequisites<a name="access-tokens-github-prereqs"></a>

Before you begin, you must add the proper permission scopes to your GitHub access token\. 

For GitHub, your personal access token must have the following scopes\. 
+ **repo**: Grants full control of private repositories\. 
+ **repo:status**: Grants read/write access to public and private repository commit statuses\.
+ **admin:repo\_hook**: Grants full control of repository hooks\. This scope is not required if your token has the `repo` scope\. 

For more information, see [Understanding scopes for OAuth apps](https://developer.github.com/apps/building-oauth-apps/understanding-scopes-for-oauth-apps/) on the GitHub website\.

### Connect GitHub with an access token \(console\)<a name="access-tokens-github-console"></a>

To use the console to connect your project to GitHub using an access token, do the following when you create a project\. For information, see [Create a build project \(console\)](create-project-console.md)\. 

1. For **Source provider**, choose **GitHub**\. 

1. For **Repository**, choose **Connect with a GitHub personal access token**\. 

1. In **GitHub personal access token**, enter your GitHub personal access token\. 

1. Choose **Save token**\. 

### Connect GitHub with an access token \(CLI\)<a name="access-tokens-github-cli"></a>

Follow these steps to use the AWS CLI to connect your project to GitHub using an access token\. For information about using the AWS CLI with AWS CodeBuild, see the [Command line reference](cmd-ref.md)\. 

1. Run the import\-source\-credentials command: 

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
   + *server\-type*: Required value\. The source provider used for this credential\. Valid values are GITHUB or GITHUB\_ENTERPRISE\. 
   + *auth\-type*: Required value\. The type of authentication used to connect to a GitHub or GitHub Enterprise Server repository\. Valid values include PERSONAL\_ACCESS\_TOKEN and BASIC\_AUTH\. You cannot use the CodeBuild API to create an OAUTH connection\. You must use the CodeBuild console instead\. 
   + *should\-overwrite*: Optional value\. Set to `false` to prevent overwriting the repository source credentials\. Set to `true` to overwrite the repository source credentials\. The default value is `true`\.
   + *token*: Required value\. For GitHub or GitHub Enterprise Server, this is the personal access token\.
   + *username*: Optional value\. This parameter is ignored for GitHub and GitHub Enterprise Server source providers\. 

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

1. To view the connected access tokens, run the list\-source\-credentials command\. 

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
   + The `authType` is the type of authentication used by credentials\. This can be `OAUTH`, `BASIC_AUTH`, or `PERSONAL_ACCESS_TOKEN`\. 
   + The `serverType` is the type of source provider\. This can be `GITHUB`, `GITHUB_ENTERPRISE`, or `BITBUCKET`\. 
   + The `arn` is the ARN of the token\. 

1. To disconnect from a source provider and remove its access tokens, run the delete\-source\-credentials command with its ARN\. 

   ```
   aws codebuild delete-source-credentials --arn arn-of-your-credentials
   ```

   JSON\-formatted data is returned with an ARN of the deleted credentials\. 

   ```
   {
     "arn": "arn:aws:codebuild:region:account-id:token/server-type"
   }
   ```

## Bitbucket app password<a name="access-tokens-bitbucket"></a>

### App password prerequisites<a name="access-tokens-bitbucket-prerequisites"></a>

Before you begin, you must add the proper permission scopes to your Bitbucket app password\. 

For Bitbucket, your app password must have the following scopes\. 
+ **repository:read**: Grants read access to all the repositories to which the authorizing user has access\. 
+ **pullrequest:read**: Grants read access to pull requests\. If your project has a Bitbucket webhook, then your app password must have this scope\. 
+ **webhook**: Grants access to webhooks\. If your project has a webhook operation, then your app password must have this scope\. 

For more information, see [Scopes for Bitbucket Cloud REST API](https://developer.atlassian.com/cloud/bitbucket/bitbucket-cloud-rest-api-scopes/) and [OAuth on Bitbucket Cloud](https://confluence.atlassian.com/bitbucket/oauth-on-bitbucket-cloud-238027431.html) on the Bitbucket website\.

### Connect Bitbucket with an app password \(console\)<a name="access-tokens-bitbucket-console"></a>

To use the console to connect your project to Bitbucket using an app password, do the following when you create a project\. For information, see [Create a build project \(console\)](create-project-console.md)\. 

1. For **Source provider**, choose **Bitbucket**\. 
**Note**  
CodeBuild does not support Bitbucket Server\.

1. For **Repository**, choose **Connect with a Bitbucket app password**\. 

1. In **Bitbucket username**, enter your Bitbucket user name\. 

1. In **Bitbucket app password**, enter your Bitbucket app password\. 

1. Choose **Save Bitbucket credentials**\. 

### Connect Bitbucket with an app password \(CLI\)<a name="access-tokens-bitbucket-cli"></a>

Follow these steps to use the AWS CLI to connect your project to Bitbucket using an app password\. For information about using the AWS CLI with AWS CodeBuild, see the [Command line reference](cmd-ref.md)\. 

1. Run the import\-source\-credentials command: 

   ```
   aws codebuild import-source-credentials --generate-cli-skeleton
   ```

   JSON\-formatted data appears in the output\. Copy the data to a file \(for example, `import-source-credentials.json`\) in a location on the local computer or instance where the AWS CLI is installed\. Modify the copied data as follows, and save your results\. 

   ```
   {
     "serverType": "BITBUCKET",
     "authType": "auth-type",
     "shouldOverwrite": "should-overwrite",
     "token": "token",
     "username": "username"
   }
   ```

   Replace the following: 
   + *auth\-type*: Required value\. The type of authentication used to connect to a Bitbucket repository\. Valid values include PERSONAL\_ACCESS\_TOKEN and BASIC\_AUTH\. You cannot use the CodeBuild API to create an OAUTH connection\. You must use the CodeBuild console instead\. 
   + *should\-overwrite*: Optional value\. Set to `false` to prevent overwriting the repository source credentials\. Set to `true` to overwrite the repository source credentials\. The default value is `true`\.
   + *token*: Required value\. For Bitbucket, this is the app password\. 
   + *username*: Optional value\. The Bitbucket user name when `authType` is BASIC\_AUTH\. This parameter is ignored for other types of source providers or connections\. 

1. To connect your account with an app password, switch to the directory that contains the `import-source-credentials.json` file you saved in step 1 and run the import\-source\-credentials command again\. 

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

   After your account is connected with an app password, you can use `create-project` to create your CodeBuild project\. For more information, see [Create a build project \(AWS CLI\)](create-project-cli.md)\. 

1. To view the connected app passwords, run the list\-source\-credentials command\. 

   ```
   aws codebuild list-source-credentials
   ```

   A JSON\-formatted `sourceCredentialsInfos` object appears in the output: 

   ```
   {
       "sourceCredentialsInfos": [
           {
               "authType": "auth-type",
               "serverType": "BITBUCKET", 
               "arn": "arn"
           }
       ]
   }
   ```

   The `sourceCredentialsObject` contains a list of connected source credentials information: 
   + The `authType` is the type of authentication used by credentials\. This can be `OAUTH`, `BASIC_AUTH`, or `PERSONAL_ACCESS_TOKEN`\. 
   + The `arn` is the ARN of the token\. 

1. To disconnect from a source provider and remove its app password, run the delete\-source\-credentials command with its ARN\. 

   ```
   aws codebuild delete-source-credentials --arn arn-of-your-credentials
   ```

   JSON\-formatted data is returned with an ARN of the deleted credentials\. 

   ```
   {
     "arn": "arn:aws:codebuild:region:account-id:token/server-type"
   }
   ```