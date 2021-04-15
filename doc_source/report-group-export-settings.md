# Update a report group<a name="report-group-export-settings"></a>

 When you update a report group, you can specify information about whether to export the raw test result data to files in an Amazon S3 bucket\. If you choose to export to an S3 bucket, you can specify the following for your report group: 
+ Whether the raw test results files are compressed in a ZIP file\.
+ Whether the raw test result files are encrypted\. You can specify encryption with one of the following:
  + A customer master key \(CMK\) for Amazon S3 that is managed by the AWS Key Management Service\. 
  + A CMK that you create and configure\.

 For more information, see [Data encryption](security-encryption.md)\. 

If you use the AWS CLI to update a report group, you can also update or add tags\. For more information, see [Tagging report groups in AWS CodeBuildTag a report group](how-to-tag-report-group.md)\.

**Note**  
The CodeBuild service role specified in the project is used for permissions to upload to the S3 bucket\.

**Topics**
+ [Update a report group \(console\)](update-report-group-console.md)
+ [Update a report group \(CLI\)](update-report-group-cli.md)