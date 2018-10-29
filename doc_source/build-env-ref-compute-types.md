--------

A new console design is available for this service\. Although the procedures in this guide were written for the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\.

--------

# Build Environment Compute Types<a name="build-env-ref-compute-types"></a>

AWS CodeBuild provides build environments with the following available memory, vCPUs, and available disk space:


****  

| Compute type | computeType value | Memory | vCPUs | Disk space | 
| --- | --- | --- | --- | --- | 
| build\.general1\.small | BUILD\_GENERAL1\_SMALL | 3 GB | 2 | 64 GB | 
| build\.general1\.medium | BUILD\_GENERAL1\_MEDIUM | 7 GB | 4 | 128 GB | 
| build\.general1\.large | BUILD\_GENERAL1\_LARGE | 15 GB | 8 | 128 GB | 

**Note**  
For custom build environment images, AWS CodeBuild supports Docker images up to 10 GB uncompressed, regardless of the compute type\. To check your build image's size, use Docker to run the `docker images REPOSITORY:TAG` command\.

To choose one of these compute types:
+ In the AWS CodeBuild console, in the **Create Project** wizard or **Update project** page, expand **Show advanced settings**, and then choose one of the options from **Compute type**\. For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console) or [Change a Build Project's Settings \(Console\)](change-project.md#change-project-console)\.
+ In the AWS CodePipeline console, in the **Create Pipeline** wizard on the **Step 3: Build** page, or in the **Add action** or **Edit action** pane, choose **Create a new build project**, expand **Advanced**, and then choose one of the options in **Compute type**\. For more information, see [Create a Pipeline that Uses AWS CodeBuild \(AWS CodePipeline Console\)](how-to-create-pipeline.md#how-to-create-pipeline-console) or [Add an AWS CodeBuild Build Action to a Pipeline \(AWS CodePipeline Console\)](how-to-create-pipeline.md#how-to-create-pipeline-add)\.
+ For the AWS CLI, run the `create-project` or `update-project` command, specifying the `computeType` value of the `environment` object\. For more information, see [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli) or [Change a Build Project's Settings \(AWS CLI\)](change-project.md#change-project-cli)\.
+ For the AWS SDKs, call the equivalent of the `CreateProject` or `UpdateProject` operation for your target programming language, specifying the equivalent of `computeType` value of the `environment` object\. For more information, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.