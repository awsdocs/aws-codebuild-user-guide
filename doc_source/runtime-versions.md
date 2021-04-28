# Runtime versions<a name="runtime-versions"></a>

When you specify a runtime in the `runtime-versions` section of your buildspec file, you can specify a specific version, a specific major version and the latest minor version, or the latest version\. The following table lists the available runtimes and how to specify them\. 


**Ubuntu and Amazon Linux 2 platform runtime versions**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/runtime-versions.html)

**Note**  
The `aws/codebuild/amazonlinux2-aarch64-standard:1.0` image does not support the Android Runtime \(ART\)\.

You can use a build specification to install other components \(for example, the AWS CLI, Apache Maven, Apache Ant, Mocha, RSpec, or similar\) during the `install` build phase\. For more information, see [Buildspec example](build-spec-ref.md#build-spec-ref-example)\.