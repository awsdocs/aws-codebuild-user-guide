# Build environment compute types<a name="build-env-ref-compute-types"></a>

AWS CodeBuild provides build environments with the following available memory, vCPUs, and disk space:


**Operating system: Linux**  

| Compute type | computeType value | Memory | vCPUs | Disk space | Environment type | 
| --- | --- | --- | --- | --- | --- | 
| build\.general1\.small | BUILD\_GENERAL1\_SMALL | 3 GB | 2 | 64 GB | LINUX\_CONTAINER | 
| build\.general1\.medium | BUILD\_GENERAL1\_MEDIUM | 7 GB | 4 | 128 GB | LINUX\_CONTAINER | 
| build\.general1\.large | BUILD\_GENERAL1\_LARGE | 15 GB | 8 | 128 GB | LINUX\_CONTAINER | 
| build\.general1\.large | BUILD\_GENERAL1\_LARGE | 255 GB | 32 | 50 GB | LINUX\_GPU\_CONTAINER | 
| build\.general1\.large | BUILD\_GENERAL1\_LARGE | 16 GB | 8 | 50 GB | ARM\_CONTAINER | 
| build\.general1\.2xlarge | BUILD\_GENERAL1\_2XLARGE | 145 GB | 72 | 824 GB \(SSD\) | LINUX\_CONTAINER | 

The disk space listed for each build environment is available only in the directory specified by the `CODEBUILD_SRC_DIR` environment variable\.

**Note**  
 Some environment and compute types have limitations:   
The environment type `LINUX_GPU_CONTAINER` is available only in Regions US East \(N\. Virginia\), US West \(Oregon\), Canada \(Central\), Europe \(Ireland\), Europe \(London\), Europe \(Frankfurt\), Asia Pacific \(Tokyo\), Asia Pacific \(Seoul\), Asia Pacific \(Singapore\), Asia Pacific \(Sydney\), China \(Beijing\), and China \(Ningxia\)\.
The environment type `ARM_CONTAINER` is available only in Regions US East \(N\. Virginia\), US East \(Ohio\), US West \(Oregon\), Europe \(Ireland\), Asia Pacific \(Mumbai\), Asia Pacific \(Tokyo\), Asia Pacific \(Sydney\), and Europe \(Frankfurt\)\.
The compute type `build.general1.2xlarge` is available only in Regions US East \(N\. Virginia\), US East \(Ohio\), US West \(N\. California\), US West \(Oregon\), Canada \(Central\), South America \(São Paulo\), Europe \(Stockholm\), Europe \(Ireland\), Europe \(London\), Europe \(Paris\), Europe \(Frankfurt\), Middle East \(Bahrain\), Asia Pacific \(Hong Kong\), Asia Pacific \(Tokyo\), Asia Pacific \(Seoul\), Asia Pacific \(Singapore\), Asia Pacific \(Sydney\), Asia Pacific \(Mumbai\), China \(Beijing\), and China \(Ningxia\)\.
For the compute type `build.general1.2xlarge`, Docker images up to 100 GB uncompressed are supported\.


**Operating system: Windows**  

| Compute type | computeType value | Memory | vCPUs | Disk space | Environment type | 
| --- | --- | --- | --- | --- | --- | 
| build\.general1\.medium | BUILD\_GENERAL1\_MEDIUM | 7 GB | 4 | 128 GB |  WINDOWS\_CONTAINER WINDOWS\_SERVER\_2019\_CONTAINER  | 
| build\.general1\.large | BUILD\_GENERAL1\_LARGE | 15 GB | 8 | 128 GB |  WINDOWS\_CONTAINER WINDOWS\_SERVER\_2019\_CONTAINER  | 

**Note**  
For custom build environment images, CodeBuild supports Docker images up to 50 GB uncompressed in Linux and Windows, regardless of the compute type\. To check your build image's size, use Docker to run the `docker images REPOSITORY:TAG` command\.

To choose a compute type:
+ In the CodeBuild console, in the **Create build project** wizard or **Edit Build Project** page, in **Environment** expand **Additional configuration**, and then choose one of the options from **Compute type**\. For more information, see [Create a build project \(console\)](create-project-console.md) or [Change a build project's settings \(console\)](change-project-console.md)\.
+ For the AWS CLI, run the `create-project` or `update-project` command, specifying the `computeType` value of the `environment` object\. For more information, see [Create a build project \(AWS CLI\)](create-project-cli.md) or [Change a build project's settings \(AWS CLI\)](change-project-cli.md)\.
+ For the AWS SDKs, call the equivalent of the `CreateProject` or `UpdateProject` operation for your target programming language, specifying the equivalent of `computeType` value of the `environment` object\. For more information, see the [AWS SDKs and tools reference](sdk-ref.md)\.

You can use Amazon EFS to access more space in your build container\. For more information, see [Amazon Elastic File System sample for AWS CodeBuild](sample-efs.md)\. If you want to manipulate container disk space during a build, then the build must run in privileged mode\.

**Note**  
By default, Docker containers do not allow access to any devices\. Privileged mode grants a build project's Docker container access to all devices\. For more information, see [Runtime Privilege and Linux Capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) on the Docker Docs website\.