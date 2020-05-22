# Delete builds in AWS CodeBuild<a name="delete-builds"></a>

You can use the AWS CLI or the AWS SDKs to delete builds in AWS CodeBuild\.

## Delete builds \(AWS CLI\)<a name="delete-builds-cli"></a>

Run the `batch-delete-builds` command:

```
aws codebuild batch-delete-builds --ids ids
```

In the preceding command, replace the following placeholder:
+ *ids*: Required string\. The IDs of the builds to delete\. To specify multiple builds, separate each build ID with a space\. To get a list of build IDs, see the following topics:
  + [View a list of build IDs \(AWS CLI\)](view-build-list.md#view-build-list-cli)
  + [View a list of build IDs for a build project \(AWS CLI\)](view-builds-for-project.md#view-builds-for-project-cli)

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

## Delete builds \(AWS SDKs\)<a name="delete-builds-sdks"></a>

For information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and tools reference](sdk-ref.md)\.