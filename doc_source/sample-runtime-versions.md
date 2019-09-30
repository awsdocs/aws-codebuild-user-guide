# Runtime Versions in Buildspec File Sample for CodeBuild<a name="sample-runtime-versions"></a>

 If you use the Amazon Linux 2 \(AL2\) standard image version 1\.0 or later, or the Ubuntu standard image version 2\.0 or later, you must specify at least one runtime and its version in the `runtime-versions` section of your buildspec file\. This sample shows how you can change your project runtime, specify more than one runtime, and specify a runtime that is dependent on another runtime\. For information about supported runtimes, see [Docker Images Provided by CodeBuild](build-env-ref-available.md)\.

**Note**  
 If you use Docker in your build container, then your build must run in privileged mode\. For more information, see [Run a Build in CodeBuild](run-build.md) and [Create a Build Project in CodeBuild](create-project.md)\. 

## Update Your Runtime Version<a name="sample-runtime-update-version"></a>

 You can modify the runtime used by your project to a new version by updating the `runtime-versions` section of your buildpec file\. The following examples show how to specify Corretto versions 8 and 11:
+  A `runtime-versions` section that specifies version 8 of Corretto \(Amazon Linux 2 only\): 

  ```
  phases:
    install:
      runtime-versions:
        java: corretto8
  ```
+  A `runtime-versions` section that specifies version 11 of Corretto \(Amazon Linux 2 only\): 

  ```
  phases:
    install:
      runtime-versions:
        java: corretto11
  ```

The following examples show how to specify Java versions 8 and 10: 
+  A `runtime-versions` section that specifies version 8 of Java \(Ubuntu only\): 

  ```
  phases:
    install:
      runtime-versions:
        java: openjdk8
  ```
+  A `runtime-versions` section that specifies version 11 of Java \(Ubuntu only\): 

  ```
  phases:
    install:
      runtime-versions:
        java: openjdk11
  ```

 The following examples show how you to specify different versions of Node\.js: 
+  A `runtime-versions` section that specifies Node\.js version 8: 

  ```
  phases:
    install:
      runtime-versions:
        nodejs: 8
  ```
+  A `runtime-versions` section that specifies Node\.js version 10: 

  ```
  phases:
    install:
      runtime-versions:
        nodejs: 10
  ```

 This sample demonstrates a project that starts with the Java version 8 runtime, and then is updated to the Java version 10 runtime\. 

