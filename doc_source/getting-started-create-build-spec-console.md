# Step 3: Create the buildspec file<a name="getting-started-create-build-spec-console"></a>

\(Previous step: [Step 2: Create the source code](getting-started-create-source-code-console.md)\)

In this step, you create a build specification \(build spec\) file\. A *buildspec* is a collection of build commands and related settings, in YAML format, that CodeBuild uses to run a build\. Without a build spec, CodeBuild cannot successfully convert your build input into build output or locate the build output artifact in the build environment to upload to your output bucket\.

Create this file, name it `buildspec.yml`, and then save it in the root \(top level\) directory\.

```
version: 0.2

phases:
  install:
    runtime-versions:
      java: corretto11
  pre_build:
    commands:
      - echo Nothing to do in the pre_build phase...
  build:
    commands:
      - echo Build started on `date`
      - mvn install
  post_build:
    commands:
      - echo Build completed on `date`
artifacts:
  files:
    - target/messageUtil-1.0.jar
```

**Important**  
Because a build spec declaration must be valid YAML, the spacing in a build spec declaration is important\. If the number of spaces in your build spec declaration does not match this one, the build might fail immediately\. You can use a YAML validator to test whether your build spec declaration is valid YAML\. 

**Note**  
Instead of including a build spec file in your source code, you can declare build commands separately when you create a build project\. This is helpful if you want to build your source code with different build commands without updating your source code's repository each time\. For more information, see [Buildspec syntax](build-spec-ref.md#build-spec-ref-syntax)\.

In this build spec declaration:
+ `version` represents the version of the build spec standard being used\. This build spec declaration uses the latest version, `0.2`\.
+ `phases` represents the build phases during which you can instruct CodeBuild to run commands\. These build phases are listed here as `install`, `pre_build`, `build`, and `post_build`\. You cannot change the spelling of these build phase names, and you cannot create more build phase names\. 

  In this example, during the `build` phase, CodeBuild runs the `mvn install` command\. This command instructs Apache Maven to compile, test, and package the compiled Java class files into a build output artifact\. For completeness, a few `echo` commands are placed in each build phase in this example\. When you view detailed build information later in this tutorial, the output of these `echo` commands can help you better understand how CodeBuild runs commands and in which order\. \(Although all build phases are included in this example, you are not required to include a build phase if you do not plan to run any commands during that phase\.\) For each build phase, CodeBuild runs each specified command, one at a time, in the order listed, from beginning to end\. 
+ `artifacts` represents the set of build output artifacts that CodeBuild uploads to the output bucket\. `files` represents the files to include in the build output\. CodeBuild uploads the single `messageUtil-1.0.jar` file found in the `target` relative directory in the build environment\. The file name `messageUtil-1.0.jar` and the directory name `target` are based on the way Apache Maven creates and stores build output artifacts for this example only\. In your own builds, these file names and directories are different\. 

For more information, see the [Buildspec reference](build-spec-ref.md)\.

At this point, your directory structure should look like this\.

```
(root directory name)
    |-- pom.xml
    |-- buildspec.yml
    `-- src
         |-- main
         |     `-- java
         |           `-- MessageUtil.java
         `-- test
               `-- java
                     `-- TestMessageUtil.java
```

## Next step<a name="getting-started-create-build-spec-console-next"></a>

[Step 4: Upload the source code and the buildspec file](getting-started-upload-source-code-console.md)