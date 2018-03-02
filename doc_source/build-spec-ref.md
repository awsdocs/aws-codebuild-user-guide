# Build Specification Reference for AWS CodeBuild<a name="build-spec-ref"></a>

This topic provides important reference information about build specifications \(build specs\)\. A *build spec* is a collection of build commands and related settings, in YAML format, that AWS CodeBuild uses to run a build\. You can include a build spec as part of the source code or you can define a build spec when you create a build project\. For information about how a build spec works, see [How AWS CodeBuild Works](concepts.md#concepts-how-it-works)\.


+ [Build Spec File Name and Storage Location](#build-spec-ref-name-storage)
+ [Build Spec Syntax](#build-spec-ref-syntax)
+ [Build Spec Example](#build-spec-ref-example)
+ [Build Spec Versions](#build-spec-ref-versions)

## Build Spec File Name and Storage Location<a name="build-spec-ref-name-storage"></a>

If you include a build spec as part of the source code, by default, the build spec file must be named `buildspec.yml` and be placed in the root of your source directory\.

You can also override the default build spec file name and location\. For example, you can:

+ Use a different build spec file for different builds in the same repository, such as `buildspec_debug.yml` and `buildspec_release.yml`\.

+ Store a build spec file somewhere other than the root of your source directory, such as `config/buildspec.yml`\.

You can specify only one build spec for a build project, regardless of the build spec file's name\.

To override the default build spec file name, location, or both, do one of the following:

+ Run the AWS CLI `create-project` or `update-project` command, setting the `buildspec` value to the path to the alternate build spec file relative to the value of the built\-in environment variable `CODEBUILD_SRC_DIR`\. You can also do the equivalent with the create project operation in the AWS SDKs\. For more information, see [Create a Build Project](create-project.md) or [Change a Build Project's Settings](change-project.md)\.

+ Run the AWS CLI `start-build` command, setting the `buildspecOverride` value to the path to the alternate build spec file relative to the value of the built\-in environment variable `CODEBUILD_SRC_DIR`\. You can also do the equivalent with the start build operation in the AWS SDKs\. For more information, see [Run a Build](run-build.md)\.

+ In an AWS CloudFormation template, set the `BuildSpec` property of `Source` in a resource of type `AWS::CodeBuild::Project` to the path to the alternate build spec file relative to the value of the built\-in environment variable `CODEBUILD_SRC_DIR`\. For more information, see the "BuildSpec" property in [AWS CodeBuild Project Source](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-codebuild-project-source.html) in the *AWS CloudFormation User Guide*\.

## Build Spec Syntax<a name="build-spec-ref-syntax"></a>

Build specs must be expressed in [YAML](http://yaml.org/) format\.

The build spec has the following syntax:

```
version: 0.2

env:
  variables:
    key: "value"
    key: "value"
  parameter-store:
    key: "value"
    key: "value"
            
phases:
  install:
    commands:
      - command
      - command
  pre_build:
    commands:
      - command
      - command
  build:
    commands:
      - command
      - command
  post_build:
    commands:
      - command
      - command
artifacts:
  files:
    - location
    - location
  discard-paths: yes
  base-directory: location
```

The build spec contains the following:

+ `version`: Required mapping\. Represents the build spec version\. We recommend you use `0.2`\.
**Note**  
Version 0\.1 is still supported\. However, we recommend that you use version 0\.2 whenever possible\. For more information, see [Build Spec Versions](#build-spec-ref-versions)\.

+ `env`: Optional sequence\. Represents information for one or more custom environment variables\.

  + `variables`: Required if `env` is specified, and you want to define custom environment variables in plain text\. Contains a mapping of *key*/*value* scalars, where each mapping represents a single custom environment variable in plain text\. *key* is the name of the custom environment variable, and *value* is that variable's value\.
**Important**  
We strongly discourage using environment variables to store sensitive values, especially AWS access key IDs and secret access keys\. Environment variables can be displayed in plain text using tools such as the AWS CodeBuild console and the AWS CLI\. For sensitive values, we recommend you use the `parameter-store` mapping instead, as described later in this section\.  
Any environment variables you set will replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you set an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` will be replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you set an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` will be replaced by the literal value `$PATH:/usr/share/ant/bin`\.  
Do not set any environment variable with a name that begins with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, the value is determined as follows:  
The value in the start build operation call takes highest precedence\.
The value in the build project definition takes next precedence\.
The value in the build spec declaration takes lowest precedence\.

  + `parameter-store`: Required if `env` is specified, and you want to retrieve custom environment variables stored in Amazon EC2 Systems Manager Parameter Store\. Contains a mapping of *key*/*value* scalars, where each mapping represents a single custom environment variable stored in Amazon EC2 Systems Manager Parameter Store\. *key* is the name you will use later in your build commands to refer to this custom environment variable, and *value* is the name of the custom environment variable stored in Amazon EC2 Systems Manager Parameter Store\. To store sensitive values, see [Systems Manager Parameter Store](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](http://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\. 
**Important**  
To allow AWS CodeBuild to retrieve custom environment variables stored in Amazon EC2 Systems Manager Parameter Store, you must add the `ssm:GetParameters` action to your AWS CodeBuild service role\. For more information, see [Create an AWS CodeBuild Service Role](setting-up.md#setting-up-service-role)\.  
Any environment variables you retrieve from Amazon EC2 Systems Manager Parameter Store will replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you retrieve an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` will be replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you retrieve an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` will be replaced by the literal value `$PATH:/usr/share/ant/bin`\.  
Do not store any environment variable with a name that begins with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, the value is determined as follows:  
The value in the start build operation call takes highest precedence\.
The value in the build project definition takes next precedence\.
The value in the build spec declaration takes lowest precedence\.

+ `phases`: Required sequence\. Represents the commands AWS CodeBuild will run during each phase of the build\. 
**Note**  
In build spec version 0\.1, AWS CodeBuild runs each command in a separate instance of the default shell in the build environment\. This means that each command runs in isolation from all other commands\. Therefore, by default, you cannot run a single command that relies on the state of any previous commands \(for example, changing directories or setting environment variables\)\. To get around this limitation, we recommend you use version 0\.2, which solves this issue\. If you must use build spec version 0\.1 for some reason, we recommend the approaches in [Shells and Commands in Build Environments](build-env-ref-cmd.md)\.

  The allowed build phase names are:

  + `install`: Optional sequence\. Represents the commands, if any, that AWS CodeBuild will run during installation\. We recommend you use the `install` phase only for installing packages in the build environment\. For example, you might use this phase to install a code testing framework such as Mocha or RSpec\.

    + `commands`: Required sequence if `install` is specified\. Contains a sequence of scalars, where each scalar represents a single command that AWS CodeBuild will run during installation\. AWS CodeBuild runs each command, one at a time, in the order listed, from beginning to end\.

  + `pre_build`: Optional sequence\. Represents the commands, if any, that AWS CodeBuild will run before the build\. For example, you might use this phase to log in to Amazon ECR, or you might install npm dependencies\. 

    + `commands`: Required sequence if `pre_build` is specified\. Contains a sequence of scalars, where each scalar represents a single command that AWS CodeBuild will run before the build\. AWS CodeBuild runs each command, one at a time, in the order listed, from beginning to end\.

  + `build`: Optional sequence\. Represents the commands, if any, that AWS CodeBuild will run during the build\. For example, you might use this phase to run Mocha, RSpec, or sbt\.

    + `commands`: Required if `build` is specified\. Contains a sequence of scalars, where each scalar represents a single command that AWS CodeBuild will run during the build\. AWS CodeBuild runs each command, one at a time, in the order listed, from beginning to end\.

  + `post_build`: Optional sequence\. Represents the commands, if any, that AWS CodeBuild will run after the build\. For example, you might use Maven to package the build artifacts into a JAR or WAR file, or you might push a Docker image into Amazon ECR\. Then you might send a build notification through Amazon SNS\.

    + `commands`: Required if `post_build` is specified\. Contains a sequence of scalars, where each scalar represents a single command that AWS CodeBuild will run after the build\. AWS CodeBuild runs each command, one at a time, in the order listed, from beginning to end\.
**Important**  
Commands in some build phases might not be run if commands in earlier build phases fail\. For example, if a command fails during the `install` phase, none of the commands in the `pre_build`, `build`, and `post_build` phases will be run for that build's lifecycle\. For more information, see [Build Phase Transitions](view-build-details.md#view-build-details-phases)\.

+ `artifacts` Optional sequence\. Represents information about where AWS CodeBuild can find the build output and how AWS CodeBuild will prepare it for uploading to the Amazon S3 output bucket\. This sequence is not required if, for example, you are building and pushing a Docker image to Amazon ECR, or you are running unit tests on your source code but not building it\.

  + `files`: Required sequence\. Represents the locations containing the build output artifacts in the build environment\. Contains a sequence of scalars, with each scalar representing a separate location where AWS CodeBuild can find build output artifacts, relative to the original build location or, if set, the base directory\. Locations can include the following:

    + A single file \(for example, `my-file.jar`\)\.

    + A single file in a subdirectory \(for example, `my-subdirectory/my-file.jar` or `my-parent-subdirectory/my-subdirectory/my-file.jar`\)\.

    + `'**/*'` represents all files recursively\.

    + `my-subdirectory/*` represents all files in a subdirectory named *my\-subdirectory*\.

    + `my-subdirectory/**/*` represents all files recursively starting from a subdirectory named *my\-subdirectory*\.

    When you specify build output artifact locations, AWS CodeBuild can locate the original build location in the build environment\. You do not have to prepend your build artifact output locations with the path to the original build location or specify `./` or similar\. If you want to know the path to this location, you can run a command such as `echo $CODEBUILD_SRC_DIR` during a build\. The location for each build environment might be slightly different\. 

  + `discard-paths`: Optional mapping\. Represents whether paths to files in the build output artifact are discarded\. `yes` if paths are discarded; otherwise, `no` or not specified \(the default\)\. For example, if a path to a file in the build output artifact would be `com/mycompany/app/HelloWorld.java`, then specifying `yes` would shorten this path to simply `HelloWorld.java`\. 

  + `base-directory`: Optional mapping\. Represents one or more top\-level directories, relative to the original build location, that AWS CodeBuild uses to determine which files and subdirectories to include in the build output artifact\. Valid values include:

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

+ `cache` Optional sequence\. Represents information about where AWS CodeBuild can prepare the files for uploading cache to an Amazon S3 cache bucket\. This sequence is not required if the cache type of the project is `No Cache`\. 

  + `paths`: Required sequence\. Represents the locations of the cache\. Contains a sequence of scalars, with each scalar representing a separate location where AWS CodeBuild can find build output artifacts, relative to the original build location or, if set, the base directory\. Locations can include the following:

    + A single file \(for example, `my-file.jar`\)\.

    + A single file in a subdirectory \(for example, `my-subdirectory/my-file.jar` or `my-parent-subdirectory/my-subdirectory/my-file.jar`\)\.

    + `'**/*'` represents all files recursively\.

    + `my-subdirectory/*` represents all files in a subdirectory named *my\-subdirectory*\.

    + `my-subdirectory/**/*` represents all files recursively starting from a subdirectory named *my\-subdirectory*\.

**Important**  
Because a build spec declaration must be valid YAML, the spacing in a build spec declaration is important\. If the number of spaces in your build spec declaration is invalid, builds might fail immediately\. You can use a YAML validator to test whether your build spec declarations are valid YAML\.   
If you use the AWS CLI, or the AWS SDKs to declare a build spec when you create or update a build project, the build spec must be a single string expressed in YAML format, along with required whitespace and newline escape characters\. There is an example in the next section\.  
If you use the AWS CodeBuild or AWS CodePipeline consoles instead of a buildspec\.yml file, you can insert commands for the `build` phase only\. Instead of using the preceding syntax, you list, in a single line, all of the commands you want to run during the build phase\. For multiple commands, separate each command by `&&` \(for example, `mvn test && mvn package`\)\.  
You can use the AWS CodeBuild or AWS CodePipeline consoles instead of a buildspec\.yml file to specify the locations of the build output artifacts in the build environment\. Instead of using the preceding syntax, you list, in a single line, all of the locations\. For multiple locations, separate each location with a comma \(for example, `appspec.yml, target/my-app.jar`\)\. 

## Build Spec Example<a name="build-spec-ref-example"></a>

Here is an example of a buildspec\.yml file\.

```
version: 0.2

env:
  variables:
    JAVA_HOME: "/usr/lib/jvm/java-8-openjdk-amd64"
  parameter-store:
    LOGIN_PASSWORD: "dockerLoginPassword"

phases:
  install:
    commands:
      - echo Entered the install phase...
      - apt-get update -y
      - apt-get install -y maven
  pre_build:
    commands:
      - echo Entered the pre_build phase...
      - docker login –u User –p $LOGIN_PASSWORD
  build:
    commands:
      - echo Entered the build phase...
      - echo Build started on `date`
      - mvn install
  post_build:
    commands:
      - echo Entered the post_build phase...
      - echo Build completed on `date`
artifacts:
  files:
    - target/messageUtil-1.0.jar
  discard-paths: yes
cache:
  paths:
    - '/root/.m2/**/*'
```

Here is an example of the preceding build spec, expressed as a single string, for use with the AWS CLI, or the AWS SDKs\.

```
"version: 0.2\n\nenv:\n  variables:\n    JAVA_HOME: "/usr/lib/jvm/java-8-openjdk-amd64"\n  parameter-store:\n    LOGIN_PASSWORD: "dockerLoginPassword"\n\nphases:\n  install:\n    commands:\n      - apt-get update -y\n      - apt-get install -y maven\n  pre_build:\n    commands:\n      - echo Entered the pre_build phase...\n  build:\n    commands:\n      - echo Build started on `date`\n      - mvn install\n  post_build:\n    commands:\n      - echo Build completed on `date`\nartifacts:\n  files:\n    - target/messageUtil-1.0.jar\n  discard-paths: yes"
```

Here is an example of the commands in the `build` phase, for use with the AWS CodeBuild or AWS CodePipeline consoles\.

```
echo Build started on `date` && mvn install
```

In these examples:

+ A custom environment variable in plain text with the key of `JAVA_HOME` and the value of `/usr/lib/jvm/java-8-openjdk-amd64` will be set\.

+ A custom environment variable named `dockerLoginPassword` you stored in Amazon EC2 Systems Manager Parameter Store will be referenced later in build commands by using the key `LOGIN_PASSWORD`\.

+ You cannot change these build phase names\. The commands that will be run in this example are `apt-get update -y` and `apt-get install -y maven` \(to install Apache Maven\), `mvn install` \(to compile, test, and package the source code into a build output artifact and to perform other actions, such as install the build output artifact in its internal repository\), `docker login` \(to log in to Docker with the password that corresponds to the value of the custom environment variable `dockerLoginPassword` you set in Amazon EC2 Systems Manager Parameter Store\), and several `echo` commands\. The `echo` commands are included here to show how AWS CodeBuild runs commands and the order in which it runs them\. 

+ `files` represents the files to upload to the build output location\. In this example, AWS CodeBuild will upload the single file `messageUtil-1.0.jar`\. The `messageUtil-1.0.jar` file can be found in the relative directory named `target` in the build environment\. Because `discard-paths: yes` is specified, `messageUtil-1.0.jar` will be uploaded directly \(and not to an intermediate `target` directory\)\. The file name `messageUtil-1.0.jar` and the relative directory name of `target` is based on the way Apache Maven creates and stores build output artifacts for this example only\. In your own scenarios, these file names and directories will be different\. 

## Build Spec Versions<a name="build-spec-ref-versions"></a>

The following table lists the build spec versions and the changes between versions\.


****  

| Version | Changes | 
| --- | --- | 
| 0\.2 |  `environment_variables` has been renamed to `env`\. `plaintext` has been renamed to `variables`\. In version 0\.1, AWS CodeBuild runs each build command in a separate instance of the default shell in the build environment\. In verson 0\.2, this issue is addressed; AWS CodeBuild runs all build commands in the same instance of the default shell in the build environment\.  | 
| 0\.1 | This is the initial definition of the build specification format\. | 