1.  Follow steps 1 and 2 in [Create the Source Code](sample-elastic-beanstalk.md#sample-elastic-beanstalk-prepare-source) to generate source code\. If successful, a directory named `my-web-app` is created with your source files\. 

1.  Create a file named `buildspec.yml` with the following contents\. Store the file in the ` (root directory name)/my-web-app` directory\. 

   ```
   version: 0.2
   
   phases:
     install:
       runtime-versions:
         java: openjdk8
     build:
       commands:
         - java -version
         - mvn package
   artifacts:
     files:
       - '**/*'
     base-directory: 'target/my-web-app'
   ```

    In the buildspec file: 
   +  The `runtime-versions` section specifies that the project uses version 8 of the Java runtime\. 
   +  The `- java -version` command displays the version of Java used by your project when it builds\. 

    Your file structure should now look like this\. 

   ```
   (root directory name)
     -- my-web-app
          |-- src    
          |     `-- main
          |           |-- resources
          |           `-- webapp
          |                 |-- WEB-INF
          |                 |     `-- web.xml
          |                 `-- index.jsp
          |-- buildspec.yml
          `-- pom.xml
   ```

1.  Upload the contents of the `my-web-app` directory to an Amazon S3 input bucket or a CodeCommit, GitHub, or Bitbucket repository\. 
**Important**  
Do not upload `(root directory name)` or `(root directory name)/my-web-app`, just the directories and files in `(root directory name)/my-web-app`\.   
If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the directory structure and files, and then upload it to the input bucket\. Do not add `(root directory name)` or `(root directory name)/my-web-app` to the ZIP file, just the directories and files in `(root directory name)/my-web-app`\.

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. Create a build project\. For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console) and [Run a Build \(Console\)](run-build.md#run-build-console)\. Leave all settings at their default values, except for these settings\.
   + For **Environment**:
     + For **Environment image**, choose **Managed image**\.
     + For **Operating system**, choose **Amazon Linux 2**\. 
     +  For **Runtime\(s\)**, choose **Standard**\. 
     + For **Image**, choose **aws/codebuild/amazonlinux2\-x86\_64\-standard:1\.0**\.

1.  Choose **Start build**\. 

1.  On **Build configuration** accept the defaults, and then choose **Start build**\. 

1.  After the build is complete, view the build output on the **Build logs** tab\. You should see output similar to the following: 

   ```
   [Container] 2019/05/14 20:45:07 Entering phase INSTALL 
   [Container] Date Time Running command echo "Installing Java version 8 ..." 
   Installing Java version 8 ... 
    
   [Container] Date Time Running command export JAVA_HOME="$JAVA_8_HOME" 
    
   [Container] Date Time Running command export JRE_HOME="$JRE_8_HOME" 
    
   [Container] Date Time Running command export JDK_HOME="$JDK_8_HOME" 
    
   [Container] Date Time Running command for tool_path in "$JAVA_8_HOME"/bin/* "$JRE_8_HOME"/bin/*;
   ```

1.  Update the `runtime-versions` section with Java version 11: 

   ```
   install:
       runtime-versions:
         java: openjdk11
   ```

1.  After you save the change, run your build again and view the build output\. You should see that the installed version of Java is 11\. You should see output similar to the following: 

   ```
   [Container] 2019/05/14 20:45:07 Entering phase INSTALL 
   [Container] Date Time Running command echo "Installing Java version 11 ..." 
   Installing Java version 11 ... 
    
   [Container] Date Time Running command export JAVA_HOME="$JAVA_11_HOME" 
    
   [Container] Date Time Running command export JRE_HOME="$JRE_11_HOME" 
    
   [Container] Date Time Running command export JDK_HOME="$JDK_11_HOME" 
    
   [Container] Date Time Running command for tool_path in "$JAVA_11_HOME"/bin/* "$JRE_11_HOME"/bin/*;
   ```

## Specify a Runtime Dependency<a name="sample-runtime-dependent-runtime"></a>

 This example shows how to specify a runtime and a dependency runtime\. For example, the Android runtime version 28 is dependent on the Java or Corretto runtime version 8\. If you specify Android version 28 and use Amazon Linux 2, you must also specify Corretto version 8\. If you specify Android version 28 and use Ubuntu, you must also specify Java version 8\. 

 The build project in this example uses source code in the GitHub [AWS Samples](https://github.com/aws-samples) repository\. The source code uses the Android version 28 runtime and the build project uses Amazon Linux 2, so the buildspec must also specify Corretto version 8\. 

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. Create a build project\. For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console) and [Run a Build \(Console\)](run-build.md#run-build-console)\. Leave all settings at their default values, except for these settings\.
   + For **Environment**:
     + For **Source provider**, choose **GitHub**\.
     + For **Repository**, choose **Public repository**\.
     + For **Repository URL**, type **https://github\.com/aws\-samples/aws\-mobile\-android\-notes\-tutorial**\.
     + For **Environment image**, choose **Managed image**\.
     + For **Operating system**, choose **Amazon Linux 2**\. 
     +  For **Runtime\(s\)**, choose **Standard**\. 
     + For **Image**, choose **aws/codebuild/amazonlinux2\-x86\_64\-standard:1\.0**\.

1.  For **Build specifications**, choose **Insert build commands**, and then choose **Switch to editor**\. 

1.  In **Build commands**, replace the placeholder text with the following: 

   ```
   version: 0.2
   
   phases:
     install:
       runtime-versions:
         android: 28
         java: corretto8
     build:
       commands:
         - ./gradlew assembleDebug
   artifacts:
     files:
       - app/build/outputs/apk/app-debug.apk
   ```

    The `runtime-versions` section specifies both Android version 28 and Java version 8 runtimes\. 

1.  Choose **Create build project**\. 

1.  Choose **Start build**\. 

1.  On **Build configuration** accept the defaults, and then choose **Start build**\. 

1.  After the build is complete, view the build output on the **Build logs** tab\. You should see output similar to the following\. It shows that Android version 28 and Corretto version 8 are installed: 

   ```
   [Container] 2019/05/14 23:21:42 Entering phase INSTALL 
   [Container] Date Time Running command echo "Installing Android version 28 ..." 
   Installing Android version 28 ... 
    
   [Container] Date Time Running command echo "Installing Java version 8 ..." 
   Installing Java version 8 ...
   ```

## Specify Two Runtimes<a name="sample-runtime-two-major-version-runtimes"></a>

 You can specify more than one runtime in the same CodeBuild build project\. This sample project uses two source files: one that uses the Go runtime and one that uses the Node\.js runtime\. 

1.  Create a directory named `my-source`\. 

1.  Inside the `my-source` directory, create a directory named `golang-app`\. 

1.  Create a file named `hello.go` with the following contents\. Store the file in the `golang-app` directory\. 

   ```
   package main
   import "fmt"
   
   func main() {
     fmt.Println("hello world from golang")
     fmt.Println("1+1 =", 1+1)
     fmt.Println("7.0/3.0 =", 7.0/3.0)
     fmt.Println(true && false)
     fmt.Println(true || false)
     fmt.Println(!true)
     fmt.Println("good bye from golang")
   }
   ```

1.  Inside the `my-source` directory, create a directory named `nodejs-app`\. It should be at the same level as the `golang-app` directory\. 

1.  Create a file named `index.js` with the following contents\. Store the file in the `nodejs-app` directory\. 

   ```
   console.log("hello world from nodejs");
   console.log("1+1 =" + (1+1));
   console.log("7.0/3.0 =" + 7.0/3.0);
   console.log(true && false);
   console.log(true || false);
   console.log(!true);
   console.log("good bye from nodejs");
   ```

1.  Create a file named `package.json` with the following contents\. Store the file in the `nodejs-app` directory\. 

   ```
   {
     "name": "mycompany-app",
     "version": "1.0.0",
     "description": "",
     "main": "index.js",
     "scripts": {
       "test": "echo \"run some tests here\""
     },
     "author": "",
     "license": "ISC"
   }
   ```

1.  Create a file named `buildspec.yml` with the following contents\. Store the file in the `my-source` directory, at the same level as the `nodejs-app` and `golang-app` directories\. The `runtime-versions` section specifies the Node\.js version 10 and Go version 1\.12 runtimes\. 

   ```
   version: 0.2
   
   phases:
     install:
       runtime-versions:
         golang: 1.12
         nodejs: 10
     build:
       commands:
         - echo Building the Go code...
         - cd $CODEBUILD_SRC_DIR/golang-app
         - go build hello.go 
         - echo Building the Node code...
         - cd $CODEBUILD_SRC_DIR/nodejs-app
         - npm run test
   artifacts:
     secondary-artifacts:
       golang_artifacts:
         base-directory: golang-app
         files:
           - hello
       nodejs_artifacts:
         base-directory: nodejs-app
         files:
           - index.js
           - package.json
   ```

1.  Your file structure should now look like this\. 

   ```
   -- my-source
       |-- golang-app    
       |     -- hello.go
       |-- nodejs.app
       |     -- index.js
       |     -- package.json
       |-- buildspec.yml
   ```

1. Upload the contents of the `my-source` directory to an Amazon S3 input bucket or a CodeCommit, GitHub, or Bitbucket repository\.
**Important**  
 If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the directory structure and files, and then upload it to the input bucket\. Do not add `my-source` to the ZIP file, just the directories and files in `my-source`\.

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. Create a build project\. For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console) and [Run a Build \(Console\)](run-build.md#run-build-console)\. Leave all settings at their default values, except for these settings\.
   + For **Environment**:
     + For **Environment image**, choose **Managed image**\.
     + For **Operating system**, choose **Amazon Linux 2**\.
     + For **Runtime\(s\)**, choose **Standard**\.
     + For **Image**, choose **aws/codebuild/amazonlinux2\-x86\_64\-standard:1\.0**\.

1.  Choose **Create build project**\. 

1.  Choose **Start build**\. 

1.  On **Build configuration**, accept the defaults, and then choose **Start build**\. 

1.  After the build is complete, view the build output on the **Build logs** tab\. You should see output similar to the following\. It shows output from the Go and Node\.js runtimes\. It also shows output from the Go and Node\.js applications\. 

   ```
   [Container] Date Time Entering phase INSTALL 
   [Container] Date Time Running command echo "Installing Go version 1.12 ..." 
   Installing Go version 1.12 ... 
    
   [Container] Date Time Running command echo "Installing Node.js version 10 ..." 
   Installing Node.js version 10 ... 
    
   [Container] Date Time Running command n 10.15.3 
    
   [Container] Date Time Phase complete: INSTALL State: SUCCEEDED 
   [Container] Date Time Phase context status code:  Message:  
   [Container] Date Time Entering phase PRE_BUILD 
   [Container] Date Time Phase complete: PRE_BUILD State: SUCCEEDED 
   [Container] Date Time Phase context status code:  Message:  
   [Container] Date Time Entering phase BUILD 
   [Container] Date Time Running command echo Building the Go code... 
   Building the Go code... 
    
   [Container] Date Time Running command cd $CODEBUILD_SRC_DIR/golang-app 
    
   [Container] Date Time Running command go build hello.go 
    
   [Container] Date Time Running command echo Building the Node code... 
   Building the Node code... 
    
   [Container] Date Time Running command cd $CODEBUILD_SRC_DIR/nodejs-app 
    
   [Container] Date Time Running command npm run test 
    
   > mycompany-app@1.0.0 test /codebuild/output/src924084119/src/nodejs-app 
   > echo "run some tests here" 
    
   run some tests here
   ```