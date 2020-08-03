# Working with builds in AWS CodeBuild<a name="builds-working"></a>

A *build* represents a set of actions performed by AWS CodeBuild to create output artifacts \(for example, a JAR file\) based on a set of input artifacts \(for example, a collection of Java class files\)\.

The following rules apply when you run multiple builds:
+ When possible, builds run concurrently\. The maximum number of concurrently running builds can vary\. For more information, see [Builds](limits.md#limits-builds)\. 
+  Builds are queued if the number of concurrently running builds reaches its limit\. The maximum number of builds in a queue is five times the concurrent build limit\. For more information, see [Builds](limits.md#limits-builds)\.
+ A build in a queue that does not start after the number of minutes specified in its time out value is removed from the queue\. The default timeout value is eight hours\. You can override the build queue timeout with a value between five minutes and eight hours when you run your build\. For more information, see [Run a build in AWS CodeBuild](run-build.md)\.
+ It is not possible to predict the order in which queued builds start\. 

**Note**  
You can access the history of a build for one year\.

You can perform these tasks when working with builds:

**Topics**
+ [Run a build in AWS CodeBuild](run-build.md)
+ [View build details in AWS CodeBuild](view-build-details.md)
+ [View a list of build IDs in AWS CodeBuild](view-build-list.md)
+ [View a list of build IDs for a build project in AWS CodeBuild](view-builds-for-project.md)
+ [Stop a build in AWS CodeBuild](stop-build.md)
+ [Stop a batch build in AWS CodeBuild](stop-batch-build.md)
+ [Retry a build in AWS CodeBuild](retry-build.md)
+ [View a running build in Session Manager](session-manager.md)
+ [Delete builds in AWS CodeBuild](delete-builds.md)