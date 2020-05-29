# Remove a tag from a report group<a name="how-to-tag-report-group-delete"></a>

You can remove one or more tags associated with a report group\. Removing a tag does not delete the tag from other AWS resources that are associated with that tag\.

**Important**  
Removing tags for a report group can impact access to that report group\. Before you remove a tag from a report group, make sure to review any IAM policies that might use the key or value for a tag to control access to resources such as report groups\. For examples of tag\-based access policies, see [Using tags to control access to AWS CodeBuild resources](auth-and-access-control-using-tags.md)\.

## Remove a tag from a report group \(console\)<a name="how-to-tag-report-group-delete-console"></a>

You can use the CodeBuild console to remove the association between a tag and a CodeBuild report group\. 

1. Open the CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. In **Report groups**, choose the name of the report group where you want to remove tags\.

1. In the navigation pane, choose **Settings**\.

1. Choose **Edit**\.

1. Find the tag you want to remove, and then choose **Remove tag**\.

1. When you have finished removing tags, choose **Submit**\.

## Remove a tag from a report group \(AWS CLI\)<a name="how-to-tag-report-group-delete-cli"></a>

Follow these steps to use the AWS CLI to remove a tag from a CodeBuild report group\. Removing a tag does not delete it, but simply removes the association between the tag and the report group\. 

**Note**  
If you delete a CodeBuild report group, all tag associations are removed from the deleted report group\. You do not have to remove tags before you delete a report group\.

 To delete one or more tags from a report group, see [Edit tags for a report group \(AWS CLI\)](how-to-tag-report-group-update.md#how-to-tag-report-group-update-cli)\. Update the `tags` section in the JSON\-formatted data with an updated list of tags that does not contain the ones you want to delete\. If you want to delete all tags, update the `tags` section to:

```
"tags: []"
```