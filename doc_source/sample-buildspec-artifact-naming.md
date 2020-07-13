# Use semantic versioning to name build artifacts sample<a name="sample-buildspec-artifact-naming"></a>

 This sample contains example buildspec files that demonstrate how to specify an artifact name that is created at build time\. A name specified in a buildspec file can incorporate Shell commands and environment variables to make it unique\. A name you specify in a buildspec file overrides a name you enter in the console when you create your project\.

 If you build multiple times, using an artifact name specified in the buildspec file can ensure your output artifact file names are unique\. For example, you can use a date and timestamp that is inserted into an artifact name at build time\. 

If you want to override the artifact name you entered in the console with a name in the buildspec file, do the following:

1.  Set your build project to override the artifact name with a name in the buildspec file\. 
   +  If you use the console to create your build project, select **Enable semantic versioning**\. For more information, see [Create a build project \(console\)](create-project-console.md)\. 
   +  If you use the AWS CLI, set the `overrideArtifactName` to true in the JSON\-formatted file passed to `create-project`\. For more information, see [Create a build project \(AWS CLI\)](create-project-cli.md)\. 
   +  If you use the AWS CodeBuild API, set the `overrideArtifactName` flag on the `ProjectArtifacts` object when a project is created or updated or a build is started\. 

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

 This Linux example shows you how to specify an artifact name that uses a CodeBuild environment variable\. For more information, see [Environment variables in build environments](build-env-ref-env-vars.md)\. 

```
version: 0.2         
phases:
  build:
    commands:
      - rspec HelloWorld_spec.rb
artifacts:
  files:
    - '**/*'
  name: myname-$AWS_REGION
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
    - '**/*'
  name: $Env:TEST_ENV_VARIABLE-$(Get-Date -UFormat "%Y%m%d-%H%M%S")
```

 This Windows example shows you how to specify an artifact name that uses a variable declared in the buildspec file and a CodeBuild environment variable\. For more information, see [Environment variables in build environments](build-env-ref-env-vars.md)\. 

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

 For more information, see [Build specification reference for CodeBuild](build-spec-ref.md)\. 