# Step 4: Upload the source code and the buildspec file<a name="getting-started-upload-source-code-console"></a>

\(Previous step: [Step 3: Create the buildspec file](getting-started-create-build-spec-console.md)\)

In this step, you add the source code and build spec file to the input bucket\.

Using your operating system's zip utility, create a file named `MessageUtil.zip` that includes `MessageUtil.java`, `TestMessageUtil.java`, `pom.xml`, and `buildspec.yml`\. 

The `MessageUtil.zip` file's directory structure must look like this\.

```
MessageUtil.zip
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

**Important**  
Do not include the `(root directory name)` directory, only the directories and files in the `(root directory name)` directory\.

Upload the `MessageUtil.zip` file to the input bucket named `codebuild-region-ID-account-ID-input-bucket`\. 

**Important**  
For CodeCommit, GitHub, and Bitbucket repositories, by convention, you must store a build spec file named `buildspec.yml` in the root \(top level\) of each repository or include the build spec declaration as part of the build project definition\. Do not create a ZIP file that contains the repository's source code and build spec file\.   
For build input stored in S3 buckets only, you must create a ZIP file that contains the source code and, by convention, a build spec file named `buildspec.yml` at the root \(top level\) or include the build spec declaration as part of the build project definition\.  
If you want to use a different name for your build spec file, or you want to reference a build spec in a location other than the root, you can specify a build spec override as part of the build project definition\. For more information, see [Buildspec file name and storage location](build-spec-ref.md#build-spec-ref-name-storage)\.

## Next step<a name="getting-started-upload-source-code-console-next"></a>

[Step 5: Create the build project](getting-started-create-build-project-console.md)