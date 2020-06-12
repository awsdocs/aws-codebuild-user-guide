# Data protection in AWS CodeBuild<a name="data-protection"></a>

 AWS CodeBuild conforms to the AWS [shared responsibility model](https://aws.amazon.com/compliance/shared-responsibility-model/), which includes regulations and guidelines for data protection\. AWS is responsible for protecting the global infrastructure that runs all the AWS services\. AWS maintains control over data hosted on this infrastructure, including the security configuration controls for handling customer content and personal data\. AWS customers and APN partners, acting either as data controllers or data processors, are responsible for any personal data that they put in the AWS Cloud\. 

 For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\), so that each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways: 
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use TLS to communicate with AWS resources\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls in AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3\.

We strongly recommend that you never put sensitive identifying information, such as your customers' account numbers, into free\-form fields such as a **Name** field\. This includes when you work with CodeBuild or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into CodeBuild or other services might get picked up for inclusion in diagnostic logs\. When you provide a URL to an external server, don't include credentials information in the URL to validate your request to that server\. 

 To protect sensitive information, the following are hidden in CodeBuild logs: 
+  AWS access key IDs\. For more information, see [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in in the *AWS Identity and Access Management User Guide*\. 
+  Strings specified using the Parameter Store\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store console walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\. 
+  Strings specified using AWS Secrets Manager\. For more information, see [Key management](security-key-management.md)\. 

For more information about data protection, see the [AWS shared responsibility model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

**Topics**
+ [Data encryption](security-encryption.md)
+ [Key management](security-key-management.md)
+ [Traffic privacy](security-traffic-privacy.md)