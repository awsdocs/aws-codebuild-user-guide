# Create a report group \(AWS CloudFormation\)<a name="test-report-group-create-cfn"></a>

 **To create a test report using the AWS CloudFormation template** 

 You can use an AWS CloudFormation template file to create and provision a report group\. For more information, see [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)\. 

 The following AWS CloudFormation YAML template creates a report group that does not export raw test result files\. 

```
Resources:
  CodeBuildReportGroup:
    Type: AWS::CodeBuild::ReportGroup
    Properties:
      Name: my-report-group-name
      Type: TEST
      ExportConfig:
        ExportConfigType: NO_EXPORT
```

 The following AWS CloudFormation YAML template creates a report group that exports raw test result files to an Amazon S3 bucket\. 

```
Resources:
  CodeBuildReportGroup:
    Type: AWS::CodeBuild::ReportGroup
    Properties:
      Name: my-report-group-name
      Type: TEST
      ExportConfig:
        ExportConfigType: S3
        S3Destination:
          Bucket: my-s3-bucket-name
          Path: path-to-folder-for-exported-files
          Packaging: ZIP
          EncryptionKey: my-KMS-encryption-key
          EncryptionDisabled: false
```

**Note**  
The CodeBuild service role specified in the project is used for permissions to upload to the S3 bucket\.