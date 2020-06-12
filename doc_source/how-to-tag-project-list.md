# View tags for a project<a name="how-to-tag-project-list"></a>

Tags can help you identify and organize your AWS resources and manage access to them\. For more information about using tags, see the [Tagging best practices](https://d1.awsstatic.com/whitepapers/aws-tagging-best-practices.pdf) whitepaper\. For examples of tag\-based access policies, see [Using tags to control access to AWS CodeBuild resources](auth-and-access-control-using-tags.md)\.

## View tags for a project \(console\)<a name="how-to-tag-project-list-console"></a>

You can use the CodeBuild console to view the tags associated with a CodeBuild project\. 

1. Open the CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. In **Build projects**, choose the name of the project where you want to view tags\.

1. In the navigation pane, choose **Settings**\. Choose **Build project tags**\. 

## View tags for a project \(AWS CLI\)<a name="how-to-tag-project-list-cli"></a>

To view tags for a build project, run the following command\. Use the name of your project for the `--names` parameter\.

```
aws codebuild batch-get-projects --names your-project-name
```

If successful, this command returns JSON\-formatted information about your build project that includes something like the following:

```
{
    "tags": {
        "Status": "Secret",
        "Team": "JanesProject"
    }
}
```

If the project does not have tags, the `tags` section is empty:

```
"tags": []
```