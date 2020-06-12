# Update a report group \(console\)<a name="update-report-group-console"></a>

**To update a report group**

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  In the navigation pane, choose **Report groups**\. 

1. Choose the report group you want to update\. 

1. Choose **Edit**\.

1. Select or clear **Backup to Amazon S3**\. If you selected this option, specify your export settings:

   1. For **S3 bucket name**, enter the name of the S3 bucket\. 

   1. For **Path prefix**, enter the path in your S3 bucket where you want to upload your test results\. 

   1.  Select **Compress test result data in a zip file** to compress your raw test result data files\. 

   1.  Expand **Additional configuration** to display encryption options\. Choose one of the following: 
      +  **Default AWS managed key** to use a customer master key \(CMK\) for Amazon S3 that is managed by the AWS Key Management Service\. In CodeBuild, the default CMK is for Amazon S3 and uses the format `aws/S3`\. For more information, see [Customer managed CMKs](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#customer-cmk) in the *AWS Key Management Service User Guide*\. This is the default encryption option\.
      +  **Choose a custom key** to use a CMK that you create and configure\. For **AWS KMS encryption key**, enter the ARN of your encryption key\. Its format is ` arn:aws:kms:region-id:aws-account-id:key/key-id`\. For more information, see [Creating KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service User Guide*\. 
      +  **Disable artifact encryption** to disable encryption\. You might choose this option if you want to share your test results or publish them to a static website\. \(A dynamic website can run code to decrypt test results\.\)