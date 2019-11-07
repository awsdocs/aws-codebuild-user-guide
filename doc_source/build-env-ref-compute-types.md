# Build Environment Compute Types<a name="build-env-ref-compute-types"></a>

AWS CodeBuild provides build environments with the following available memory, vCPUs, and disk space:


****  

| Compute type | computeType value | Memory | vCPUs | Disk space | Operating system | 
| --- | --- | --- | --- | --- | --- | 
| build\.general1\.small | BUILD\_GENERAL1\_SMALL | 3 GB | 2 | 64 GB | Linux | 
| build\.general1\.medium | BUILD\_GENERAL1\_MEDIUM | 7 GB | 4 | 128 GB | Linux, Windows | 
| build\.general1\.large | BUILD\_GENERAL1\_LARGE | 15 GB | 8 | 128 GB | Linux, Windows | 

**Note**  
For custom build environment images, CodeBuild supports Docker images up to 50 GB uncompressed in Linux and Windows, regardless of the compute type\. To check your build image's size, use Docker to run the `docker images REPOSITORY:TAG` command\.

To choose a compute type:
+ In the CodeBuild console, in the **Create build project** wizard or **Edit Build Project** page, in **Environment** expand **Additional configuration**, and then choose one of the options from **Compute type**\. For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console) or [Change a Build Project's Settings \(Console\)](change-project.md#change-project-console)\.
+ For the AWS CLI, run the `create-project` or `update-project` command, specifying the `computeType` value of the `environment` object\. For more information, see [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli) or [Change a Build Project's Settings \(AWS CLI\)](change-project.md#change-project-cli)\.
+ For the AWS SDKs, call the equivalent of the `CreateProject` or `UpdateProject` operation for your target programming language, specifying the equivalent of `computeType` value of the `environment` object\. For more information, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.

You can use Amazon EFS to access more space in your build container\. For more information, see [Amazon Elastic File System Sample for CodeBuild](sample-efs.md)\. If you want to manipulate container disk space during a build, then the build must run in privileged mode\.