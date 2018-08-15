# Using Semantic Versioning to Name Build Artifacts Sample<a name="sample-buildspec-artifact-naming"></a>

 This sample contains example buildspec files that demonstrate how to specify an artifact name that is created at build time\. A name specified in a buildspec file can incorporate Shell commands and environment variables to make it unique\. A name you specify in a buildspec file overrides a name you enter in the console when you create your project\.

 If you build mulitple times, using an artifact name specified in the buildspec file can ensure your output artifact file names are unique\. For example, you can use a date and time stamp that is inserted into an artifact name at build time\. 

If you want to override the artifact name you entered in the console with a name in the buildspec file, do the following:

1.  Set your build project to override the artifact name with a name in the buildspec file\. 
   +  If you use the console to create your build project, select **Use the name specified in the buildspec file**\. For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console)\. 
   +  If you use the AWS CLI, set the `overrideArtifactName` to true in the JSON\-formatted file passed to `create-project`\. For more information, see [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli)\. 
   +  If use the AWS CodeBuild API, set the `overrideArtifactName` flag on the `ProjectArtifacts` object when a project is created or updated or a build is started\. 

1.  Specify a name in the buildspec file\. Use the following sample buildspec files as a guide\. 

 This Linux example shows you how to specify an artifact name that includes the date the build is created: 

```
version: 0.2         
phases:
  build:
    commands:
      - rspec HelloWorld_spec.rb
artifacts:
  files:
    - '**/*'
  name: myname-$(date +%Y-%m-%d)
```

 This Linux example shows you how to specify an artifact name that uses an AWS CodeBuild environment variable\. For more information, see [Environment Variables in Build Environments](build-env-ref-env-vars.md)\. 

```
version: 0.2         
phases:
  build:
    commands:
      - rspec HelloWorld_spec.rb
artifacts:
  files:
    - '**/*'
  name: myname-$(AWS_REGION)
```

 This Windows example shows you how to specify an artifact name that includes the date and time the build is created: 

```
version: 0.2
env:
  variables:
    TEST_ENV_VARIABLE: myArtifactName
phases:
  build:
    commands:
      - cd samples/helloworld
      - dotnet restore
      - dotnet run
artifacts:
  files:
    - '**/*
  name: $Env:TEST_ENV_VARIABLE-$(Get-Date -UFormat "%Y%m%d-%H%M%S")
```

 This Windows example shows you how to specify an artifact name that uses a variable declared in the buildspec file and an AWS CodeBuild environment variable\. For more information, see [Environment Variables in Build Environments](build-env-ref-env-vars.md)\. 

```
version: 0.2
env:
  variables:
    TEST_ENV_VARIABLE: myArtifactName
phases:
  build:
    commands:
      - cd samples/helloworld
      - dotnet restore
      - dotnet run
artifacts:
  files:
    - '**/*'
  name: $Env:TEST_ENV_VARIABLE-$Env:AWS_REGION
```

 For more information, see [Build Specification Reference for AWS CodeBuild](build-spec-ref.md)\. 