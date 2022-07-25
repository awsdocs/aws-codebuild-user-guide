# Available runtimes<a name="available-runtimes"></a>

You can specify one or more runtimes in the `runtime-versions` section of your buildspec file\. If your runtime is dependent upon another runtime, you can also specify its dependent runtime in the buildspec file\. If you do not specify any runtimes in the buildspec file, CodeBuild chooses the default runtimes that are available in the image you use\. If you specify one or more runtimes, CodeBuild uses only those runtimes\. If a dependent runtime is not specified, CodeBuild attempts to choose the dependent runtime for you\. For more information, see [Specify runtime versions in the buildspec file](build-spec-ref.md#runtime-versions-buildspec-file)\.

**Topics**
+ [Linux image runtimes](#linux-runtimes)
+ [Windows image runtimes](#windows-runtimes)

## Linux image runtimes<a name="linux-runtimes"></a>

The following table contains the available runtimes and the standard Linux images that support them\. 


**Ubuntu and Amazon Linux 2 platform runtimes**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/available-runtimes.html)

## Windows image runtimes<a name="windows-runtimes"></a>

The base image of the Windows Server Core 2019 contains the following runtimes\.


**Windows platform runtimes**  

| Runtime name | Versions available in `windows-base:2019-1.0` | Versions available in `windows-base:2019-2.0` | 
| --- | --- | --- | 
| dotnet | 3\.1\.4045\.0 | 3\.1\.4196\.0\.300 | 
| golang | 1\.14 | 1\.18\.2 | 
| nodejs | 12\.18 | 16\.15\.0 | 
| java | corretto11 | corretto11corretto17 | 
| php | 7\.4\.7 | 8\.1\.6 | 
| powershell | 7\.0\.2 | 7\.2\.4 | 
| python | 3\.8\.3 | 3\.10\.4 | 
| ruby | 2\.7 | 3\.1\.1\.1 | 