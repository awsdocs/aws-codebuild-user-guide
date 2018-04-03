# Docker Images Provided by AWS CodeBuild<a name="build-env-ref-available"></a>

AWS CodeBuild manages the following Docker images that are available in the AWS CodeBuild and AWS CodePipeline consoles\.

**Note**  
If you do not find your image on this page, it most likely contains components that are no longer supported by a vendor\. Images with one or more unsupported components are not available from the AWS CodeBuild console or the AWS CodeBuild SDK\. The images might still be available in the CLI, but they are not supported and will not be updated\.


****  

| Platform | Programming language or framework | Runtime version | Image identifier | Definition | 
| --- | --- | --- | --- | --- | 
| Ubuntu 14\.04 | \(Base image\) |  | aws/codebuild/ubuntu\-base:14\.04 | [ubuntu/ubuntu\-base/14\.04](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/ubuntu-base/14.04) | 
| Ubuntu 14\.04 | Android | 24\.4\.1 | aws/codebuild/android\-java\-8:24\.4\.1 | [ubuntu/android\-java\-8/24\.4\.1](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/android-java-8/24.4.1) | 
| Ubuntu 14\.04 | Docker | 17\.09\.0 | aws/codebuild/docker:17\.09\.0 | [ubuntu/docker/17\.09\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/docker/17.09.0) | 
| Ubuntu 14\.04 | Golang | 1\.10 | aws/codebuild/golang:1\.10 | [ubuntu/golang/1\.10](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/golang/1.10) | 
| Ubuntu 14\.04 | Java | 8 | aws/codebuild/java:openjdk\-8 | [ubuntu/java/openjdk\-8](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/java/openjdk-8) | 
| Ubuntu 14\.04 | Java | 9 | aws/codebuild/java:openjdk\-9 | [ubuntu/java/openjdk\-9](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/java/openjdk-9) | 
| Ubuntu 14\.04 | Node\.js | 6\.3\.1 | aws/codebuild/nodejs:6\.3\.1 | [ubuntu/nodejs/6\.3\.1](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/nodejs/6.3.1) | 
| Ubuntu 14\.04 | Node\.js | 4\.4\.7 | aws/codebuild/nodejs:4\.4\.7 | [ubuntu/nodejs/4\.4\.7](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/nodejs/4.4.7) | 
| Ubuntu 14\.04 | Node\.js | 4\.3\.2 | aws/codebuild/nodejs:4\.3\.2 | [ubuntu/nodejs/4\.3\.2](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/nodejs/4.3.2) | 
| Ubuntu 14\.04 | PHP | 5\.6 | aws/codebuild/php:5\.6 | [ubuntu/php/5\.6](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/php/5.6) | 
| Ubuntu 14\.04 | PHP | 7\.0 | aws/codebuild/php:7\.0 | [ubuntu/php/7\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/php/7.0) | 
| Ubuntu 14\.04 | Python | 3\.5\.2 | aws/codebuild/python:3\.5\.2 | [ubuntu/python/3\.5\.2](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/python/3.5.2) | 
| Ubuntu 14\.04 | Python | 3\.4\.5 | aws/codebuild/python:3\.4\.5 | [ubuntu/python/3\.4\.5](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/python/3.4.5) | 
| Ubuntu 14\.04 | Python | 3\.3\.6 | aws/codebuild/python:3\.3\.6 | [ubuntu/python/3\.3\.6](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/python/3.3.6) | 
| Ubuntu 14\.04 | Python | 2\.7\.12 | aws/codebuild/python:2\.7\.12 | [ubuntu/python/2\.7\.12](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/python/2.7.12) | 
| Ubuntu 14\.04 | Ruby | 2\.3\.1 | aws/codebuild/ruby:2\.3\.1 | [ubuntu/ruby/2\.3\.1](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/ruby/2.3.1) | 
| Ubuntu 14\.04 | Ruby | 2\.2\.5 | aws/codebuild/ruby:2\.2\.5 | [ubuntu/ruby/2\.2\.5](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/ruby/2.2.5) | 
| Ubuntu 14\.04 | \.NET Core | 1\.1 | aws/codebuild/dot\-net:core\-1 | [ubuntu/dot\-net/core\-1](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/dot-net/core-1) | 
| Ubuntu 14\.04 | \.NET Core | 2\.0 | aws/codebuild/dot\-net:core\-2\.0 | [ubuntu/dot\-net/core\-2](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/dot-net/core-2) | 

AWS CodeBuild also manages the following Docker images that are not in the AWS CodeBuild and AWS CodePipeline consoles\.


****  

