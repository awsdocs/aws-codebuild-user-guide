# Update a report group \(CLI\)<a name="update-report-group-cli"></a>

**To update a report group**

1. Create a file named `UpdateReportGroupInput.json`\.

1. Copy the following into `UpdateReportGroupInput.json`: 

   ```
   {
       "arn": "",
       "exportConfig": {
           "exportConfigType": "S3",
           "s3Destination": {
               "bucket": "bucket-name", 
               "path": "path", 
               "packaging": "NONE | ZIP",
               "encryptionDisabled": "false",
               "encryptionKey": "your-key"
            }
        },
        "tags": [
           {
               "key": "tag-key",
               "value": "tag-value"
           }
        ]
   }
   ```

1.  Enter the ARN of your report group in the `arn` line \(for example, `"arn":"arn:aws:codebuild:region:123456789012:report-group/report-group-1")`\. 

1.  Update `UpdateReportGroupInput.json` with the updates you want to apply to your report group\. 
   +  If you want to update your report group to export raw test result files to an S3 bucket, update the `exportConfig` section\. Replace `bucket-name` with your S3 bucket name and `path` with the path in your S3 bucket that you want to export the files to\. If you want to compress the exported files, for `packaging`, specify `ZIP`\. Otherwise, specify `NONE`\. Use `encryptionDisabled` to specify whether to encrypt the exported files\. If you encrypt the exported files, enter your customer master key \(CMK\)\.
   +  If you want to update your report group so that it does not export raw test result files to an S3 bucket, update the `exportConfig` section with the following JSON: 

     ```
     { 
       "exportConfig": {
           "exportConfigType": "NO_EXPORT"
       }
     }
     ```
   +  If you want to update the report group's tags, update the `tags` section\. You can change, add, or remove tags\. If you want to remove all tags, update it with the following JSON: 

     ```
     "tags": []
     ```

1.  Run the following command: 

   ```
   aws codebuild upate-report-group \
   --cli-input-json file://UpdateReportGroupInput.json
   ```