--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Delete Builds in AWS CodeBuild<a name="delete-builds"></a>

To delete builds in AWS CodeBuild, you can use the AWS CLI, or the AWS SDKs\.

## Delete Builds \(AWS CLI\)<a name="delete-builds-cli"></a>

Run the `batch-delete-builds` command:

```
aws codebuild batch-delete-builds --ids ids
```

In the preceding command, replace the following placeholder:
+ *ids*: Required string\. The IDs of the builds to delete\. To specify multiple builds, separate each build ID with a space\. To get a list of build IDs, see the following topics:
  + [View a List of Build IDs \(AWS CLI\)](view-build-list.md#view-build-list-cli)
  + [View a List of Build IDs for a Build Project \(AWS CLI\)](view-builds-for-project.md#view-builds-for-project-cli)

If successful, a `buildsDeleted` array appears in the output, containing the Amazon Resource Name \(ARN\) of each build that was successfully deleted\. Information about builds that were not successfully deleted appears in output within a `buildsNotDeleted` array\.

For example, if you run this command:

```
aws codebuild batch-delete-builds --ids my-demo-build-project:f8b888d2-5e1e-4032-8645-b115195648EX my-other-demo-build-project:a18bc6ee-e499-4887-b36a-8c90349c7eEX
```

Information similar to the following appears in the output:

```
{
  "buildsNotDeleted": [
    {
      "id": "arn:aws:codebuild:us-west-2:123456789012:build/my-demo-build-project:f8b888d2-5e1e-4032-8645-b115195648EX", 
      "statusCode": "BUILD_IN_PROGRESS"
    }
  ], 
  "buildsDeleted": [
    "arn:aws:codebuild:us-west-2:123456789012:build/my-other-demo-build-project:a18bc6ee-e499-4887-b36a-8c90349c7eEX"
  ]
}
```

## Delete Builds \(AWS SDKs\)<a name="delete-builds-sdks"></a>

For information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.