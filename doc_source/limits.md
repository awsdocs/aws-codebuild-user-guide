--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Limits for AWS CodeBuild<a name="limits"></a>

The following tables list the current limits in AWS CodeBuild\. These limits are for each supported AWS region for each AWS account, unless otherwise specified\. 

## Build Project Limits<a name="limits-build-projects"></a>


****  

| Resource | Default limit | 
| --- | --- | 
| Maximum number of build projects | 1,000 | 
| Length of a build project name | 2 to 255 characters, inclusive | 
| Allowed characters in a build project name | The letters A\-Z and a\-z, the numbers 0\-9, and the special characters \- and \_ | 
| Maximum length of a build project description | 255 characters | 
| Allowed characters in a build project description | Any | 
| Maximum number of build projects you can request information about at any one time by using the AWS CLI, or AWS SDKs | 100 | 
| Maximum number of tags you can associate with a build project | 50 | 
| Number of minutes you can specify in a build project for the build timeout of all related builds | 5 to 480 \(8 hours\) | 
| Number of subnets you can add under VPC configuration | 1 to 16 | 
| Number of security groups you can add under VPC configuration | 1 to 5 | 

## Build Limits<a name="limits-builds"></a>


****  

| Resource | Default limit | 
| --- | --- | 
| Maximum number of concurrent running builds \* | 60 | 
| Maximum number of builds you can request information about at any one time by using the AWS CLI, AWS SDKs | 100 | 
| Number of minutes you can specify for the build timeout of a single build | 5 to 480 \(8 hours\) | 

\* Limits for the maximum number of concurrent running builds vary, depending on the compute type\. For some platforms and compute types, the default is 20\. For a new account, the limit can be between 1 and 5\. To request a higher concurrent build limit or if you get a "Cannot have more than X active builds for the account" error, contact AWS support\.
