--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Shells and Commands in Build Environments<a name="build-env-ref-cmd"></a>

You provide a set of commands for AWS CodeBuild to run in a build environment during the lifecycle of a build \(for example, installing build dependencies and testing and compiling your source code\)\. There are several ways to specify these commands:
+ Create a build spec file and include it with your source code\. In this file, specify the commands you want to run in each phase of the build lifecycle\. For more information, see the [Build Specification Reference for AWS CodeBuild](build-spec-ref.md)\.
+ Use the AWS CodeBuild console to create a build project\. In **Insert build commands**, for **Build commands**, enter the commands you want to run in the `build` phase\. For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console)\.
+ Use the AWS CodeBuild console to change the settings of a build project\. In **Insert build commands**, for **Build commands**, enter the commands you want to run in the `build` phase\. For more information, see [Change a Build Project's Settings \(Console\)](change-project.md#change-project-console)\.
+ Use the AWS CLI or AWS SDKs to create a build project or change the settings of a build project\. Reference the source code that contains a build spec file with your commands, or specify a single string that includes the contents of an equivalent build spec file\. For more information, see [Create a Build Project](create-project.md) or [Change a Build Project's Settings](change-project.md)\.
+ Use the AWS CLI or AWS SDKs to start a build, specifying a build spec file or a single string that includes the contents of an equivalent build spec file\. For more information, see the description for the `buildspecOverride` value in [Run a Build](run-build.md)\.

You can specify any Shell command\. In build spec version 0\.1, AWS CodeBuild runs each Shell command in a separate instance in the build environment\. This means that each command runs in isolation from all other commands\. Therefore, by default, you cannot run a single command that relies on the state of any previous commands \(for example, changing directories or setting environment variables\)\. To get around this limitation, we recommend that you use version 0\.2, which solves this issue\. If you must use version 0\.1, we recommend the following approaches:
+ Include a shell script in your source code that contains the commands you want to run in a single instance of the default shell\. For example, you could include a file named `my-script.sh` in your source code that contains commands such as `cd MyDir; mkdir -p mySubDir; cd mySubDir; pwd;`\. Then, in your build spec file, specify the command `./my-script.sh`\. 
+ In your build spec file or on the **Build commands** setting for the `build` phase only, enter a single command that includes all of the commands you want to run in a single instance of the default shell \(for example, `cd MyDir && mkdir -p mySubDir && cd mySubDir && pwd`\)\. 

If AWS CodeBuild encounters an error, the error might be more difficult to troubleshoot compared to running a single command in its own instance of the default shell\.