# Batch build buildspec reference<a name="batch-build-buildspec"></a>

This topic contains the buildspec reference for batch build properties\.

## batch<a name="build-spec.batch"></a>

Optional mapping\. The batch build settings for the project\.

batch/**fast\-fail**  
Optional\.     
`false`  
The default value\. All running builds will complete\.   
`true`  
All running builds will be stopped if one of the builds fails\.

By default, all batch build tasks run with the build settings such as `env` and `phases`, specified in the buildspec file\. You can override the default build settings by specifying different `env` values or a different buildspec file in the `batch/<batch-type>/buildspec` parameter\.

The contents of the `batch` property varies based on the type of batch build being specified\. The possible batch build types are:
+ [`batch/build-graph`](#build-spec.batch.build-graph)
+ [`batch/build-list`](#build-spec.batch.build-list)
+ [`batch/build-matrix`](#build-spec.batch.build-matrix)

## `batch/build-graph`<a name="build-spec.batch.build-graph"></a>

Defines a *build graph*\. A build graph defines a set of tasks that have dependencies on other tasks in the batch\. For more information, see [Build graph](batch-build.md#batch_build_graph)\.

This element contains an array of build tasks\. Each build task contains the following properties\.

**identifier**  
Required\. The identifier of the task\.

**buildspec**  
Optional\. The path and file name of the buildspec file to use for this task\.

**debug\-session**  
Optional\. A Boolean value that indicates whether session debugging is enabled for this batch build\. For more information about session debugging, see [View a running build in Session Manager](session-manager.md)\.    
`false`  
Session debugging is disabled\.   
`true`  
Session debugging is enabled\. 

**depend\-on**  
Optional\. An array of task identifiers that this task depends on\. This task will not run until these tasks are completed\.

**env**  
Optional\. The build environment overrides for the task\. This can contain the following properties:    
**compute\-type**  
The identifier of the compute type to use for the task\. See **computeType** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
**image**  
The identifier of the image to use for the task\. See **Image identifier** in [Docker images provided by CodeBuild](build-env-ref-available.md) for possible values\.  
**privileged\-mode**  
A Boolean value that indicates whether to run the Docker daemon inside a Docker container\. Set to `true` only if the build project is used to build Docker images\. Otherwise, a build that attempts to interact with the Docker daemon fails\. The default setting is `false`\.  
**type**  
The identifier of the environment type to use for the task\. See **Environment type** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
**variables**  
The environment variables that will be present in the build environment\. See [env/variables](build-spec-ref.md#build-spec.env.variables) for more information, \.

**ignore\-failure**  
Optional\. A Boolean value that indicates whether failures in the batch can be ignored\.    
`false`  
The default value\. If one build task fails, the batch build will fail\.   
`true`  
If one build task fails, the remaining build tasks will still run\. 

The following is an example of a build graph buildspec entry:

```
batch:
  fast-fail: false
  build-graph:
    - identifier: build1
      env:
        compute-type: BUILD_GENERAL1_SMALL
      debug-session: true
    - identifier: build2
      env:
        compute-type: BUILD_GENERAL1_MEDIUM
      depend-on:
        - build1
      debug-session: false
    - identifier: build3
      env:
        compute-type: BUILD_GENERAL1_LARGE
      depend-on:
        - build2
```

## `batch/build-list`<a name="build-spec.batch.build-list"></a>

Defines a *build list*\. A build list is used to define a number of tasks that run in parallel\. For more information, see [Build list](batch-build.md#batch_build_list)\.

This element contains an array of build tasks\. Each build task contains the following properties\.

**identifier**  
Required\. The identifier of the task\.

**buildspec**  
Optional\. The path and file name of the buildspec file to use for this task\.

**debug\-session**  
Optional\. A Boolean value that indicates whether session debugging is enabled for this batch build\. For more information about session debugging, see [View a running build in Session Manager](session-manager.md)\.    
`false`  
Session debugging is disabled\.   
`true`  
Session debugging is enabled\. 

**env**  
Optional\. The build environment overrides for the task\. This can contain the following properties:    
**compute\-type**  
The identifier of the compute type to use for the task\. See **computeType** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
**image**  
The identifier of the image to use for the task\. See **Image identifier** in [Docker images provided by CodeBuild](build-env-ref-available.md) for possible values\.  
**privileged\-mode**  
A Boolean value that indicates whether to run the Docker daemon inside a Docker container\. Set to `true` only if the build project is used to build Docker images\. Otherwise, a build that attempts to interact with the Docker daemon fails\. The default setting is `false`\.  
**type**  
The identifier of the environment type to use for the task\. See **Environment type** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
**variables**  
The environment variables that will be present in the build environment\. See [env/variables](build-spec-ref.md#build-spec.env.variables) for more information, \.

**ignore\-failure**  
Optional\. A Boolean value that indicates whether failures in the batch can be ignored\.    
`false`  
The default value\. If one build task fails, the batch build will fail\.   
`true`  
If one build task fails, the remaining build tasks will still run\. 

The following is an example of a build list buildspec entry:

```
batch:
  fast-fail: false
  build-list:
    - identifier: linux_small
      env:
        compute-type: BUILD_GENERAL1_SMALL
      ignore-failure: true
      debug-session: true
    - identifier: windows_medium
      env:
        type: WINDOWS_SERVER_2019_CONTAINER
        image: aws/codebuild/windows-base:2019-1.0
        compute-type: BUILD_GENERAL1_MEDIUM
```

## `batch/build-matrix`<a name="build-spec.batch.build-matrix"></a>

Defines a *build matrix*\. A build matrix is used to define tasks that will run in parallel with different environments\. CodeBuild creates a separate build for each possible environment configuration\. For more information, see [Build matrix](batch-build.md#batch_build_matrix)\.

**static**  
The static properties apply to all build tasks\.    
**ignore\-failure**  
Optional\. A Boolean value that indicates whether failures in the batch can be ignored\.    
`false`  
The default value\. If one build task fails, the batch build will fail\.   
`true`  
If one build task fails, the remaining build tasks will still run\.   
**env**  
Optional\. The build environment overrides for all tasks\.     
**compute\-type**  
The identifier of the compute type to use for the task\. See **computeType** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
**image**  
The identifier of the image to use for the task\. See **Image identifier** in [Docker images provided by CodeBuild](build-env-ref-available.md) for possible values\.  
**privileged\-mode**  
A Boolean value that indicates whether to run the Docker daemon inside a Docker container\. Set to `true` only if the build project is used to build Docker images\. Otherwise, a build that attempts to interact with the Docker daemon fails\. The default setting is `false`\.  
**type**  
The identifier of the environment type to use for the task\. See **Environment type** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
**variables**  
The environment variables that will be present in the build environment\. See [env/variables](build-spec-ref.md#build-spec.env.variables) for more information, \.  
**privileged\-mode**  
Optional\. A Boolean value that indicates whether to run the Docker daemon inside a Docker container\. Set to `true` only if the build project is used to build Docker images\. Otherwise, a build that attempts to interact with the Docker daemon fails\. The default setting is `false`\.  
**type**  
Optional\. The identifier of the environment type to use for the task\. See **Environment Type** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.

**dynamic**  
The dynamic properties define the build matrix\.    
**buildspec**  
Optional\. An array that contains the path and file name of the buildspec files to use for these tasks\.  
**env**  
Optional\. The build environment overrides for the task\.     
**compute\-type**  
An array that containbs the identifiers of the compute types to use for these tasks\. See **computeType** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
**image**  
Optional\. An array that contains the identifiers of the images to use for these tasks\. See **Image identifier** in [Docker images provided by CodeBuild](build-env-ref-available.md) for possible values\.  
**variables**  
An array that contains the environment variables that will be present in the build environments for these tasks\. See [env/variables](build-spec-ref.md#build-spec.env.variables) for more information, \.

The following is an example of a build matrix buildspec entry:

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

For more information, see [Build matrix](batch-build.md#batch_build_matrix)\.