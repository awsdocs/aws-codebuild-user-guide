# Tagging projects in AWS CodeBuild<a name="how-to-tag-project"></a>

A *tag* is a custom attribute label that you or AWS assigns to an AWS resource\. Each AWS tag has two parts:
+ A *tag key* \(for example, `CostCenter`, `Environment`, `Project`, or `Secret`\)\. Tag keys are case sensitive\.
+ An optional field known as a *tag value* \(for example, `111122223333`, `Production`, or a team name\)\. Omitting the tag value is the same as using an empty string\. Like tag keys, tag values are case sensitive\.

Together these are known as key\-value pairs\. For information about the number of tags you can have on a project and restrictions on tag keys and values, see [Tags](limits.md#tag-limits)\.

Tags help you identify and organize your AWS resources\. Many AWS services support tagging, so you can assign the same tag to resources from different services to indicate that the resources are related\. For example, you can assign the same tag to a CodeBuild project that you assign to an S3 bucket\. For more information about using tags, see the [Tagging best practices](https://d1.awsstatic.com/whitepapers/aws-tagging-best-practices.pdf) whitepaper\. 

In CodeBuild, the primary resources are the project and the report group\. You can use the CodeBuild console, the AWS CLI, CodeBuild APIs, or AWS SDKs to add, manage, and remove tags for a project\. In addition to identifying, organizing, and tracking your project with tags, you can use tags in IAM policies to help control who can view and interact with your project\. For examples of tag\-based access policies, see [Using tags to control access to AWS CodeBuild resources](auth-and-access-control-using-tags.md)\.

**Topics**
+ [Add a tag to a project](how-to-tag-project-add.md)
+ [View tags for a project](how-to-tag-project-list.md)
+ [Edit tags for a project](how-to-tag-project-update.md)
+ [Remove a tag from a project](how-to-tag-project-delete.md)