# Build Caching in CodeBuild<a name="build-caching"></a>

 You can save time when your project builds by using a cache\. A cache can store reusable pieces of your build environment and use them across multiple builds\. Your build project can use one of two types of caching: Amazon S3 or local\. If you use a local cache, you must choose one or more of three cache modes: source cache, Docker layer cache, and custom cache\. 

**Note**  
Docker layer cache mode is available for the Linux environment only\. If you choose this mode, you must run your build in privileged mode\.

**Topics**
+ [Amazon S3 Caching](#caching-s3)
+ [Local Caching](#caching-local)

## Amazon S3 Caching<a name="caching-s3"></a>

 Amazon S3 caching stores the cache in an Amazon S3 bucket that is available across multiple build hosts\. This is a good option for small intermediate build artifacts that are more expensive to build than to download\. This is not the best option for large build artifacts because they can take a long time to transfer over your network, which can affect build performance\. 

## Local Caching<a name="caching-local"></a>

 Local caching stores a cache locally on a build host that is available to that build host only\. This is a good option for large intermediate build artifacts because the cache is immediately available on the build host\. This means that build performance is not impacted by network transfer time\. If you choose local caching, you must choose one or more of the following cache modes: 
+  Source cache mode caches Git metadata for primary and secondary sources\. After the cache is created, subsequent builds pull only the change between commits\. This mode is a good choice for projects with a clean working directory and a source that is a large Git repository\. If you choose this option and your project does not use a Git repository \(GitHub, GitHub Enterprise, or Bitbucket\), the option is ignored\. 
+  Docker layer cache mode caches existing Docker layers\. This mode is a good choice for projects that build or pull large Docker images\. It can prevent the performance issues caused by pulling large Docker images down from the network\. 
**Note**  
 You can use a Docker layer cache in the Linux environment only\. 
 The `privileged` flag must be set so that your project has the required Docker permissions\. 
 You should consider the security implications before you use a Docker layer cache\. 
+  Custom cache mode caches directories you specify in the buildspec file\. This mode is a good choice if your build scenario is not suited to one of the other two local cache modes\. If you use a custom cache: 
  +  Only directories can be specified for caching\. You cannot specify individual files\. 
  +  Symlinks are used to reference cached directories\. 
  +  Cached directories are linked to your build before it downloads its project sources\. Cached items are overriden if a source item has the same name\. Directories are specified using cache paths in the buildspec file\. For more information, see [Build Spec Syntax](build-spec-ref.md#build-spec-ref-syntax)\. 

**Topics**
+ [Specify Local Caching \(CLI\)](#caching-local-cli)
+ [Specify Local Caching \(Console\)](#caching-local-console)
+ [Specify Local Caching \(AWS CloudFormation\)](#caching-local-cfn)

 You can use the AWS CLI, console, SDK, or AWS CloudFormation to specify a local cache\. 

### Specify Local Caching \(CLI\)<a name="caching-local-cli"></a>

 You can use the the `--cache` parameter in the AWS CLI to specify each of the three local cache types\. 
+  To specify a source cache: 

  ```
  --cache type=LOCAL,mode=[LOCAL_SOURCE_CACHE]
  ```
+  To specify a Docker layer cache: 

  ```
  --cache type=LOCAL,mode=[LOCAL_DOCKER_LAYER_CACHE]
  ```
+  To specify a custom cache: 

  ```
  --cache type=LOCAL,mode=[LOCAL_CUSTOM_CACHE]
  ```

For more information, see [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli)\.

### Specify Local Caching \(Console\)<a name="caching-local-console"></a>

You specify a cache in the **Artifacts** section of the console\. For ** Cache type**, choose **Amazon S3** or **Local**\. If you choose **Local**, choose one or more of the three local cache options\. The following shows how to choose a local cache\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/local-cache.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console)\.

### Specify Local Caching \(AWS CloudFormation\)<a name="caching-local-cfn"></a>

 If you use AWS CloudFormation to specify a local cache, on the `Cache` property, for `Type`, you specify `LOCAL`\. The following sample YAML\-formatted AWS CloudFormation code specifies all three local cache types\. You can specify any combination of the types\. If you use a Docker layer cache, under `Environment`, you must set `PrivilegedMode` to `true` and `Type` to `LINUX_CONTAINER`\. 

```
CodeBuildProject:
    Type: AWS::CodeBuild::Project
    Properties:
      Name: MyProject
      ServiceRole: service-role
      Artifacts:
        Type: S3
        Location: myBucket
        Name: myArtifact
        EncryptionDisabled: true
        OverrideArtifactName: true
      Environment:
        Type: LINUX_CONTAINER
        ComputeType: BUILD_GENERAL1_SMALL
        Image: aws/codebuild/standard:2.0
        Certificate: bucket/cert.zip
        # PrivilegedMode must be true if you specify LOCAL_DOCKER_LAYER_CACHE
        PrivilegedMode: true
      Source:
        Type: GITHUB
        Location: github-location
        InsecureSsl: true
        GitCloneDepth: 1
        ReportBuildStatus: false
      TimeoutInMinutes: 10
      Cache:
        Type: LOCAL
        Modes: # You can specify one or more cache mode, 
          - LOCAL_CUSTOM_CACHE
          - LOCAL_DOCKER_LAYER_CACHE
          - LOCAL_SOURCE_CACHE
```

For more information, see [Create a Build Project \(AWS CloudFormation\)](create-project.md#create-project-cloud-formation)\.
