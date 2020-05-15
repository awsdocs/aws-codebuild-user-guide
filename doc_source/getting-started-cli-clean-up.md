# Step 10: Delete the S3 input bucket<a name="getting-started-cli-clean-up"></a>

\(Previous step: [Step 9: Get the build output artifact](getting-started-cli-output.md)\)

To prevent ongoing charges to your AWS account, you can delete the input bucket used in this tutorial\. For instructions, see [Deleting or Emptying a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/delete-or-empty-bucket.html) in the *Amazon Simple Storage Service Developer Guide*\.

If you are using the IAM user or an administrator IAM user to delete this bucket, the user must have more access permissions\. Add the following statement between the markers \(*\#\#\# BEGIN ADDING STATEMENT HERE \#\#\#* and *\#\#\# END ADDING STATEMENTS HERE \#\#\#*\) to an existing access policy for the user\. 

The ellipses \(\.\.\.\) in this statement are used for brevity\. Do not remove any statements in the existing access policy\. Do not enter these ellipses into the policy\.

```
{
  "Version": "2012-10-17",
  "Id": "...",
  "Statement": [
    ### BEGIN ADDING STATEMENT HERE ###
    {
      "Effect": "Allow",
      "Action": [
        "s3:DeleteBucket",
        "s3:DeleteObject"
      ],
      "Resource": "*"
    }
    ### END ADDING STATEMENT HERE ###
  ]
}
```

## Next step<a name="getting-started-cli-clean-up-next"></a>

[Wrapping up](getting-started-cli-next-steps.md)