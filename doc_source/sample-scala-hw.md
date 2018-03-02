# Scala Hello World Sample for AWS CodeBuild<a name="sample-scala-hw"></a>

This Scala sample produces as build output a single JAR file named `core_2.11-1.0.0.jar`\.

**Important**  
Running this sample may result in charges to your AWS account\. These include possible charges for AWS CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, and CloudWatch Logs\. For more information, see [AWS CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), and [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.


+ [Running the Sample](#sample-scala-hw-running)
+ [Directory Structure](#sample-scala-hw-dir)
+ [Files](#sample-scala-hw-files)
+ [Related Resources](#w3ab1b9c50c37c15)

## Running the Sample<a name="sample-scala-hw-running"></a>

To run this sample:

1. Identify a Docker image that contains sbt, a build tool for Scala projects\. To find a compatible Docker image, search [Docker Hub](https://hub.docker.com) for **sbt**\.

1. Create the directory structure and files as described in the Directory Structure and Files sections of this topic, and then upload them to an Amazon S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\. 
**Important**  
Do not upload `(root directory name)`, just the directories and files inside of `(root directory name)`\.   
If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the directory structure and files, and then upload it to the input bucket\. Do not add `(root directory name)` to the ZIP file, just the directories and files inside of `(root directory name)`\.

1. Create a build project, run the build, and view related build information by following the steps in [Run AWS CodeBuild Directly](how-to-run.md)\.

   If you use the AWS CLI to create the build project, the JSON\-formatted input to the `create-project` command might look similar to this\. \(Replace the placeholders with your own values\.\)

   ```
   {
     "name": "sample-scala-project",
     "source": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-input-bucket/ScalaSample.zip"
     },
     "artifacts": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-output-bucket",
       "packaging": "ZIP",
       "name": "ScalaOutputArtifact.zip"
     },
     "environment": {
       "type": "LINUX_CONTAINER",
       "image": "scala-image-ID",
       "computeType": "BUILD_GENERAL1_SMALL"
     },
     "serviceRole": "arn:aws:iam::account-ID:role/role-name",
     "encryptionKey": "arn:aws:kms:region-ID:account-ID:key/key-ID"
   }
   ```

1. To get the build output artifact, open your Amazon S3 output bucket\.

1. Download the `ScalaOutputArtifact.zip` file to your local computer or instance, and then extract the contents of the file\. In the extracted contents, open the `core/target/scala-2.11` folder to get the `core_2.11-1.0.0.jar` file\. 

## Directory Structure<a name="sample-scala-hw-dir"></a>

This sample assumes this directory structure\.

```
(root directory name)
    |-- buildspec.yml
    |-- core
    |     `-- src
    |           `-- main
    |                 `-- scala
    |                       `-- Test.scala
    |-- macros
    |     `-- src
    |           `-- main
    |                 `-- scala
    |                       `-- Macros.scala
    `-- project
          |-- Build.scala
          `-- build.properties
```

## Files<a name="sample-scala-hw-files"></a>

This sample uses these files\.

`buildspec.yml` \(in `(root directory name)`\)

```
version: 0.2

phases:
  build:
    commands:
      - echo Build started on `date`
      - echo Run the test and package the code...
      - sbt run       
  post_build:
    commands:
      - echo Build completed on `date`
      - sbt package
artifacts:
  files:
    - core/target/scala-2.11/core_2.11-1.0.0.jar
```

`Test.scala` \(in `(root directory name)/core/src/main/scala`\)

```
object Test extends App {
  Macros.hello
}
```

`Macros.scala` \(in `(root directory name)/macros/src/main/scala`\)

```
import scala.language.experimental.macros
import scala.reflect.macros.Context

object Macros {
  def impl(c: Context) = {
    import c.universe._
    c.Expr[Unit](q"""println("Hello World")""")
  }

  def hello: Unit = macro impl
}
```

`Build.scala` \(in `(root directory name)/project`\)

```
import sbt._
import Keys._

object BuildSettings {
  val buildSettings = Defaults.defaultSettings ++ Seq(
    organization := "org.scalamacros",
    version := "1.0.0",
    scalaVersion := "2.11.8",
    crossScalaVersions := Seq("2.10.2", "2.10.3", "2.10.4", "2.10.5", "2.10.6", "2.11.0", "2.11.1", "2.11.2", "2.11.3", "2.11.4", "2.11.5", "2.11.6", "2.11.7", "2.11.8"),
    resolvers += Resolver.sonatypeRepo("snapshots"),
    resolvers += Resolver.sonatypeRepo("releases"),
    scalacOptions ++= Seq()
  )
}

object MyBuild extends Build {
  import BuildSettings._

  lazy val root: Project = Project(
    "root",
    file("."),
    settings = buildSettings ++ Seq(
      run <<= run in Compile in core)
  ) aggregate(macros, core)

  lazy val macros: Project = Project(
    "macros",
    file("macros"),
    settings = buildSettings ++ Seq(
      libraryDependencies <+= (scalaVersion)("org.scala-lang" % "scala-reflect" % _),
      libraryDependencies := {
        CrossVersion.partialVersion(scalaVersion.value) match {
          // if Scala 2.11+ is used, quasiquotes are available in the standard distribution
          case Some((2, scalaMajor)) if scalaMajor >= 11 =>
            libraryDependencies.value
          // in Scala 2.10, quasiquotes are provided by macro paradise
          case Some((2, 10)) =>
            libraryDependencies.value ++ Seq(
              compilerPlugin("org.scalamacros" % "paradise" % "2.1.0-M5" cross CrossVersion.full),
              "org.scalamacros" %% "quasiquotes" % "2.1.0-M5" cross CrossVersion.binary)
        }
      }
    )
  )

  lazy val core: Project = Project(
    "core",
    file("core"),
    settings = buildSettings
  ) dependsOn(macros)
}
```

`build.properties` \(in `(root directory name)/project`\)

```
sbt.version=0.13.7
```

## Related Resources<a name="w3ab1b9c50c37c15"></a>

+ For more information about getting started with AWS CodeBuild, see [Getting Started with AWS CodeBuild](getting-started.md)\.

+ For more information about troubleshooting problems with AWS CodeBuild, see [Troubleshooting AWS CodeBuild](troubleshooting.md)\.

+ For more information about limits in AWS CodeBuild, see [Limits for AWS CodeBuild](limits.md)\.