# Batch build buildspec reference<a name="batch-build-buildspec"></a>

## batch<a name="build-spec.batch"></a>

Optional mapping\. Represents the batch build settings for the project\.

By default, all of the batch build tasks are run with the build settings, such as `env` and `phases`, specified in this buildspec\. You can override the default build settings by specifying different `env` values, or an entirely different buildspec file in the `batch/<batch-type>/buildspec` parameter\.

The contents of the `batch` property varies based on the type of batch being specified\. The possible batch types are:

**Topics**
+ [`batch-graph`](#build-spec.batch.build-graph)
+ [`batch-list`](#build-spec.batch.build-list)
+ [`batch-matrix`](#build-spec.batch.build-matrix)

### `batch-graph`<a name="build-spec.batch.build-graph"></a>

Defines a *build graph*\. A build graph is used to define a set of tasks that have dependencies on other tasks in the batch\. 

batch/ **fast\-fail**   
Optional\.     
 `false`   
The default value\. All running builds will complete\.   
 `true`   
All running builds will be stopped when one of the builds fail\.

batch/batch\-graph/**buildspec**  
Optional\. Specifies the path and file name of the buildspec file to use for this task\.

batch/batch\-graph/**depend\-on**  
An array of task identifiers that this task depends on\. This task will not be run until these tasks are completed\.

batch/batch\-graph/**env**  
Optional\. Defines the build environment overrides for the task\.     
batch/batch\-graph/env/**compute\-type**  
The identifier of the compute type to use for the task\. See **computeType** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
batch/batch\-graph/env/**image**  
The identifier of the image to use for the task\. See **Image identifier** in [Docker images provided by CodeBuild](build-env-ref-available.md) for possible values\.  
batch/batch\-graph/env/**type**  
The identifier of the environment type to use for the task\. See **Environment type** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
batch/batch\-graph/env/**variables**  
Defines the environment variables that will be present in the build environment\. See [env/variables](build-spec-ref.md#build-spec.env.variables) for more information, \.

batch/batch\-graph/**identifier**  
Required\. The identifier of the task\.

The following is an example of a build graph buildspec entry:

```
batch:
  fast-fail: false
  build-graph:
    - identifier: linux_small
      env:
        compute-type: BUILD_GENERAL1_SMALL
    - identifier: linux_medium
      env:
        compute-type: BUILD_GENERAL1_MEDIUM
      depend-on:
        - linux_small
    - identifier: linux_large
      env:
        compute-type: BUILD_GENERAL1_LARGE
      depend-on:
        - linux_medium
```

### `batch-list`<a name="build-spec.batch.build-list"></a>

Defines a *build list*\. A build list is used to define a number of tasks that run in parallel\. 

batch/ **fast\-fail**   
Optional\.     
 `false`   
The default value\. All running builds will complete\.   
 `true`   
All running builds will be stopped when one of the builds fail\. 

batch/batch\-list/**env**  
Optional\. Defines the build environment overrides for the task\.     
batch/batch\-graph/env/**compute\-type**  
The identifier of the compute type to use for the task\. See **computeType** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
batch/batch\-graph/env/**image**  
The identifier of the image to use for the task\. See **Image identifier** in [Docker images provided by CodeBuild](build-env-ref-available.md) for possible values\.  
batch/batch\-graph/env/**type**  
The identifier of the environment type to use for the task\. See **Environment type** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
batch/batch\-graph/env/**variables**  
Defines the environment variables that will be present in the build environment\. See [env/variables](build-spec-ref.md#build-spec.env.variables) for more information, \.

batch/batch\-list/**identifier**  
Optional\. The identifier of the task\.

The following is an example of a build list buildspec entry:

```
batch:
  fast-fail: false
  build-list:
    - identifier: linux_small
      env:
        compute-type: BUILD_GENERAL1_SMALL
    - identifier: windows_medium
      env:
        type: WINDOWS_CONTAINER
        image: aws/codebuild/windows-base:2.0
        compute-type: BUILD_GENERAL1_MEDIUM
```

### `batch-matrix`<a name="build-spec.batch.build-matrix"></a>

Defines a *build matrix*\. A build matrix is used to define tasks that will run in parallel with different environments\. CodeBuild creates a separate build for each possible environment configuration\. For example, if your build matrix has two images and three values for an environment variable, such as this:

```
batch:
  build-matrix:
    env:
      image:
        - aws/codebuild/amazonlinux2-x86_64-standard:3.0
        - aws/codebuild/windows-base:2.0
      variables:
        MY_VAR:
          - VALUE1
          - VALUE2
          - VALUE3
```

CodeBuild will create six builds:
+ `aws/codebuild/amazonlinux2-x86_64-standard:3.0` / `MY_VAR=VALUE1`
+ `aws/codebuild/amazonlinux2-x86_64-standard:3.0` / `MY_VAR=VALUE2`
+ `aws/codebuild/amazonlinux2-x86_64-standard:3.0` / `MY_VAR=VALUE3`
+ `aws/codebuild/windows-base:2.0` / `MY_VAR=VALUE1`
+ `aws/codebuild/windows-base:2.0` / `MY_VAR=VALUE2`
+ `aws/codebuild/windows-base:2.0` / `MY_VAR=VALUE3`

batch/batch\-matrix/**env**  
Optional\. Defines the build environment overrides for the task\.     
batch/batch\-matrix/env/**compute\-type**  
The identifier of the compute type to use for the task\. See **computeType** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
batch/batch\-matrix/env/**image**  
The identifier of the image to use for the task\. See **Image identifier** in [Docker images provided by CodeBuild](build-env-ref-available.md) for possible values\.  
batch/batch\-matrix/env/**variables**  
Defines the environment variables that will be present in the build environment\. See [env/variables](build-spec-ref.md#build-spec.env.variables) for more information, \.