# Build specification reference for CodeBuild<a name="build-spec-ref"></a>

This topic provides important reference information about build specification \(buildspec\) files\. A *buildspec* is a collection of build commands and related settings, in YAML format, that CodeBuild uses to run a build\. You can include a buildspec as part of the source code or you can define a buildspec when you create a build project\. For information about how a build spec works, see [How CodeBuild works](concepts.md#concepts-how-it-works)\.

**Topics**
+ [Buildspec file name and storage location](#build-spec-ref-name-storage)
+ [Buildspec syntax](#build-spec-ref-syntax)
+ [Buildspec example](#build-spec-ref-example)
+ [Buildspec versions](#build-spec-ref-versions)
+ [Batch build buildspec reference](batch-build-buildspec.md)

## Buildspec file name and storage location<a name="build-spec-ref-name-storage"></a>

If you include a buildspec as part of the source code, by default, the buildspec file must be named `buildspec.yml` and placed in the root of your source directory\.

You can override the default buildspec file name and location\. For example, you can:
+ Use a different buildspec file for different builds in the same repository, such as `buildspec_debug.yml` and `buildspec_release.yml`\.
+ Store a buildspec file somewhere other than the root of your source directory, such as `config/buildspec.yml` or in an S3 bucket\. The S3 bucket must be in the same AWS Region as your build project\. Specify the buildspec file using its ARN \(for example, `arn:aws:s3:::my-codebuild-sample2/buildspec.yml`\)\.

You can specify only one buildspec for a build project, regardless of the buildspec file's name\.

To override the default buildspec file name, location, or both, do one of the following:
+ Run the AWS CLI `create-project` or `update-project` command, setting the `buildspec` value to the path to the alternate buildspec file relative to the value of the built\-in environment variable `CODEBUILD_SRC_DIR`\. You can also do the equivalent with the `create project` operation in the AWS SDKs\. For more information, see [Create a build project](create-project.md) or [Change a build project's settings](change-project.md)\.
+ Run the AWS CLI `start-build` command, setting the `buildspecOverride` value to the path to the alternate buildspec file relative to the value of the built\-in environment variable `CODEBUILD_SRC_DIR`\. You can also do the equivalent with the `start build` operation in the AWS SDKs\. For more information, see [Run a build](run-build.md)\.
+ In an AWS CloudFormation template, set the `BuildSpec` property of `Source` in a resource of type `AWS::CodeBuild::Project` to the path to the alternate buildspec file relative to the value of the built\-in environment variable `CODEBUILD_SRC_DIR`\. For more information, see the BuildSpec property in [AWS CodeBuild project source](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-codebuild-project-source.html) in the *AWS CloudFormation User Guide*\.

## Buildspec syntax<a name="build-spec-ref-syntax"></a>

Buildspec files must be expressed in [YAML](http://yaml.org/) format\. 

If a command contains a character, or a string of characters, that is not supported by YAML, you must enclose the command in quotation marks \(""\)\. The following command is enclosed in quotation marks because a colon \(:\) followed by a space is not allowed in YAML\. The quotation mark in the command is escaped \(\\"\)\.

```
"export PACKAGE_NAME=$(cat package.json | grep name | head -1 | awk -F: '{ print $2 }' | sed 's/[\",]//g')"
```

The buildspec has the following syntax:

```
version: 0.2

run-as: Linux-user-name

env:
  shell: shell-tag
  variables:
    key: "value"
    key: "value"
  parameter-store:
    key: "value"
    key: "value"
  exported-variables:
    - variable
    - variable
  secrets-manager:
    key: secret-id:json-key:version-stage:version-id
  git-credential-helper: no | yes

proxy:
  upload-artifacts: no | yes
  logs: no | yes

batch:
  fast-fail: false | true
  # build-list:
  # build-matrix:
  # build-graph:
        
phases:
  install:
    run-as: Linux-user-name
    runtime-versions:
      runtime: version
      runtime: version
    commands:
      - command
      - command
    finally:
      - command
      - command
  pre_build:
    run-as: Linux-user-name
    commands:
      - command
      - command
    finally:
      - command
      - command
  build:
    run-as: Linux-user-name
    commands:
      - command
      - command
    finally:
      - command
      - command
  post_build:
    run-as: Linux-user-name
    commands:
      - command
      - command
    finally:
      - command
      - command
reports:
  report-group-name-or-arn:
    files:
      - location
      - location
    base-directory: location
    discard-paths: no | yes
    file-format: report-format
artifacts:
  files:
    - location
    - location
  name: artifact-name
  discard-paths: no | yes
  base-directory: location
  secondary-artifacts:
    artifactIdentifier:
      files:
        - location
        - location
      name: secondary-artifact-name
      discard-paths: no | yes
      base-directory: location
    artifactIdentifier:
      files:
        - location
        - location
      discard-paths: no | yes
      base-directory: location
cache:
  paths:
    - path
    - path
```

The buildspec contains the following:

### version<a name="build-spec.version"></a>

Required mapping\. Represents the buildspec version\. We recommend that you use `0.2`\.

**Note**  
Although version 0\.1 is still supported, we recommend that you use version 0\.2 whenever possible\. For more information, see [Buildspec versions](#build-spec-ref-versions)\.

### run\-as<a name="build-spec.run-as"></a>

Optional sequence\. Available to Linux users only\. Specifies a Linux user that runs commands in this buildspec file\. `run-as` grants the specified user read and execute permissions\. When you specify `run-as` at the top of the buildspec file, it applies globally to all commands\. If you don't want to specify a user for all buildspec file commands, you can specify one for commands in a phase by using `run-as` in one of the `phases` blocks\. If `run-as` is not specified, then all commands run as the root user\.

### env<a name="build-spec.env"></a>

Optional sequence\. Represents information for one or more custom environment variables\.

**Note**  
 To protect sensitive information, the following are hidden in CodeBuild logs:   
 AWS access key IDs\. For more information, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *AWS Identity and Access Management User Guide*\. 
 Strings specified using the Parameter Store\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\. 
 Strings specified using AWS Secrets Manager\. For more information, see [Key management](security-key-management.md)\. 

env/**shell**  <a name="build-spec.shell"></a>
Optional sequence\. Specifies the supported shell for Linux or Windows operating systems\.   
For Linux operating systems, supported shell tags are:  
+ `bash`
+ `/bin/sh`
For Windows operating systems, supported shell tags are:  
+ `powershell.exe`
+ `cmd.exe`

env/**variables**  <a name="build-spec.env.variables"></a>
Required if `env` is specified, and you want to define custom environment variables in plain text\. Contains a mapping of *key*/*value* scalars, where each mapping represents a single custom environment variable in plain text\. *key* is the name of the custom environment variable, and *value* is that variable's value\.  
We strongly discourage the storing of sensitive values, especially AWS access key IDs and secret access keys, in environment variables\. Environment variables can be displayed in plain text using tools such as the CodeBuild console and the AWS CLI\. For sensitive values, we recommend that you use `parameter-store` or `secrets-manager` mapping instead, as described later in this section\.  
Any environment variables you set replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you set an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you set an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\.  
Do not set any environment variable with a name that starts with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, the value is determined as follows:  
+ The value in the start build operation call takes highest precedence\. You can add or override environment variables when you create a build\. For more information, see [Run a build in AWS CodeBuild](run-build.md)\. 
+ The value in the build project definition takes next precedence\. You can add environment variables at the project level when you create or edit a project\. For more information, see [Create a build project in AWS CodeBuild](create-project.md) and [Change a build project's settings in AWS CodeBuild ](change-project.md)\.
+ The value in the buildspec declaration takes lowest precedence\.

env/**parameter\-store**  <a name="build-spec.env.parameter-store"></a>
Required if `env` is specified, and you want to retrieve custom environment variables stored in Amazon EC2 Systems Manager Parameter Store\. Contains a mapping of *key*/*value* scalars, where each mapping represents a single custom environment variable stored in Amazon EC2 Systems Manager Parameter Store\. *key* is the name you use later in your build commands to refer to this custom environment variable, and *value* is the name of the custom environment variable stored in Amazon EC2 Systems Manager Parameter Store\. To store sensitive values, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Walkthrough: Create and test a String parameter \(console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-console.html) in the *Amazon EC2 Systems Manager User Guide*\.   
To allow CodeBuild to retrieve custom environment variables stored in Amazon EC2 Systems Manager Parameter Store, you must add the `ssm:GetParameters` action to your CodeBuild service role\. For more information, see [Create a CodeBuild service role](setting-up.md#setting-up-service-role)\.  
Any environment variables you retrieve from Amazon EC2 Systems Manager Parameter Store replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you retrieve an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you retrieve an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\.  
Do not store any environment variable with a name that starts with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, the value is determined as follows:  
+ The value in the start build operation call takes highest precedence\. You can add or override environment variables when you create a build\. For more information, see [Run a build in AWS CodeBuild](run-build.md)\. 
+ The value in the build project definition takes next precedence\. You can add environment variables at the project level when you create or edit a project\. For more information, see [Create a build project in AWS CodeBuild](create-project.md) and [Change a build project's settings in AWS CodeBuild ](change-project.md)\.
+ The value in the buildspec declaration takes lowest precedence\.

env/**secrets\-manager**  <a name="build-spec.env.secrets-manager"></a>
Required if `env` specified, and you want to retrieve custom environment variables stored in AWS Secrets Manager\. Specify a Secrets Manager `reference-key` using the following pattern:  
 `secret-id:json-key:version-stage:version-id`   
+  `secret-id`: The name or Amazon Resource Name \(ARN\) that serves as a unique identifier for the secret\. To access a secret in your AWS account, simply specify the secret name\. To access a secret in a different AWS account, specify the secret ARN\. 
+  `json-key`: Specifies the key name of the key\-value pair whose value you want to retrieve\. If you do not specify a `json-key`, CodeBuild retrieves the entire secret text\. 
+  `version-stage`: Specifies the secret version that you want to retrieve by the staging label attached to the version\. Staging labels are used to keep track of different versions during the rotation process\. If you use `version-stage`, don't specify `version-id`\. If you don't specify a version stage or version ID, the default is to retrieve the version with the version stage value of `AWSCURRENT`\. 
+  `version-id`: Specifies the unique identifier of the version of the secret that you want to use\. If you specify `version-id`, don't specify `version-stage`\. If you don't specify a version stage or version ID, the default is to retrieve the version with the version stage value of AWSCURRENT\. 
 For more information, see [What is AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *AWS Secrets Manager User Guide*\. 

env/**exported\-variables**  <a name="build-spec.env.exported-variables"></a>
Optional mapping\. Used to list environment variables you want to export\. Specify the name of each variable you want to export on a separate line under `exported-variables`\. The variable you want to export must be available in your container during the build\. The variable you export can be an environment variable\.  
 During a build, the value of a variable is available starting with the `install` phase\. It can be updated between the start of the `install` phase and the end of the `post_build` phase\. After the `post_build` phase ends, the value of exported variables cannot change\.  
 The following cannot be exported:   
+  Amazon EC2 Systems Manager Parameter Store secrets specified in the build project\. 
+  Secrets Manager secrets specified in the build project 
+  Environment variables that start with `AWS_`\. 

env/**git\-credential\-helper**  <a name="build-spec.env.git-credential-helper"></a>
Optional mapping\. Used to indicate if CodeBuild uses its Git credential helper to provide Git credentials\. `yes` if it is used\. Otherwise, `no` or not specified\. For more information, see [gitcredentials](https://git-scm.com/docs/gitcredentials) on the Git website\.   
 `git-credential-helper` is not supported for builds that are triggered by a webhook for a public Git repository\.

### proxy<a name="build-spec.proxy"></a>

Optional sequence\. Used to represent settings if you run your build in an explicit proxy server\. For more information, see [ Run CodeBuild in an explicit proxy server](use-proxy-server.md#run-codebuild-in-explicit-proxy-server)\. 

proxy/**upload\-artifacts**  <a name="build-spec.proxy.upload-artifacts"></a>
Optional mapping\. Set to `yes` if you want your build in an explicit proxy server to upload artifacts\. The default is `no`\. 

proxy/**logs**  <a name="build-spec.proxy.logs"></a>
Optional mapping\. Set to `yes` for your build in a explicit proxy server to create CloudWatch logs\. The default is `no`\. 

### phases<a name="build-spec.phases"></a>

Required sequence\. Represents the commands CodeBuild runs during each phase of the build\. 

**Note**  
In buildspec version 0\.1, CodeBuild runs each command in a separate instance of the default shell in the build environment\. This means that each command runs in isolation from all other commands\. Therefore, by default, you cannot run a single command that relies on the state of any previous commands \(for example, changing directories or setting environment variables\)\. To get around this limitation, we recommend that you use version 0\.2, which solves this issue\. If you must use buildspec version 0\.1, we recommend the approaches in [Shells and commands in build environments](build-env-ref-cmd.md)\.

phases/\*/**run\-as**  <a name="build-spec.phases.run-as"></a>
Optional sequence\. Use in a build phase to specify a Linux user that runs its commands\. If `run-as` is also specified globally for all commands at the top of the buildspec file, then the phase\-level user takes precedence\. For example, if globally `run-as` specifies User\-1, and for the `install` phase only a `run-as` statement specifies User\-2, then all commands in then buildspec file are run as User\-1 *except* commands in the `install` phase, which are run as User\-2\.

The allowed build phase names are:

phases/**install**  <a name="build-spec.phases.install"></a>
Optional sequence\. Represents the commands, if any, that CodeBuild runs during installation\. We recommend that you use the `install` phase only for installing packages in the build environment\. For example, you might use this phase to install a code testing framework such as Mocha or RSpec\.    
phases/install/**runtime\-versions**  
<a name="runtime-versions-in-build-spec"></a>Optional sequence\. A runtime version is supported with the Ubuntu standard image 2\.0 or later and the Amazon Linux 2 standard image 1\.0 or later\. If specified, at least one runtime must be included in this section\. Specify a runtime using a specific version, a major version followed by `.x` to specify that CodeBuild uses that major version with its latest minor version, or `latest` to use the most recent major and minor version \(for example, `java: openjdk11`, `ruby: 2.6`, `nodejs: 12.x`, or `java: latest`\)\. You can specify the runtime using a number or an environment variable\. For example, if you use the Amazon Linux 2 standard image 2\.0, then the following specifies that version 8 of Java, the latest minor version of python version 3, and a version contained in an environment variable of Ruby is installed\. For more information, see [Docker images provided by CodeBuild](build-env-ref-available.md)\.   

```
phases:
  install:
    runtime-versions:
      java: corretto8
      python: 3.x
      ruby: "$MY_RUBY_VAR"
```
 You can specify one or more runtimes in the `runtime-versions` section of your buildspec file\. If your runtime is dependent upon another runtime, you can also specify its dependent runtime in the buildspec file\. If you do not specify any runtimes in the buildspec file, CodeBuild chooses the default runtimes that are available in the image you use\. If you specify one or more runtimes, CodeBuild uses only those runtimes\. If a dependent runtime is not specified, CodeBuild attempts to choose the dependent runtime for you\.   
If two specified runtimes conflict, the build fails\. For example, `android: 29` and `java: openjdk11` conflict, so if both are specified, the build fails\.  
 The following supported runtimes can be specified\.     
**Ubuntu 18\.04 and Amazon Linux 2 platforms runtimes**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)
 If you specify a `runtime-versions` section and use an image other than Ubuntu Standard Image 2\.0 or later, or the Amazon Linux 2 \(AL2\) standard image 1\.0 or later, the build issues the warning, "`Skipping install of runtimes. Runtime version selection is not supported by this build image`\."   
phases/install/**commands**  
`commands`: Optional sequence\. Contains a sequence of scalars, where each scalar represents a single command that CodeBuild runs during installation\. CodeBuild runs each command, one at a time, in the order listed, from beginning to end\.  
phases/install/**finally**  
Optional block\. Commands specified in a `finally` block are executed after commands in the `commands` block\. The commands in a `finally` block are executed even if a command in the `commands` block fails\. For example, if the `commands` block contains three commands and the first fails, CodeBuild skips the remaining two commands and runs any commands in the `finally` block\. The phase is successful when all commands in the `commands` and the `finally` blocks run successfully\. If any command in a phase fails, the phase fails\.

phases/**pre\_build**  <a name="build-spec.phases.pre_build"></a>
Optional sequence\. Represents the commands, if any, that CodeBuild runs before the build\. For example, you might use this phase to sign in to Amazon ECR, or you might install npm dependencies\.     
phases/pre\_build/**commands**  
Required sequence if `pre_build` is specified\. Contains a sequence of scalars, where each scalar represents a single command that CodeBuild runs before the build\. CodeBuild runs each command, one at a time, in the order listed, from beginning to end\.  
phases/pre\_build/**finally**  
Optional block\. Commands specified in a `finally` block are executed after commands in the `commands` block\. The commands in a `finally` block are executed even if a command in the `commands` block fails\. For example, if the `commands` block contains three commands and the first fails, CodeBuild skips the remaining two commands and runs any commands in the `finally` block\. The phase is successful when all commands in the `commands` and the `finally` blocks run successfully\. If any command in a phase fails, the phase fails\.

phases/**build**  <a name="build-spec.phases.build"></a>
Optional sequence\. Represents the commands, if any, that CodeBuild runs during the build\. For example, you might use this phase to run Mocha, RSpec, or sbt\.    
phases/build/**commands**  
`commands`: Required if `build` is specified\. Contains a sequence of scalars, where each scalar represents a single command that CodeBuild runs during the build\. CodeBuild runs each command, one at a time, in the order listed, from beginning to end\.  
phases/build/**finally**  
Optional block\. Commands specified in a `finally` block are executed after commands in the `commands` block\. The commands in a `finally` block are executed even if a command in the `commands` block fails\. For example, if the `commands` block contains three commands and the first fails, CodeBuild skips the remaining two commands and runs any commands in the `finally` block\. The phase is successful when all commands in the `commands` and the `finally` blocks run successfully\. If any command in a phase fails, the phase fails\.

phases/**post\_build**  <a name="build-spec.phases.post_build"></a>
Optional sequence\. Represents the commands, if any, that CodeBuild runs after the build\. For example, you might use Maven to package the build artifacts into a JAR or WAR file, or you might push a Docker image into Amazon ECR\. Then you might send a build notification through Amazon SNS\.    
phases/post\_build/**commands**  
`commands`: Required if `post_build` is specified\. Contains a sequence of scalars, where each scalar represents a single command that CodeBuild runs after the build\. CodeBuild runs each command, one at a time, in the order listed, from beginning to end\.  
phases/post\_build/**finally**  
Optional block\. Commands specified in a `finally` block are executed after commands in the `commands` block\. The commands in a `finally` block are executed even if a command in the `commands` block fails\. For example, if the `commands` block contains three commands and the first fails, CodeBuild skips the remaining two commands and runs any commands in the `finally` block\. The phase is successful when all commands in the `commands` and the `finally` blocks run successfully\. If any command in a phase fails, the phase fails\.

**Important**  
Commands in some build phases might not be run if commands in earlier build phases fail\. For example, if a command fails during the `install` phase, none of the commands in the `pre_build`, `build`, and `post_build` phases are run for that build's lifecycle\. For more information, see [Build phase transitions](view-build-details.md#view-build-details-phases)\.<a name="reports-buildspec-file"></a>

### reports<a name="build-spec.reports"></a>

**report\-group\-name\-or\-arn**  <a name="build-spec.reports.report-name-or-arn"></a>
Optional sequence\. Specifies the report group that the reports are sent to\. A project can have a maximum of five report groups\. Specify the ARN of an existing report group, or the name of a new report group\. If you specify a name, CodeBuild creates a report group using your project name and the name you specify in the format `<project-name>-<report-group-name>`\. For more information, see [Report group naming](test-report-group-naming.md)\.

reports/<report\-group>/**files**  <a name="build-spec.reports.files"></a>
Required sequence\. Represents the locations that contain the raw data of test results generated by the report\. Contains a sequence of scalars, with each scalar representing a separate location where CodeBuild can find test files, relative to the original build location or, if set, the `base-directory`\. Locations can include the following:  
+ A single file \(for example, `my-test-report-file.json`\)\.
+ A single file in a subdirectory \(for example, `my-subdirectory/my-test-report-file.json` or `my-parent-subdirectory/my-subdirectory/my-test-report-file.json`\)\.
+ `'**/*'` represents all files recursively\.
+ `my-subdirectory/*` represents all files in a subdirectory named *my\-subdirectory*\.
+ `my-subdirectory/**/*` represents all files recursively starting from a subdirectory named *my\-subdirectory*\.

reports/<report\-group>/**file\-format**  <a name="build-spec.reports.file-format"></a>
Optional mapping\. Represents the report file format\. If not specified, `JUNITXML` is used\. This value is not case sensitive\. Possible values are:  
**Test reports**    
 `CUCUMBERJSON`   
Cucumber JSON  
 `JUNITXML`   
JUnit XML  
 `NUNITXML`   
NUnit XML  
 `NUNIT3XML`   
NUnit 3 XML  
 `TESTNGXML`   
TestNG XML  
 `VISUALSTUDIOTRX`   
Visual Studio TRX
**Code coverage reports**    
 `CLOVERXML`   
Clover XML  
 `COBERTURAXML`   
Cobertura XML  
 `JACOCOXML`   
JaCoCo XML  
 `SIMPLECOV`   
SimpleCov JSON  
CodeBuild accepts JSON code coverage reports generated by [simplecov](https://github.com/simplecov-ruby/simplecov), not [simplecov\-json](https://github.com/vicentllongo/simplecov-json)\.

reports/<report\-group>/**base\-directory**  <a name="build-spec.reports.base-directory"></a>
Optional mapping\. Represents one or more top\-level directories, relative to the original build location, that CodeBuild uses to determine where to find the raw test files\.

reports/<report\-group>/**discard\-paths**  <a name="build-spec.reports.discard-paths"></a>
Optional\. Specifies if the report file directories are flattened in the output\. If this is not specified, or contains `no`, report files are output with their directory structure intact\. If this contains `yes`, all of the test files are placed in the same output directory\. For example, if a path to a test result is `com/myapp/mytests/TestResult.xml`, specifying `yes` will place this file in `/TestResult.xml`\. <a name="artifacts-build-spec"></a>

### artifacts<a name="build-spec.artifacts"></a>

Optional sequence\. Represents information about where CodeBuild can find the build output and how CodeBuild prepares it for uploading to the S3 output bucket\. This sequence is not required if, for example, you are building and pushing a Docker image to Amazon ECR, or you are running unit tests on your source code, but not building it\.

artifacts/**files**  <a name="build-spec.artifacts.files"></a>
Required sequence\. Represents the locations that contain the build output artifacts in the build environment\. Contains a sequence of scalars, with each scalar representing a separate location where CodeBuild can find build output artifacts, relative to the original build location or, if set, the base directory\. Locations can include the following:  
+ A single file \(for example, `my-file.jar`\)\.
+ A single file in a subdirectory \(for example, `my-subdirectory/my-file.jar` or `my-parent-subdirectory/my-subdirectory/my-file.jar`\)\.
+ `'**/*'` represents all files recursively\.
+ `my-subdirectory/*` represents all files in a subdirectory named *my\-subdirectory*\.
+ `my-subdirectory/**/*` represents all files recursively starting from a subdirectory named *my\-subdirectory*\.
When you specify build output artifact locations, CodeBuild can locate the original build location in the build environment\. You do not have to prepend your build artifact output locations with the path to the original build location or specify `./` or similar\. If you want to know the path to this location, you can run a command such as `echo $CODEBUILD_SRC_DIR` during a build\. The location for each build environment might be slightly different\. 

artifacts/**name**  <a name="build-spec.artifacts.name"></a>
Optional name\. Specifies a name for your build artifact\. This name is used when one of the following is true\.  
+  You use the CodeBuild API to create your builds and the `overrideArtifactName` flag is set on the `ProjectArtifacts` object when a project is updated, a project is created, or a build is started\. 
+  You use the CodeBuild console to create your builds, a name is specified in the buildspec file, and you select **Enable semantic versioning** when you create or update a project\. For more information, see [Create a build project \(console\)](create-project-console.md)\. 
You can specify a name in the buildspec file that is calculated at build time\. The name specified in a buildspec file uses the Shell command language\. For example, you can append a date and time to your artifact name so that it is always unique\. Unique artifact names prevent artifacts from being overwritten\. For more information, see [Shell command language](http://pubs.opengroup.org/onlinepubs/9699919799/)\.   
This is an example of an artifact name appended with the date the artifact is created\.   

```
version: 0.2
phases:
  build:
    commands:
      - rspec HelloWorld_spec.rb
artifacts:
  files:
    - '**/*'
  name: myname-$(date +%Y-%m-%d)
```
This is an example of an artifact name that uses a CodeBuild environment variable\. For more information, see [Environment variables in build environments](build-env-ref-env-vars.md)\.   

```
version: 0.2
phases:
  build:
    commands:
      - rspec HelloWorld_spec.rb
artifacts:
  files:
    - '**/*'
  name: myname-$AWS_REGION
```
This is an example of an artifact name that uses a CodeBuild environment variable with the artifact's creation date appended to it\.   

```
version: 0.2
phases:
  build:
    commands:
      - rspec HelloWorld_spec.rb
artifacts:
  files:
    - '**/*'
  name: $AWS_REGION-$(date +%Y-%m-%d)
```

artifacts/**discard\-paths**  <a name="build-spec.artifacts.discard-paths"></a>
Optional\. Specifies if the build artifact directories are flattened in the output\. If this is not specified, or contains `no`, build artifacts are output with their directory structure intact\. If this contains `yes`, all of the build artifacts are placed in the same output directory\. For example, if a path to a file in the build output artifact is `com/mycompany/app/HelloWorld.java`, specifying `yes` will place this file in `/HelloWorld.java`\. 

artifacts/**base\-directory**  <a name="build-spec.artifacts.base-directory"></a>
Optional mapping\. Represents one or more top\-level directories, relative to the original build location, that CodeBuild uses to determine which files and subdirectories to include in the build output artifact\. Valid values include:  
+ A single top\-level directory \(for example, `my-directory`\)\.
+ `'my-directory*'` represents all top\-level directories with names starting with `my-directory`\.
Matching top\-level directories are not included in the build output artifact, only their files and subdirectories\.   
You can use `files` and `discard-paths` to further restrict which files and subdirectories are included\. For example, for the following directory structure:   

```
|-- my-build1
|     `-- my-file1.txt
`-- my-build2
      |-- my-file2.txt
      `-- my-subdirectory
            `-- my-file3.txt
```
And for the following `artifacts` sequence:  

```
artifacts:
  files:
    - '*/my-file3.txt'
  base-directory: my-build2
```
The following subdirectory and file would be included in the build output artifact:  

```
my-subdirectory
  `-- my-file3.txt
```
While for the following `artifacts` sequence:  

```
artifacts:
  files:
    - '**/*'
  base-directory: 'my-build*'
  discard-paths: yes
```
The following files would be included in the build output artifact:  

```
|-- my-file1.txt
|-- my-file2.txt
`-- my-file3.txt
```

artifacts/**secondary\-artifacts**  <a name="build-spec.artifacts.secondary-artifacts"></a>
Optional sequence\. Represents one or more artifact definitions as a mapping between an artifact identifier and an artifact definition\. Each artifact identifiers in this block must match an artifact defined in the `secondaryArtifacts` attribute of your project\. Each separate definition has the same syntax as the `artifacts` block above\.   
The [`artifacts/files`](#build-spec.artifacts.files) sequence is always required, even when there are only secondary artifacts defined\.
For example, if your project has the following structure:  

```
{
  "name": "sample-project",
  "secondaryArtifacts": [
    {
      "type": "S3",
      "location": "output-bucket1",
      "artifactIdentifier": "artifact1",
      "name": "secondary-artifact-name-1"
    },
    {
      "type": "S3",
      "location": "output-bucket2",
      "artifactIdentifier": "artifact2",
      "name": "secondary-artifact-name-2"
    }
  ]
}
```
Then your buildspec looks like the following:  

```
version: 0.2

phases:
build:
  commands:
    - echo Building...
artifacts:
  files:
    - '**/*'
  secondary-artifacts:
    artifact1:
      files:
        - directory/file1
      name: secondary-artifact-name-1
    artifact2:
      files:
        - directory/file2
      name: secondary-artifact-name-2
```

### cache<a name="build-spec.cache"></a>

Optional sequence\. Represents information about where CodeBuild can prepare the files for uploading cache to an S3 cache bucket\. This sequence is not required if the cache type of the project is `No Cache`\.

cache/**paths**  <a name="build-spec.cache.paths"></a>
Required sequence\. Represents the locations of the cache\. Contains a sequence of scalars, with each scalar representing a separate location where CodeBuild can find build output artifacts, relative to the original build location or, if set, the base directory\. Locations can include the following:  
+ A single file \(for example, `my-file.jar`\)\.
+ A single file in a subdirectory \(for example, `my-subdirectory/my-file.jar` or `my-parent-subdirectory/my-subdirectory/my-file.jar`\)\.
+ `'**/*'` represents all files recursively\.
+ `my-subdirectory/*` represents all files in a subdirectory named *my\-subdirectory*\.
+ `my-subdirectory/**/*` represents all files recursively starting from a subdirectory named *my\-subdirectory*\.

**Important**  
Because a buildspec declaration must be valid YAML, the spacing in a buildspec declaration is important\. If the number of spaces in your buildspec declaration is invalid, builds might fail immediately\. You can use a YAML validator to test whether your buildspec declarations are valid YAML\.   
If you use the AWS CLI, or the AWS SDKs to declare a buildspec when you create or update a build project, the buildspec must be a single string expressed in YAML format, along with required whitespace and newline escape characters\. There is an example in the next section\.  
If you use the CodeBuild or AWS CodePipeline consoles instead of a buildspec\.yml file, you can insert commands for the `build` phase only\. Instead of using the preceding syntax, you list, in a single line, all of the commands that you want to run during the build phase\. For multiple commands, separate each command by `&&` \(for example, `mvn test && mvn package`\)\.  
You can use the CodeBuild or CodePipeline consoles instead of a buildspec\.yml file to specify the locations of the build output artifacts in the build environment\. Instead of using the preceding syntax, you list, in a single line, all of the locations\. For multiple locations, separate each location with a comma \(for example, `buildspec.yml, target/my-app.jar`\)\. 

## Buildspec example<a name="build-spec-ref-example"></a>

Here is an example of a buildspec\.yml file\.

```
version: 0.2

env:
  variables:
    JAVA_HOME: "/usr/lib/jvm/java-8-openjdk-amd64"
  parameter-store:
    LOGIN_PASSWORD: /CodeBuild/dockerLoginPassword

phases:
  install:
    commands:
      - echo Entered the install phase...
      - apt-get update -y
      - apt-get install -y maven
    finally:
      - echo This always runs even if the update or install command fails 
  pre_build:
    commands:
      - echo Entered the pre_build phase...
      - docker login –u User –p $LOGIN_PASSWORD
    finally:
      - echo This always runs even if the login command fails 
  build:
    commands:
      - echo Entered the build phase...
      - echo Build started on `date`
      - mvn install
    finally:
      - echo This always runs even if the install command fails
  post_build:
    commands:
      - echo Entered the post_build phase...
      - echo Build completed on `date`

reports:
  arn:aws:codebuild:your-region:your-aws-account-id:report-group/report-group-name-1:
    files:
      - "**/*"
    base-directory: 'target/tests/reports'
    discard-paths: no
  reportGroupCucumberJson:
    files:
      - 'cucumber/target/cucumber-tests.xml'
    discard-paths: yes
    file-format: CUCUMBERJSON # default is JUNITXML
artifacts:
  files:
    - target/messageUtil-1.0.jar
  discard-paths: yes
  secondary-artifacts:
    artifact1:
      files:
        - target/artifact-1.0.jar
      discard-paths: yes
    artifact2:
      files:
        - target/artifact-2.0.jar
      discard-paths: yes
cache:
  paths:
    - '/root/.m2/**/*'
```

Here is an example of the preceding buildspec, expressed as a single string, for use with the AWS CLI, or the AWS SDKs\.

```
"version: 0.2\n\nenv:\n  variables:\n    JAVA_HOME: \"/usr/lib/jvm/java-8-openjdk-amd64\\"\n  parameter-store:\n    LOGIN_PASSWORD: /CodeBuild/dockerLoginPassword\n  phases:\n\n  install:\n    commands:\n      - echo Entered the install phase...\n      - apt-get update -y\n      - apt-get install -y maven\n    finally:\n      - echo This always runs even if the update or install command fails \n  pre_build:\n    commands:\n      - echo Entered the pre_build phase...\n      - docker login –u User –p $LOGIN_PASSWORD\n    finally:\n      - echo This always runs even if the login command fails \n  build:\n    commands:\n      - echo Entered the build phase...\n      - echo Build started on `date`\n      - mvn install\n    finally:\n      - echo This always runs even if the install command fails\n  post_build:\n    commands:\n      - echo Entered the post_build phase...\n      - echo Build completed on `date`\n\n reports:\n  reportGroupJunitXml:\n    files:\n      - \"**/*\"\n    base-directory: 'target/tests/reports'\n    discard-paths: false\n  reportGroupCucumberJson:\n    files:\n      - 'cucumber/target/cucumber-tests.xml'\n    file-format: CUCUMBERJSON\n\nartifacts:\n  files:\n    - target/messageUtil-1.0.jar\n  discard-paths: yes\n  secondary-artifacts:\n    artifact1:\n      files:\n       - target/messageUtil-1.0.jar\n      discard-paths: yes\n    artifact2:\n      files:\n       - target/messageUtil-1.0.jar\n      discard-paths: yes\n cache:\n  paths:\n    - '/root/.m2/**/*'"
```

Here is an example of the commands in the `build` phase, for use with the CodeBuild or CodePipeline consoles\.

```
echo Build started on `date` && mvn install
```

In these examples:
+ A custom environment variable, in plain text, with the key of `JAVA_HOME` and the value of `/usr/lib/jvm/java-8-openjdk-amd64`, is set\.
+ A custom environment variable named `dockerLoginPassword` you stored in Amazon EC2 Systems Manager Parameter Store is referenced later in build commands by using the key `LOGIN_PASSWORD`\.
+ You cannot change these build phase names\. The commands that are run in this example are `apt-get update -y` and `apt-get install -y maven` \(to install Apache Maven\), `mvn install` \(to compile, test, and package the source code into a build output artifact and to install the build output artifact in its internal repository\), `docker login` \(to sign in to Docker with the password that corresponds to the value of the custom environment variable `dockerLoginPassword` you set in Amazon EC2 Systems Manager Parameter Store\), and several `echo` commands\. The `echo` commands are included here to show how CodeBuild runs commands and the order in which it runs them\. 
+ `files` represents the files to upload to the build output location\. In this example, CodeBuild uploads the single file `messageUtil-1.0.jar`\. The `messageUtil-1.0.jar` file can be found in the relative directory named `target` in the build environment\. Because `discard-paths: yes` is specified, `messageUtil-1.0.jar` is uploaded directly \(and not to an intermediate `target` directory\)\. The file name `messageUtil-1.0.jar` and the relative directory name of `target` is based on the way Apache Maven creates and stores build output artifacts for this example only\. In your own scenarios, these file names and directories will be different\. 
+ `reports` represents two report groups that generate reports during the build:
  + `arn:aws:codebuild:your-region:your-aws-account-id:report-group/report-group-name-1` specifies the ARN of a report group\. Test results generated by the test framework are in the `target/tests/reports` directory\. The file format is `JunitXml` and the path is not removed from the files that contain test results\.
  + `reportGroupCucumberJson` specifies a new report group\. If the name of the project is `my-project`, a report group with the name `my-project-reportGroupCucumberJson` is created when a build is run\. Test results generated by the test framework are in `cucumber/target/cucumber-tests.xml`\. The test file format is `CucumberJson` and the path is removed from the files that contain test results\.

## Buildspec versions<a name="build-spec-ref-versions"></a>

The following table lists the buildspec versions and the changes between versions\.


| Version | Changes | 
| --- | --- | 
| 0\.2 |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)  | 
| 0\.1 | This is the initial definition of the build specification format\. | 