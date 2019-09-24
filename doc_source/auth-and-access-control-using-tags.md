# Using Tags to Control Access to CodeBuild Resources<a name="auth-and-access-control-using-tags"></a>

Conditions in IAM policy statements are part of the syntax that you can use to specify permissions to CodeBuild project\-based actions\. You can create a policy that allows or denies actions on projects based on the tags associated with those projects, and then apply those policies to the IAM groups you configure for managing IAM users\. For information about applying tags to a project using the console or AWS CLI, see [Create a Build Project in CodeBuild](create-project.md)\. For information about applying tags using the CodeBuild SDK, see [CreateProject ](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateProject.html#API_CreateProject_RequestSyntax) and [Tags](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_Tag.html) in the *CodeBuild API Reference*\. For information about using tags to control access to AWS resources, see [Controlling Access to AWS Resources Using Resource Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html)\.

**Example Example 1: Limit CodeBuild Project Actions Based on Resource Tags**  
 The following example denies all `BatchGetProjects` actions on projects tagged with the key *Environment* with the key value of *Production*\. A user's administrator must attach this IAM policy in addition to the managed user policy to unauthorized IAM users\. The `aws:ResourceTag` condition key is used to control access to resources based on their tags\.   

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "codebuild:BatchGetProjects"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringEquals": {
          "aws:ResourceTag/Environment": "Production"
        }
      }
    }
  ]
}
```

**Example Example 2: Limit CodeBuild Project Actions Based on Request Tags**  
The following policy denies users permission to the `CreateProject` action if the request contains a tag with the key *Environment* and the key value *Production*\. In addition, the policy prevents these unauthorized users from modifying projects by using the `aws:TagKeys` condition key to not allow `UpdateProject` if the request contains a tag with the key *Environment*\. An administrator must attach this IAM policy in addition to the managed user policy to users who are not authorized to perform these actions\. The `aws:RequestTag` condition key is used to control which tags can be passed in an IAM request  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "codebuild:CreateProject"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringEquals": {
          "aws:RequestTag/Environment": "Production"
        }
      }
    },
    {
      "Effect": "Deny",
      "Action": [
        "codebuild:UpdateProject"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringEquals": {
          "aws:TagKeys": ["Environment"]
        }
      }
    }
  ]
}
```