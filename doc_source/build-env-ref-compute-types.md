# Build environment compute types<a name="build-env-ref-compute-types"></a>

AWS CodeBuild provides build environments with the following available memory, vCPUs, and disk space:


| Compute type | Environment computeType value | Environment type value | Memory | vCPUs | Disk space | 
| --- | --- | --- | --- | --- | --- | 
| ARM Large | BUILD\_GENERAL1\_LARGE | ARM\_CONTAINER | 16 GB | 8 | 50 GB | 
| Linux Small | BUILD\_GENERAL1\_SMALL | LINUX\_CONTAINER | 3 GB | 2 | 64 GB | 
| Linux Medium | BUILD\_GENERAL1\_MEDIUM | LINUX\_CONTAINER | 7 GB | 4 | 128 GB | 
| Linux Large | BUILD\_GENERAL1\_LARGE | LINUX\_CONTAINER | 15 GB | 8 | 128 GB | 
| Linux 2XLarge | BUILD\_GENERAL1\_2XLARGE | LINUX\_CONTAINER | 145 GB | 72 | 824 GB \(SSD\) | 
| Linux GPU Large | BUILD\_GENERAL1\_LARGE | LINUX\_GPU\_CONTAINER | 255 GB | 32 | 50 GB | 
| Windows Medium | BUILD\_GENERAL1\_MEDIUM | WINDOWS\_SERVER\_2019\_CONTAINER | 7 GB | 4 | 128 GB | 
| Windows Large | BUILD\_GENERAL1\_LARGE | WINDOWS\_SERVER\_2019\_CONTAINER | 15 GB | 8 | 128 GB | 

The disk space listed for each build environment is available only in the directory specified by the `CODEBUILD_SRC_DIR` environment variable\.

To choose a compute type:
+ In the CodeBuild console, in the **Create build project** wizard or **Edit Build Project** page, in **Environment** expand **Additional configuration**, and then choose one of the options from **Compute type**\. For more information, see [Create a build project \(console\)](create-project-console.md) or [Change a build project's settings \(console\)](change-project-console.md)\.
+ For the AWS CLI, run the `create-project` or `update-project` command, specifying the `computeType` value of the `environment` object\. For more information, see [Create a build project \(AWS CLI\)](create-project-cli.md) or [Change a build project's settings \(AWS CLI\)](change-project-cli.md)\.
+ For the AWS SDKs, call the equivalent of the `CreateProject` or `UpdateProject` operation for your target programming language, specifying the equivalent of `computeType` value of the `environment` object\. For more information, see the [AWS SDKs and tools reference](sdk-ref.md)\.

Some environment and compute types have Region availability limitations: 
+ The environment type `LINUX_GPU_CONTAINER` is only available in these Regions:
  + US East \(N\. Virginia\)
  + US West \(Oregon\)
  + Asia Pacific \(Seoul\)
  + Asia Pacific \(Singapore\)
  + Asia Pacific \(Sydney\)
  + Asia Pacific \(Tokyo\)
  + Canada \(Central\)
  + China \(Beijing\)
  + China \(Ningxia\)
  + Europe \(Frankfurt\)
  + Europe \(Ireland\)
  + Europe \(London\)
+ The environment type `ARM_CONTAINER` is only available in these Regions:
  + US East \(Ohio\)
  + US East \(N\. Virginia\)
  + US West \(N\. California\)
  + US West \(Oregon\)
  + Asia Pacific \(Mumbai\)
  + Asia Pacific \(Singapore\)
  + Asia Pacific \(Sydney\)
  + Asia Pacific \(Tokyo\)
  + Europe \(Frankfurt\)
  + Europe \(Ireland\)
+ The compute type `BUILD_GENERAL1_2XLARGE` is only available in these Regions:
  + US East \(Ohio\)
  + US East \(N\. Virginia\)
  + US West \(N\. California\)
  + US West \(Oregon\)
  + Asia Pacific \(Hong Kong\)
  + Asia Pacific \(Mumbai\)
  + Asia Pacific \(Seoul\)
  + Asia Pacific \(Singapore\)
  + Asia Pacific \(Sydney\)
  + Asia Pacific \(Tokyo\)
  + Canada \(Central\)
  + China \(Beijing\)
  + China \(Ningxia\)
  + Europe \(Frankfurt\)
  + Europe \(Ireland\)
  + Europe \(London\)
  + Europe \(Paris\)
  + Europe \(Stockholm\)
  + Middle East \(Bahrain\)
  + South America \(São Paulo\)

For the compute type `BUILD_GENERAL1_2XLARGE`, Docker images up to 100 GB uncompressed are supported\.

**Note**  
For custom build environment images, CodeBuild supports Docker images up to 50 GB uncompressed in Linux and Windows, regardless of the compute type\. To check your build image's size, use Docker to run the `docker images REPOSITORY:TAG` command\.

You can use Amazon EFS to access more space in your build container\. For more information, see [Amazon Elastic File System sample for AWS CodeBuild](sample-efs.md)\. If you want to manipulate container disk space during a build, then the build must run in privileged mode\.

**Note**  
By default, Docker containers do not allow access to any devices\. Privileged mode grants a build project's Docker container access to all devices\. For more information, see [Runtime Privilege and Linux Capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) on the Docker Docs website\.