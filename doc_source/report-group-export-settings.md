# Update a Report Group<a name="report-group-export-settings"></a>


|  | 
| --- |
| The test reporting feature is in preview release for CodeBuild and is subject to change\. | 

 When you update a report group, you can specify information about whether to export the raw test result data to files in an S3 bucket\. If you choose to export to an S3 bucket, you can specify the following for your report group: 
+ Whether the raw test results files are compressed in a ZIP file\.
+ Whether the raw test result files are encrypted\. You can specify encryption with one of the following:
  + A customer master key \(CMK\) for Amazon S3 that is managed by the AWS Key Management Service\. 
  + A CMK that you create and configure\.

 For more information, see [Data Encryption](security-encryption.md)\. 