| Platform | Programming language or framework | Runtime version | Additional components | Image identifier | 
| --- | --- | --- | --- | --- | 
| Amazon Linux 2016\.03, 64\-bit v2\.3\.2 | Golang | 1\.6 | Apache Maven 3\.3\.3, Apache Ant 1\.9\.6, Gradle 2\.7 | aws/codebuild/eb\-go\-1\.6\-amazonlinux\-64:2\.3\.2 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.6 | Golang | 1\.5\.3 | Apache Maven 3\.3\.3, Apache Ant 1\.9\.6, Gradle 2\.7 | aws/codebuild/eb\-go\-1\.5\-amazonlinux\-64:2\.1\.6 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.3 | Golang | 1\.5\.3 | Apache Maven 3\.3\.3, Apache Ant 1\.9\.6, Gradle 2\.7  | aws/codebuild/eb\-go\-1\.5\-amazonlinux\-64:2\.1\.3 | 
| Amazon Linux 2016\.03, 64\-bit v2\.4\.3 | Java | 1\.8\.0 | Apache Maven 3\.3\.3, Apache Ant 1\.9\.6, Gradle 2\.7 | aws/codebuild/eb\-java\-8\-amazonlinux\-64:2\.4\.3 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.6 | Java | 1\.8\.0 | Apache Maven 3\.3\.3, Apache Ant 1\.9\.6, Gradle 2\.7 | aws/codebuild/eb\-java\-8\-amazonlinux\-64:2\.1\.6 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.3 | Java | 1\.8\.0 | Apache Maven 3\.3\.3, Apache Ant 1\.9\.6, Gradle 2\.7 | aws/codebuild/eb\-java\-8\-amazonlinux\-64:2\.1\.3 | 
| Amazon Linux 2016\.03, 64\-bit v2\.4\.3 | Java | 1\.7\.0 | Apache Maven 3\.3\.3, Apache Ant 1\.9\.6, Gradle 2\.7 | aws/codebuild/eb\-java\-7\-amazonlinux\-64:2\.4\.3 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.6 | Java | 1\.7\.0 | Apache Maven 3\.3\.3, Apache Ant 1\.9\.6, Gradle 2\.7 | aws/codebuild/eb\-java\-7\-amazonlinux\-64:2\.1\.6 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.3 | Java | 1\.7\.0 | Apache Maven 3\.3\.3, Apache Ant 1\.9\.6, Gradle 2\.7 | aws/codebuild/eb\-java\-7\-amazonlinux\-64:2\.1\.3 | 
| Amazon Linux 2016\.03, 64\-bit v4\.0\.0 | Node\.js | 6\.10\.0 | Git 2\.7\.4, npm 2\.15\.5 | aws/codebuild/eb\-nodejs\-6\.10\.0\-amazonlinux\-64:4\.0\.0 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.3 | Node\.js | 4\.4\.6 | Git 2\.7\.4, npm 2\.15\.5 | aws/codebuild/eb\-nodejs\-4\.4\.6\-amazonlinux\-64:2\.1\.3 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.6 | Python | 3\.4\.3 | meld3 1\.0\.2, pip 7\.1\.2, setuptools 18\.4 | aws/codebuild/eb\-python\-3\.4\-amazonlinux\-64:2\.1\.6 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.3 | Python | 3\.4\.3 | meld3 1\.0\.2, pip 7\.1\.2, setuptools 18\.4 | aws/codebuild/eb\-python\-3\.4\-amazonlinux\-64:2\.1\.3 | 
| Amazon Linux 2016\.03, 64\-bit v2\.3\.2 | Python | 3\.4 | meld3 1\.0\.2, pip 7\.1\.2, setuptools 18\.4 | aws/codebuild/eb\-python\-3\.4\-amazonlinux\-64:2\.3\.2 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.6 | Python | 2\.7\.10 | meld3 1\.0\.2, pip 7\.1\.2, setuptools 18\.4 | aws/codebuild/eb\-python\-2\.7\-amazonlinux\-64:2\.1\.6 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.3 | Python | 2\.7\.10 | meld3 1\.0\.2, pip 7\.1\.2, setuptools 18\.4 | aws/codebuild/eb\-python\-2\.7\-amazonlinux\-64:2\.1\.3 | 
| Amazon Linux 2016\.03, 64\-bit v2\.3\.2 | Python | 2\.7 | meld3 1\.0\.2, pip 7\.1\.2, setuptools 18\.4 | aws/codebuild/eb\-python\-2\.7\-amazonlinux\-64:2\.3\.2 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.6 | Python | 2\.6\.9 | meld3 1\.0\.2, pip 7\.1\.2, setuptools 18\.4 | aws/codebuild/eb\-python\-2\.6\-amazonlinux\-64:2\.1\.6 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.3 | Python | 2\.6\.9 | meld3 1\.0\.2, pip 7\.1\.2, setuptools 18\.4 | aws/codebuild/eb\-python\-2\.6\-amazonlinux\-64:2\.1\.3 | 
| Amazon Linux 2016\.03, 64\-bit v2\.3\.2 | Python | 2\.6 | meld3 1\.0\.2, pip 7\.1\.2, setuptools 18\.4 | aws/codebuild/eb\-python\-2\.6\-amazonlinux\-64:2\.3\.2 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.6 | Ruby | 2\.3\.1 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-2\.3\-amazonlinux\-64:2\.1\.6 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.3 | Ruby | 2\.3\.1 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-2\.3\-amazonlinux\-64:2\.1\.3 | 
| Amazon Linux 2016\.03, 64\-bit v2\.3\.2 | Ruby | 2\.3 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-2\.3\-amazonlinux\-64:2\.3\.2 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.6 | Ruby | 2\.2\.5 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-2\.2\-amazonlinux\-64:2\.1\.6 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.3 | Ruby | 2\.2\.5 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-2\.2\-amazonlinux\-64:2\.1\.3 | 
| Amazon Linux 2016\.03, 64\-bit v2\.3\.2 | Ruby | 2\.2 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-2\.2\-amazonlinux\-64:2\.3\.2 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.6 | Ruby | 2\.1\.9 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-2\.1\-amazonlinux\-64:2\.1\.6 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.3 | Ruby | 2\.1\.9 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-2\.1\-amazonlinux\-64:2\.1\.3 | 
| Amazon Linux 2016\.03, 64\-bit v2\.3\.2 | Ruby | 2\.1 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-2\.1\-amazonlinux\-64:2\.3\.2 | 
| Amazon Linux 2016\.03, 64\-bit v2\.3\.2 | Ruby | 2\.0 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-2\.0\-amazonlinux\-64:2\.3\.2 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.6 | Ruby | 2\.0\.0 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-2\.0\-amazonlinux\-64:2\.1\.6 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.3 | Ruby | 2\.0\.0 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-2\.0\-amazonlinux\-64:2\.1\.3 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.6 | Ruby | 1\.9\.3 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-1\.9\-amazonlinux\-64:2\.1\.6 | 
| Amazon Linux 2016\.03, 64\-bit v2\.1\.3 | Ruby | 1\.9\.3 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-1\.9\-amazonlinux\-64:2\.1\.3 | 
| Amazon Linux 2016\.03, 64\-bit v2\.3\.2 | Ruby | 1\.9 | Bundler, RubyGems | aws/codebuild/eb\-ruby\-1\.9\-amazonlinux\-64:2\.3\.2 | 
|  For more information about the Docker images that contain `eb-` in their identifier, see [Supported Platforms](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/concepts.platforms.html) and [Platform History](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/platform-history.html) in the *AWS Elastic Beanstalk Developer Guide*\. Docker images that contain `eb-` in their identifier are available for use in Elastic Beanstalk; but are not available in the AWS CodeBuild and AWS CodePipeline consoles\.  | 

