# View tags for a report group<a name="how-to-tag-report-group-list"></a>

Tags can help you identify and organize your AWS resources and manage access to them\. For more information about using tags, see the [Tagging best practices](https://d1.awsstatic.com/whitepapers/aws-tagging-best-practices.pdf) whitepaper\. For examples of tag\-based access policies, see [Deny or allow actions on report groups based on resource tags](auth-and-access-control-using-tags.md#report-group-tag-policy-example)\.

## View tags for a report group \(console\)<a name="how-to-tag-report-group-list-console"></a>

You can use the CodeBuild console to view the tags associated with a CodeBuild report group\. 

1. Open the CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. In **Report groups**, choose the name of the report group where you want to view tags\.

1. In the navigation pane, choose **Settings**\.

## View tags for a report group \(AWS CLI\)<a name="how-to-tag-report-group-list-cli"></a>

Follow these steps to use the AWS CLI to view the AWS tags for a report group\. If no tags have been added, the returned tags list is empty\.

1.  Use the console or the AWS CLI to locate the ARN of your report group\. Make a note of it\. 

------
#### [ AWS CLI ]

    Run the following command\. 

   ```
   aws list-report-groups
   ```

    This command returns JSON\-formatted information similar to the following: 

   ```
   {
       "reportGroups": [
           "arn:aws:codebuild:region:123456789012:report-group/report-group-1",
           "arn:aws:codebuild:region:123456789012:report-group/report-group-2",
           "arn:aws:codebuild:region:123456789012:report-group/report-group-3"
       ]
   }
   ```

   A report group ARN ends with its name, which you can use to identify the ARN for your report group\.

------
#### [ Console ]

   1. Open the CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

   1. In **Report groups**, choose the name of your report group with the tags you want to view\.

   1.  In **Configuration** locate your report group's ARN\. 

------

1.  Run the following command\. Use the ARN you made a note of for the `--report-group-arns` parameter\. 

   ```
   aws codebuild batch-get-report-groups --report-group-arns arn:aws:codebuild:region:123456789012:report-group/report-group-name
   ```

    If successful, this command returns JSON\-formatted information that contains a `tags` section similar to the following: 

   ```
   {
       ...                        
       "tags": {
           "Status": "Secret",
           "Project": "TestBuild"
       }
       ...
   }
   ```