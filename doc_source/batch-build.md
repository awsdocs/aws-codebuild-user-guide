# Batch builds in AWS CodeBuild<a name="batch-build"></a>

AWS CodeBuild supports the execution of concurrent and coordinated builds of a project with *batch builds*\. For more information, see the following topics:
+ [Batch build buildspec reference](batch-build-buildspec.md)
+ [Batch configuration](create-project-console.md#create-project-console-batch-config)
+ [Run a batch build \(AWS CLI\)](run-batch-build-cli.md)
+ [Stop a batch build in AWS CodeBuild ](stop-batch-build.md)

Batch builds introduce a new security role in the batch configuration\. This new role is required as CodeBuild must be able to call the `StartBuild`, `StopBuild`, and `RetryBuild` actions on your behalf to run builds as part of a batch\. Customers should use a new role, and not the same role they use in their build, for two reasons:
+ Giving the build role `StartBuild`, `StopBuild`, and `RetryBuild` permissions would allow a single build to start more builds via the buildspec\.
+ CodeBuild batch builds provide restrictions that restrict the number of builds and compute types that can be used for the builds in the batch\. If the build role has these permissions, it is possible the builds themselves could bypass these restrictions\.