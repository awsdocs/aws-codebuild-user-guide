# Build caching in AWS CodeBuild<a name="build-caching"></a>

 You can save time when your project builds by using a cache\. A cache can store reusable pieces of your build environment and use them across multiple builds\. Your build project can use one of two types of caching: Amazon S3 or local\. If you use a local cache, you must choose one or more of three cache modes: source cache, Docker layer cache, and custom cache\. 

**Note**  
Docker layer cache mode is available for the Linux environment only\. If you choose this mode, you must run your build in privileged mode\. CodeBuild projects granted privileged mode grants its container access to all devices\. For more information, see [Runtime privilege and Linux capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) on the Docker Docs website\.

**Topics**
+ [Amazon S3 caching](#caching-s3)
+ [Local caching](#caching-local)

## Amazon S3 caching<a name="caching-s3"></a>

 Amazon S3 caching stores the cache in an Amazon S3 bucket that is available across multiple build hosts\. This is a good option for small intermediate build artifacts that are more expensive to build than to download\. This is not the best option for large build artifacts because they can take a long time to transfer over your network, which can affect build performance\. It also is not the best option if you use Docker layers\. 

## Local caching<a name="caching-local"></a>

 Local caching stores a cache locally on a build host that is available to that build host only\. This is a good option for large intermediate build artifacts because the cache is immediately available on the build host\. This is not the best option if your builds are infrequent\. This means that build performance is not impacted by network transfer time\. If you choose local caching, you must choose one or more of the following cache modes: 
+  Source cache mode caches Git metadata for primary and secondary sources\. After the cache is created, subsequent builds pull only the change between commits\. This mode is a good choice for projects with a clean working directory and a source that is a large Git repository\. If you choose this option and your project does not use a Git repository \(GitHub, GitHub Enterprise Server, or Bitbucket\), the option is ignored\. 
+  Docker layer cache mode caches existing Docker layers\. This mode is a good choice for projects that build or pull large Docker images\. It can prevent the performance issues caused by pulling large Docker images down from the network\. 
**Note**  
 You can use a Docker layer cache in the Linux environment only\. 
 The `privileged` flag must be set so that your project has the required Docker permissions\.   
By default, Docker containers do not allow access to any devices\. Privileged mode grants a build project's Docker container access to all devices\. For more information, see [Runtime Privilege and Linux Capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) on the Docker Docs website\.
 You should consider the security implication before you use a Docker layer cache\. 
+  Custom cache mode caches directories you specify in the buildspec file\. This mode is a good choice if your build scenario is not suited to one of the other two local cache modes\. If you use a custom cache: 
  +  Only directories can be specified for caching\. You cannot specify individual files\. 
  +  Symlinks are used to reference cached directories\. 
  +  Cached directories are linked to your build before it downloads its project sources\. Cached items overrides source items if they have the same name\. Directories are specified using cache paths in the buildspec file\. For more information, see [Buildspec syntax](build-spec-ref.md#build-spec-ref-syntax)\. 
  + Avoid directory names that are the same in the source and in the cache\. Locally\-cached directories may override, or delete the contents of, directories in the source repository that have the same name\.

**Note**  
The `ARM_CONTAINER` and `LINUX_GPU_CONTAINER` environment types and the `BUILD_GENERAL1_2XLARGE` compute type do not support the use of a local cache\. For more information, see [Build environment compute types](build-env-ref-compute-types.md)\.

**Topics**
+ [Specify local caching \(CLI\)](#caching-local-cli)
+ [Specify local caching \(console\)](#caching-local-console)
+ [Specify local caching \(AWS CloudFormation\)](#caching-local-cfn)

 You can use the AWS CLI, console, SDK, or AWS CloudFormation to specify a local cache\. 

### Specify local caching \(CLI\)<a name="caching-local-cli"></a>

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

For more information, see [Create a build project \(AWS CLI\)](create-project-cli.md)\.

### Specify local caching \(console\)<a name="caching-local-console"></a>

You specify a cache in the **Artifacts** section of the console\. For **Cache type**, choose **Amazon S3** or **Local**\. If you choose **Local**, choose one or more of the three local cache options\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/local-cache.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

For more information, see [Create a build project \(console\)](create-project-console.md)\.

### Specify local caching \(AWS CloudFormation\)<a name="caching-local-cfn"></a>

 If you use AWS CloudFormation to specify a local cache, on the `Cache` property, for `Type`, specify `LOCAL`\. The following sample YAML\-formatted AWS CloudFormation code specifies all three local cache types\. You can specify any combination of the types\. If you use a Docker layer cache, under `Environment`, you must set `PrivilegedMode` to `true` and `Type` to `LINUX_CONTAINER`\. 

```
CodeBuildProject:
    Type: AWS::CodeBuild::Project
    Properties:
      Name: MyProject
      ServiceRole: <service-role>
      Artifacts:
        Type: S3
        Location: myBucket
        Name: myArtifact
        EncryptionDisabled: true
        OverrideArtifactName: true
      Environment:
        Type: LINUX_CONTAINER
        ComputeType: BUILD_GENERAL1_SMALL
        Image: aws/codebuild/standard:4.0
        Certificate: bucket/cert.zip
        # PrivilegedMode must be true if you specify LOCAL_DOCKER_LAYER_CACHE
        PrivilegedMode: true
      Source:
        Type: GITHUB
        Location: <github-location>
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

**Note**  
By default, Docker containers do not allow access to any devices\. Privileged mode grants a build project's Docker container access to all devices\. For more information, see [Runtime Privilege and Linux Capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) on the Docker Docs website\.

For more information, see [Create a build project \(AWS CloudFormation\)](create-project-cloud-formation.md)\.