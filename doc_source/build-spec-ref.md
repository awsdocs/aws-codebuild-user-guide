--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Build Specification Reference for AWS CodeBuild<a name="build-spec-ref"></a>

This topic provides important reference information about build specification \(build spec\) files\. A *build spec* is a collection of build commands and related settings, in YAML format, that AWS CodeBuild uses to run a build\. You can include a build spec as part of the source code or you can define a build spec when you create a build project\. For information about how a build spec works, see [How AWS CodeBuild Works](concepts.md#concepts-how-it-works)\.

**Topics**
+ [Build Spec File Name and Storage Location](#build-spec-ref-name-storage)
+ [Build Spec Syntax](#build-spec-ref-syntax)
+ [Build Spec Example](#build-spec-ref-example)
+ [Build Spec Versions](#build-spec-ref-versions)

## Build Spec File Name and Storage Location<a name="build-spec-ref-name-storage"></a>

If you include a build spec as part of the source code, by default, the build spec file must be named `buildspec.yml` and placed in the root of your source directory\.

You can override the default build spec file name and location\. For example, you can:
+ Use a different build spec file for different builds in the same repository, such as `buildspec_debug.yml` and `buildspec_release.yml`\.
+ Store a build spec file somewhere other than the root of your source directory, such as `config/buildspec.yml`\.

You can specify only one build spec for a build project, regardless of the build spec file's name\.

To override the default build spec file name, location, or both, do one of the following:
+ Run the AWS CLI `create-project` or `update-project` command, setting the `buildspec` value to the path to the alternate build spec file relative to the value of the built\-in environment variable `CODEBUILD_SRC_DIR`\. You can also do the equivalent with the `create project` operation in the AWS SDKs\. For more information, see [Create a Build Project](create-project.md) or [Change a Build Project's Settings](change-project.md)\.
+ Run the AWS CLI `start-build` command, setting the `buildspecOverride` value to the path to the alternate build spec file relative to the value of the built\-in environment variable `CODEBUILD_SRC_DIR`\. You can also do the equivalent with the `start build` operation in the AWS SDKs\. For more information, see [Run a Build](run-build.md)\.
+ In an AWS CloudFormation template, set the `BuildSpec` property of `Source` in a resource of type `AWS::CodeBuild::Project` to the path to the alternate build spec file relative to the value of the built\-in environment variable `CODEBUILD_SRC_DIR`\. For more information, see the BuildSpec property in [AWS CodeBuild Project Source](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-codebuild-project-source.html) in the *AWS CloudFormation User Guide*\.

## Build Spec Syntax<a name="build-spec-ref-syntax"></a>

Build spec files must be expressed in [YAML](http://yaml.org/) format\.

The build spec has the following syntax:

```
version: 0.2

run-as: Linux-user-name

env:
  variables:
    key: "value"
    key: "value"
  parameter-store:
    key: "value"
    key: "value"
            
phases:
  install:
    run-as: Linux-user-name
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
artifacts:
  files:
    - location
    - location
    - name
  discard-paths: yes
  base-directory: location
  secondary-artifacts:
    artifactIdentifier:
      files:
        - location
        - location
      discard-paths: yes
      base-directory: location
    artifactIdentifier:
      files:
        - location
        - location
      discard-paths: yes
      base-directory: location
cache:
  paths:
    - path
    - path
```

The build spec contains the following:
+ `version`: Required mapping\. Represents the build spec version\. We recommend that you use `0.2`\.
**Note**  
Although version 0\.1 is still supported, we recommend that you use version 0\.2 whenever possible\. For more information, see [Build Spec Versions](#build-spec-ref-versions)\.
+ `run-as`: Optional sequence\. Available to Linux users only\. Specifies a Linux user that runs commands in this buildspec file\. `run-as` grants the specified user read and execute perimissions\. When you specify `run-as` at the top of the buildspec file, it applies globally to all commands\. If you don't want to specify a user for all buildspec file commands, you can specify one for commands in a phase by using `run-as` in one of the `phases` blocks\. If `run-as` is not specified, then all commands run as the root\.
+ `env`: Optional sequence\. Represents information for one or more custom environment variables\.
  + `variables`: Required if `env` is specified, and you want to define custom environment variables in plain text\. Contains a mapping of *key*/*value* scalars, where each mapping represents a single custom environment variable in plain text\. *key* is the name of the custom environment variable, and *value* is that variable's value\.
**Important**  
We strongly discourage the storing of sensitive values, especially AWS access key IDs and secret access keys, in environment variables\. Environment variables can be displayed in plain text using tools such as the AWS CodeBuild console and the AWS CLI\. For sensitive values, we recommend that you use the `parameter-store` mapping instead, as described later in this section\.  
Any environment variables you set replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you set an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you set an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\.  
Do not set any environment variable with a name that starts with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, the value is determined as follows:  
The value in the start build operation call takes highest precedence\.
The value in the build project definition takes next precedence\.
The value in the build spec declaration takes lowest precedence\.
  + `parameter-store`: Required if `env` is specified, and you want to retrieve custom environment variables stored in Amazon EC2 Systems Manager Parameter Store\. Contains a mapping of *key*/*value* scalars, where each mapping represents a single custom environment variable stored in Amazon EC2 Systems Manager Parameter Store\. *key* is the name you use later in your build commands to refer to this custom environment variable, and *value* is the name of the custom environment variable stored in Amazon EC2 Systems Manager Parameter Store\. To store sensitive values, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\. 
**Important**  
To allow AWS CodeBuild to retrieve custom environment variables stored in Amazon EC2 Systems Manager Parameter Store, you must add the `ssm:GetParameters` action to your AWS CodeBuild service role\. For more information, see [Create an AWS CodeBuild Service Role](setting-up.md#setting-up-service-role)\.  
Any environment variables you retrieve from Amazon EC2 Systems Manager Parameter Store replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you retrieve an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you retrieve an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\.  
Do not store any environment variable with a name that starts with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, the value is determined as follows:  
The value in the start build operation call takes highest precedence\.
The value in the build project definition takes next precedence\.
The value in the build spec declaration takes lowest precedence\.
+ `phases`: Required sequence\. Represents the commands AWS CodeBuild runs during each phase of the build\. 
**Note**  
In build spec version 0\.1, AWS CodeBuild runs each command in a separate instance of the default shell in the build environment\. This means that each command runs in isolation from all other commands\. Therefore, by default, you cannot run a single command that relies on the state of any previous commands \(for example, changing directories or setting environment variables\)\. To get around this limitation, we recommend that you use version 0\.2, which solves this issue\. If you must use build spec version 0\.1, we recommend the approaches in [Shells and Commands in Build Environments](build-env-ref-cmd.md)\.
  + `run-as`: Optional sequence\. Use in a build phase to specify a Linux user that runs its commands\. If `run-as` is also specified globally for all commands at the top of the buildspec file, then the phase\-level user takes precedence\. For example, if globally `run-as` specifies User\-1, and for the `install` phase only a `run-as` statement specifies User\-2, then all commands in then buildspec file are run as User\-1 *except* commands in the `install` phase, which are run as User\-2\.

  The allowed build phase names are:
  + `install`: Optional sequence\. Represents the commands, if any, that AWS CodeBuild runs during installation\. We recommend that you use the `install` phase only for installing packages in the build environment\. For example, you might use this phase to install a code testing framework such as Mocha or RSpec\.
    + `commands`: Required sequence if `install` is specified\. Contains a sequence of scalars, where each scalar represents a single command that AWS CodeBuild runs during installation\. AWS CodeBuild runs each command, one at a time, in the order listed, from beginning to end\.
  + `pre_build`: Optional sequence\. Represents the commands, if any, that AWS CodeBuild runs before the build\. For example, you might use this phase to sign in to Amazon ECR, or you might install npm dependencies\. 
    + `commands`: Required sequence if `pre_build` is specified\. Contains a sequence of scalars, where each scalar represents a single command that AWS CodeBuild runs before the build\. AWS CodeBuild runs each command, one at a time, in the order listed, from beginning to end\.
  + `build`: Optional sequence\. Represents the commands, if any, that AWS CodeBuild runs during the build\. For example, you might use this phase to run Mocha, RSpec, or sbt\.
    + `commands`: Required if `build` is specified\. Contains a sequence of scalars, where each scalar represents a single command that AWS CodeBuild runs during the build\. AWS CodeBuild runs each command, one at a time, in the order listed, from beginning to end\.
  + `post_build`: Optional sequence\. Represents the commands, if any, that AWS CodeBuild runs after the build\. For example, you might use Maven to package the build artifacts into a JAR or WAR file, or you might push a Docker image into Amazon ECR\. Then you might send a build notification through Amazon SNS\.
    + `commands`: Required if `post_build` is specified\. Contains a sequence of scalars, where each scalar represents a single command that AWS CodeBuild runs after the build\. AWS CodeBuild runs each command, one at a time, in the order listed, from beginning to end\.
**Important**  
Commands in some build phases might not be run if commands in earlier build phases fail\. For example, if a command fails during the `install` phase, none of the commands in the `pre_build`, `build`, and `post_build` phases are run for that build's lifecycle\. For more information, see [Build Phase Transitions](view-build-details.md#view-build-details-phases)\.
+ `finally`: Optional block\. Commands specified in a `finally` block are executed after commands in the `commands` block\. The commands in a `finally` block are executed even if a command in the `commands` block fails\. For example, if the `commands` block contains three commands and the first fails, AWS CodeBuild skips the remaining two commands and runs any commands in the `finally` block\. The phase is successful when all commands in the `commands` and the `finally` blocks run successfully\. If any command in a phase fails, the phase fails\.
+ `artifacts`: Optional sequence\. Represents information about where AWS CodeBuild can find the build output and how AWS CodeBuild prepares it for uploading to the Amazon S3 output bucket\. This sequence is not required if, for example, you are building and pushing a Docker image to Amazon ECR, or you are running unit tests on your source code, but not building it\.
  + `files`: Required sequence\. Represents the locations that contain the build output artifacts in the build environment\. Contains a sequence of scalars, with each scalar representing a separate location where AWS CodeBuild can find build output artifacts, relative to the original build location or, if set, the base directory\. Locations can include the following:
    + A single file \(for example, `my-file.jar`\)\.
    + A single file in a subdirectory \(for example, `my-subdirectory/my-file.jar` or `my-parent-subdirectory/my-subdirectory/my-file.jar`\)\.
    + `'**/*'` represents all files recursively\.
    + `my-subdirectory/*` represents all files in a subdirectory named *my\-subdirectory*\.
    + `my-subdirectory/**/*` represents all files recursively starting from a subdirectory named *my\-subdirectory*\.

    When you specify build output artifact locations, AWS CodeBuild can locate the original build location in the build environment\. You do not have to prepend your build artifact output locations with the path to the original build location or specify `./` or similar\. If you want to know the path to this location, you can run a command such as `echo $CODEBUILD_SRC_DIR` during a build\. The location for each build environment might be slightly different\. 
  + `name`: Optional name\. Specifies a name for your build artifact\. This name is used when one of the following is true\.
    +  You use the AWS CodeBuild API to create your builds and the `overrideArtifactName` flag is set on the `ProjectArtifacts` object when a project is updated, a project is created, or a build is started\. 
    +  You use the AWS CodeBuild console to create your builds, a name is specified in the buildspec file, and you select **Use the name specified in the buildspec file** when you create or update a project\. For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console)\. 

     You can specify a name in the build spec file that is calculated at build time\. The name specified in a build spec file uses the Shell command language\. For example, you can append a date and time to your artifact name so that it is always unique\. Unique artifact names prevent artifacts from being overwritten\. For more information, see [Shell Command Language](http://pubs.opengroup.org/onlinepubs/9699919799/)\. 

     This is an example of an arifact name appended with the date the artifact is created\. 

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

     This is an example of an artifact name that uses an AWS CodeBuild environment variable\. For more information, see [Environment Variables in Build Environments](build-env-ref-env-vars.md)\. 

    ```
    version: 0.2         
    phases:
      build:
        commands:
          - rspec HelloWorld_spec.rb
    artifacts:
      files:
        - '**/*'
      name: myname-$(AWS_REGION)
    ```

     This is an example of an artifact name that uses an AWS CodeBuild environment variable with the artifact's creation date appended to it\. 

    ```
    version: 0.2         
    phases:
      build:
        commands:
          - rspec HelloWorld_spec.rb
    artifacts:
      files:
        - '**/*'
      name: $(AWS_REGION)-$(date +%Y-%m-%d)
    ```
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
  + `secondary-artifacts`: Optional sequence\. Represents one or more artifact definitions as a mapping between an artifact identifier and an artifact definition\. Each artifact identifiers in this block must match an artifact defined in the `secondaryArtifacts` attribute of your project\. Each separate definition has the same syntax as the `artifacts:` block above\. For example, if your project has the following structure:

    ```
      {
        "name": "sample-project",
        "secondaryArtifacts": [
          {
            "type": "S3",
            "location": "output-bucket1",
            "artifactIdentifier": "artifact1"
          },
          {
            "type": "S3",
            "location": "output-bucket2",
            "artifactIdentifier": "artifact2"
          }
        ]
      }
    ```

    Then your buildpec looks like the following:

    ```
    version: 0.2
    
    phases:
    build:
      commands:
        - echo Building...
    artifacts:
      secondary-artifacts:
        artifact1:
          files:
            - directory/file
        artifact2:
          files:
            - directory/file2
    ```
+ `cache`: Optional sequence\. Represents information about where AWS CodeBuild can prepare the files for uploading cache to an Amazon S3 cache bucket\. This sequence is not required if the cache type of the project is `No Cache`\. 
  + `paths`: Required sequence\. Represents the locations of the cache\. Contains a sequence of scalars, with each scalar representing a separate location where AWS CodeBuild can find build output artifacts, relative to the original build location or, if set, the base directory\. Locations can include the following:
    + A single file \(for example, `my-file.jar`\)\.
    + A single file in a subdirectory \(for example, `my-subdirectory/my-file.jar` or `my-parent-subdirectory/my-subdirectory/my-file.jar`\)\.
    + `'**/*'` represents all files recursively\.
    + `my-subdirectory/*` represents all files in a subdirectory named *my\-subdirectory*\.
    + `my-subdirectory/**/*` represents all files recursively starting from a subdirectory named *my\-subdirectory*\.

**Important**  
Because a build spec declaration must be valid YAML, the spacing in a build spec declaration is important\. If the number of spaces in your build spec declaration is invalid, builds might fail immediately\. You can use a YAML validator to test whether your build spec declarations are valid YAML\.   
If you use the AWS CLI, or the AWS SDKs to declare a build spec when you create or update a build project, the build spec must be a single string expressed in YAML format, along with required whitespace and newline escape characters\. There is an example in the next section\.  
If you use the AWS CodeBuild or AWS CodePipeline consoles instead of a buildspec\.yml file, you can insert commands for the `build` phase only\. Instead of using the preceding syntax, you list, in a single line, all of the commands that you want to run during the build phase\. For multiple commands, separate each command by `&&` \(for example, `mvn test && mvn package`\)\.  
You can use the AWS CodeBuild or AWS CodePipeline consoles instead of a buildspec\.yml file to specify the locations of the build output artifacts in the build environment\. Instead of using the preceding syntax, you list, in a single line, all of the locations\. For multiple locations, separate each location with a comma \(for example, `buildspec.yml, target/my-app.jar`\)\. 

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
artifacts:
  files:
    - target/messageUtil-1.0.jar
  discard-paths: yes
secondary-artifacts:
  artifact1:
    files:
      - target/messageUtil-1.0.jar
    discard-paths: yes
  artifact2:
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
+ A custom environment variable, in plain text, with the key of `JAVA_HOME` and the value of `/usr/lib/jvm/java-8-openjdk-amd64`, is set\.
+ A custom environment variable named `dockerLoginPassword` you stored in Amazon EC2 Systems Manager Parameter Store is referenced later in build commands by using the key `LOGIN_PASSWORD`\.
+ You cannot change these build phase names\. The commands that are run in this example are `apt-get update -y` and `apt-get install -y maven` \(to install Apache Maven\), `mvn install` \(to compile, test, and package the source code into a build output artifact and to install the build output artifact in its internal repository\), `docker login` \(to sign in to Docker with the password that corresponds to the value of the custom environment variable `dockerLoginPassword` you set in Amazon EC2 Systems Manager Parameter Store\), and several `echo` commands\. The `echo` commands are included here to show how AWS CodeBuild runs commands and the order in which it runs them\. 
+ `files` represents the files to upload to the build output location\. In this example, AWS CodeBuild uploads the single file `messageUtil-1.0.jar`\. The `messageUtil-1.0.jar` file can be found in the relative directory named `target` in the build environment\. Because `discard-paths: yes` is specified, `messageUtil-1.0.jar` is uploaded directly \(and not to an intermediate `target` directory\)\. The file name `messageUtil-1.0.jar` and the relative directory name of `target` is based on the way Apache Maven creates and stores build output artifacts for this example only\. In your own scenarios, these file names and directories will be different\. 

## Build Spec Versions<a name="build-spec-ref-versions"></a>

The following table lists the build spec versions and the changes between versions\.


****  

| Version | Changes | 
| --- | --- | 
| 0\.2 |  `environment_variables` has been renamed to `env`\. `plaintext` has been renamed to `variables`\. In version 0\.1, AWS CodeBuild runs each build command in a separate instance of the default shell in the build environment\. In version 0\.2, AWS CodeBuild runs all build commands in the same instance of the default shell in the build environment\.  | 
| 0\.1 | This is the initial definition of the build specification format\. | 