You can use a build specification to install other components \(for example, the AWS CLI, Apache Maven, Apache Ant, Mocha, RSpec, or similar\) during the `install` build phase\. For more information, see [Build Spec Example](build-spec-ref.md#build-spec-ref-example)\.

AWS CodeBuild frequently updates the list of Docker images\. To get the most current list, do one of the following:
+ In the AWS CodeBuild console, in the **Create project** wizard or **Update project** page, for **Environment image**, choose **Use an image managed by AWS CodeBuild**\. Choose from the **Operating system**, **Runtime**, and **Version** drop\-down lists\. For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console) or [Change a Build Project's Settings \(Console\)](change-project.md#change-project-console)\.
+ In the AWS CodePipeline console, in the **Create pipeline** wizard on the **Step 3: Build** page, or in the **AWS CodeBuild** section of the **Add action** or **Edit action** pane, choose **Create a new build project**\. In **Environment: How to build**, for **Environment image**, choose **Use an image managed by AWS CodeBuild**\. Choose from the **Operating system**, **Runtime**, and **Version** drop\-down lists\. For more information, see [Create a Pipeline that Uses AWS CodeBuild \(AWS CodePipeline Console\)](how-to-create-pipeline.md#how-to-create-pipeline-console) or [Add an AWS CodeBuild Build Action to a Pipeline \(AWS CodePipeline Console\)](how-to-create-pipeline.md#how-to-create-pipeline-add)\.
+ For the AWS CLI, run the `list-curated-environment-images` command:

  ```
  aws codebuild list-curated-environment-images
  ```
+ For the AWS SDKs, call the `ListCuratedEnvironmentImages` operation for your target programming language\. For more information, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.

To confirm the version of a component installed on a Docker image, you can run a command during a build\. The version number for the component will appear in the output\. For example, include one or more of the following commands in your build specification:
+ For Apache Ant, run `ant -version`\.
+ For Apache Maven, run `mvn -version`\.
+ For the AWS CLI, run `aws --version`\.
+ For Bundler, run `bundle version`\.
+ For Git, run `git --version`\.
+ For Gradle, run `gradle --version`\.
+ For Java, run `java -version`\.
+ For NPM, run `npm --version`\.
+ For PHP, run `php --version`\.
+ For pip, run `pip --version`\.
+ For Python, run `python --version`\.
+ For RubyGems, run `gem --version`\.
+ For setuptools, run `easy_install --version`\.

The following build command \(entered through the AWS CodeBuild or AWS CodePipeline console as part of a build project's settings\) returns the versions of the AWS CLI, Git, pip, and Python on a Docker image that has these components installed: `aws --version && git --version && pip --version && python --version`\.