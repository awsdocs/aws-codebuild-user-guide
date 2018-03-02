# Shells and Commands in Build Environments<a name="build-env-ref-cmd"></a>

You provide a set of commands for AWS CodeBuild to run in a build environment during the lifecycle of a build \(for example, installing build dependencies and testing and compiling your source code\)\. There are several ways to specify these commands:

+ Create a build spec file and include it with your source code\. In this file, specify the commands you want to run in each phase of the build lifecycle\. For more information, see the [Build Specification Reference for AWS CodeBuild](build-spec-ref.md)\.

+ Use the AWS CodeBuild or AWS CodePipeline console to create a build project\. In **Insert build commands**, for **Build command**, specify the commands you want to run in the `build` phase\. For more information, see the description of **Insert build commands** in [Create a Build Project \(Console\)](create-project.md#create-project-console) or [Use AWS CodePipeline with AWS CodeBuild](how-to-create-pipeline.md)\.

+ Use the AWS CodeBuild console to change the settings of a build project\. In **Insert build commands**, for **Build command**, specify the commands you want to run in the `build` phase\. For more information, see the description of **Insert build commands** in [Change a Build Project's Settings \(Console\)](change-project.md#change-project-console)\.

+ Use the AWS CLI, or AWS SDKs to create a build project or change the settings of a build project\. Reference the source code that contains a build spec file with your commands, or specify a single string that includes the contents of an equivalent build spec file\. For more information, see the description for the `buildspec` value in [Create a Build Project](create-project.md) or [Change a Build Project's Settings](change-project.md)\.

+ Use the AWS CLI, or AWS SDKs to start a build, specifying a build spec file or a single string that includes the contents of an equivalent build spec file\. For more information, see the description for the `buildspecOverride` value in [Run a Build](run-build.md)\.

You can specify any command that is supported by the build environment's default shell \(\- `sh` is the default shell in curated image\)\. In build spec version 0\.1, AWS CodeBuild runs each command in a separate instance of the default shell in the build environment\. This means that each command runs in isolation from all other commands\. Therefore, by default, you cannot run a single command that relies on the state of any previous commands \(for example, changing directories or setting environment variables\)\. To get around this limitation, we recommend you use version 0\.2, which solves this issue\. If you must use version 0\.1 for some reason, we recommend the following approaches:

+ Include a shell script in your source code that contains the commands you want to run in a single instance of the default shell\. For example, you could include a file named `my-script.sh` in your source code that contains commands such as `cd MyDir; mkdir -p mySubDir; cd mySubDir; pwd;`\. Then, in your build spec file, specify the command `./my-script.sh`\. 

+ In your build spec file, or for the console's **Build command** setting for the `build` phase only, specify a single command that includes all of the commands you want to run in a single instance of the default shell \(for example, `cd MyDir && mkdir -p mySubDir && cd mySubDir && pwd`\)\. 

If AWS CodeBuild encounters an error, the error might be more difficult to troubleshoot compared to running a single command in its own instance of the default shell\.