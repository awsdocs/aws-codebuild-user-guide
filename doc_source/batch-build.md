# Batch builds in AWS CodeBuild<a name="batch-build"></a>

You can use AWS CodeBuild to run concurrent and coordinated builds of a project with batch builds\. 

**Topics**
+ [Security role](#batch_security_role)
+ [Batch build types](#batch_build_types)
+ [More information](#batch_more_info)

## Security role<a name="batch_security_role"></a>

Batch builds introduce a new security role in the batch configuration\. This new role is required as CodeBuild must be able to call the `StartBuild`, `StopBuild`, and `RetryBuild` actions on your behalf to run builds as part of a batch\. Customers should use a new role, and not the same role they use in their build, for two reasons:
+ Giving the build role `StartBuild`, `StopBuild`, and `RetryBuild` permissions would allow a single build to start more builds via the buildspec\.
+ CodeBuild batch builds provide restrictions that restrict the number of builds and compute types that can be used for the builds in the batch\. If the build role has these permissions, it is possible the builds themselves could bypass these restrictions\.

## Batch build types<a name="batch_build_types"></a>

CodeBuild supports the following batch build types:

**Topics**
+ [Build graph](#batch_build_graph)
+ [Build list](#batch_build_list)
+ [Build matrix](#batch_build_matrix)

### Build graph<a name="batch_build_graph"></a>

A build graph defines a set of tasks that have dependencies on other tasks in the batch\. 

The following example defines a build graph that creates a dependency chain\. 

```
batch:
  fast-fail: false
  build-graph:
    - identifier: build1
      env:
        compute-type: BUILD_GENERAL1_SMALL
    - identifier: build2
      env:
        compute-type: BUILD_GENERAL1_MEDIUM
      depend-on:
        - build1
    - identifier: build3
      env:
        compute-type: BUILD_GENERAL1_LARGE
      depend-on:
        - build2
```

In this example:
+ `build1` runs first because it has no dependencies\.
+ `build2` has a dependency on `build1`, so `build2` runs after `build1` completes\.
+ `build3` has a dependency on `build2`, so `build3` runs after `build2` completes\.

For more information about the build graph buildspec syntax, see [`batch/build-graph`](batch-build-buildspec.md#build-spec.batch.build-graph)\.

### Build list<a name="batch_build_list"></a>

A build list defines a number of tasks that run in parallel\. 

The following example defines a build list\. The `linux_small` and `windows_medium` builds will be run in parallel\.

```
batch:
  fast-fail: false
  build-list:
    ignore-failure: true
      - identifier: linux_small
        env:
          compute-type: BUILD_GENERAL1_SMALL
      - identifier: windows_medium
        env:
          type: WINDOWS_SERVER_2019_CONTAINER
          image: aws/codebuild/windows-base:2019-1.0
          compute-type: BUILD_GENERAL1_MEDIUM
```

For more information about the build list buildspec syntax, see [`batch/build-list`](batch-build-buildspec.md#build-spec.batch.build-list)\.

### Build matrix<a name="batch_build_matrix"></a>

A build matrix defines tasks that will run in parallel with different environments\. CodeBuild creates a separate build for each possible environment configuration\. 

The following example shows a build matrix with two images and three values for an environment variable\.

```
batch:
  build-matrix:
    static:
      ignore-failure: false
      env:
        type: LINUX_CONTAINER
        privileged-mode: true
    dynamic:
      env:
        image:
          - aws/codebuild/amazonlinux2-x86_64-standard:3.0
          - aws/codebuild/windows-base:2019-1.0
        variables:
          MY_VAR:
            - VALUE1
            - VALUE2
            - VALUE3
```

In this example, CodeBuild creates six builds:
+ `aws/codebuild/amazonlinux2-x86_64-standard:3.0` / `MY_VAR=VALUE1`
+ `aws/codebuild/amazonlinux2-x86_64-standard:3.0` / `MY_VAR=VALUE2`
+ `aws/codebuild/amazonlinux2-x86_64-standard:3.0` / `MY_VAR=VALUE3`
+ `aws/codebuild/windows-base:2019-1.0` / `MY_VAR=VALUE1`
+ `aws/codebuild/windows-base:2019-1.0` / `MY_VAR=VALUE2`
+ `aws/codebuild/windows-base:2019-1.0` / `MY_VAR=VALUE3`

Each build will have the following settings:
+ `ignore-failure` set to `false`
+ `env/type` set to `LINUX_CONTAINER`
+ `env/privileged`\-mode set to `true`

These builds run in parallel\.

For more information about the build matrix buildspec syntax, see [`batch/build-matrix`](batch-build-buildspec.md#build-spec.batch.build-matrix)\.

## More information<a name="batch_more_info"></a>

For more information, see the following topics:
+ [Batch build buildspec reference](batch-build-buildspec.md)
+ [Batch configuration](create-project-console.md#create-project-console-batch-config)
+ [Run a batch build \(AWS CLI\)](run-batch-build-cli.md)
+ [Stop a batch build in AWS CodeBuild ](stop-batch-build.md)