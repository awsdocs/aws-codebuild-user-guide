--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Python Hello World Sample for AWS CodeBuild<a name="sample-python-hw"></a>

This Python sample tests whether an internal variable in code contains the string `Hello world!`\. It produces as build output a single file named `HelloWorld.py`\.

**Important**  
Running this sample may result in charges to your AWS account\. These include possible charges for AWS CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, and CloudWatch Logs\. For more information, see [AWS CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), and [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

**Note**  
 Did you know you can use AWS Cloud9 to work with the code in this topic? AWS Cloud9 is an online, cloud\-based integrated development environment \(IDE\) you can use to write, run, debug, and deploy code—using just a browser from an internet\-connected machine\. AWS Cloud9 includes a code editor, debugger, terminal, and essential tools, such as the AWS CLI and Git\. In many cases, you don't need to install files or configure your development machine to start working with code\. Learn more in the [AWS Cloud9 User Guide](https://docs.aws.amazon.com/cloud9/latest/user-guide/)\.

**Topics**
+ [Running the Sample](#sample-python-hw-running)
+ [Directory Structure](#sample-python-hw-dir)
+ [Files](#sample-python-hw-files)
+ [Related Resources](#w4aac11c47c33c17)

## Running the Sample<a name="sample-python-hw-running"></a>

To run this sample:

1. Create the files as described in the Directory Structure and Files sections of this topic, and then upload them to an Amazon S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\. 
**Important**  
Do not upload `(root directory name)`, just the files inside of `(root directory name)`\.   
If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the files, and then upload it to the input bucket\. Do not add `(root directory name)` to the ZIP file, just the files inside of `(root directory name)`\.

1. Create a build project, run the build, and view related build information by following the steps in [Run AWS CodeBuild Directly](how-to-run.md)\.

   If you use the AWS CLI to create the build project, the JSON\-formatted input to the`create-project` command might look similar to this\. \(Replace the placeholders with your own values\.\)

   ```
   {
     "name": "sample-python-project",
     "source": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-input-bucket/PythonSample.zip"
     },
     "artifacts": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-output-bucket",
       "packaging": "ZIP",
       "name": "PythonOutputArtifact.zip"
     },
     "environment": {
       "type": "LINUX_CONTAINER",
       "image": "aws/codebuild/python:3.5.2",
       "computeType": "BUILD_GENERAL1_SMALL"
     },
     "serviceRole": "arn:aws:iam::account-ID:role/role-name",
     "encryptionKey": "arn:aws:kms:region-ID:account-ID:key/key-ID"
   }
   ```

1. To get the build output artifact, open your Amazon S3 output bucket\.

1. Download the `PythonOutputArtifact.zip` file to your local computer or instance, and then extract the contents of the file\. In the extracted contents, get the `HelloWorld.py` file\. 

## Directory Structure<a name="sample-python-hw-dir"></a>

This sample assumes this directory structure\.

```
(root directory name)
    |-- buildspec.yml
    |-- HelloWorld.py
    `-- HelloWorld_tst.py
```

## Files<a name="sample-python-hw-files"></a>

This sample uses these files\.

`buildspec.yml` \(in `(root directory name)`\)

```
version: 0.2

phases:
  build:
    commands:
      - echo Build started on `date`
      - echo Compiling the Python code...
      - python HelloWorld_tst.py
  post_build:
    commands:
      - echo Build completed on `date`
artifacts:
  files:
    - HelloWorld.py
```

`HelloWorld.py` \(in `(root directory name)`\)

```
class HelloWorld:
  def __init__(self):
    self.message = 'Hello world!'
```

`HelloWorld_tst.py` \(in `(root directory name)`\)

```
import unittest
from HelloWorld import HelloWorld

class MyTestCase(unittest.TestCase):
  def test_default_greeting_set(self):
    hw = HelloWorld()
    self.assertEqual(hw.message, 'Hello world!')

if __name__ == '__main__':
  unittest.main()
```

## Related Resources<a name="w4aac11c47c33c17"></a>
+ For more information about getting started with AWS CodeBuild, see [Getting Started with AWS CodeBuild](getting-started.md)\.
+ For more information about troubleshooting problems with AWS CodeBuild, see [Troubleshooting AWS CodeBuild](troubleshooting.md)\.
+ For more information about limits in AWS CodeBuild, see [Limits for AWS CodeBuild](limits.md)\.