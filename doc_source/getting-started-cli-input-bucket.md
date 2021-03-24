# Step 3: Create two S3 buckets<a name="getting-started-cli-input-bucket"></a>

\(Previous step: [Step 2: Create the buildspec file](getting-started-cli-create-build-spec.md)\)

Although you can use a single bucket for this tutorial, two buckets makes it easier to see where the build input is coming from and where the build output is going\.
+ One of these buckets \(the *input bucket*\) stores the build input\. In this tutorial, the name of this input bucket is `codebuild-region-ID-account-ID-input-bucket`, where *region\-ID* is the AWS Region of the bucket and *account\-ID* is your AWS account ID\.
+ The other bucket \(the *output bucket*\) stores the build output\. In this tutorial, the name of this output bucket is `codebuild-region-ID-account-ID-output-bucket`\.

If you chose different names for these buckets, be sure to use them throughout this tutorial\.

These two buckets must be in the same AWS Region as your builds\. For example, if you instruct CodeBuild to run a build in the US East \(Ohio\) Region, these buckets must also be in the US East \(Ohio\) Region\.

For more information, see [Creating a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service User Guide*\. 

**Note**  
Although CodeBuild also supports build input stored in CodeCommit, GitHub, and Bitbucket repositories, this tutorial does not show you how to use them\. For more information, see [Plan a build](planning.md)\.

## Next step<a name="getting-started-cli-input-bucket-next"></a>

[Step 4: Upload the source code and the buildspec file](getting-started-cli-upload-source-code.md)