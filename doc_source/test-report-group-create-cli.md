# Create a report group \(CLI\)<a name="test-report-group-create-cli"></a>

**To create a test report**

1. Create a file named `CreateReportGroup.json`\.

1.  Depending on your requirements, copy one of the following JSON code snippets into `CreateReportGroup.json`: 
   + Use the following JSON to specify that your test report group exports raw test result files to an Amazon S3 bucket\. 

     ```
     {
     "name": "report-name", 
     "type": "TEST", 
     "exportConfig": {
       "exportConfigType": "S3",
       "s3Destination": {
         "bucket": "bucket-name", 
         "path": "path", 
         "packaging": "NONE | ZIP",
         "encryptionDisabled": "false",
         "encryptionKey": "your-key"
       },
       "tags": [
         {
           "key": "tag-key",
           "value": "tag-value"
         }
       ]
     }
     ```

      Replace `bucket-name` with your S3 bucket name and `path` with the path in your S3 bucket to where you want to export the files\. If you want to compress the exported files, for `packaging`, specify `ZIP`\. Otherwise, specify `NONE`\. Use `encryptionDisabled` to specify whether to encrypt the exported files\. If you encrypt the exported files, enter your customer master key \(CMK\)\. For more information, see [Update a report group](report-group-export-settings.md)\.
   + Use the following JSON to specify that your test report does not export raw test files: 

     ```
     {
       "name": "report-name", 
       "type": "TEST", 
       "exportConfig": {
           "exportConfigType": "NO_EXPORT"
       }
     }
     ```
**Note**  
The CodeBuild service role specified in the project is used for permissions to upload to the S3 bucket\.

1.  Run the following command: 

   ```
   aws codebuild create-report-group \
   --cli-input-json file://CreateReportGroupInput.json \
   --region us-east-2
   ```