# Working with Builds in CodeBuild<a name="builds-working"></a>

A *build* represents a set of actions performed by AWS CodeBuild to create output artifacts \(for example, a JAR file\) based on a set of input artifacts \(for example, a collection of Java class files\)\.

The following rules apply when you run multiple builds:
+ When possible, builds run concurrently\. The maximum number of concurrently running builds can vary\. For more information, see [Build Limits](limits.md#limits-builds)\. 
+  Builds are queued if the number of concurrently running builds reaches its limit\. The maximum number of builds in a queue is five times the concurrent build limit\. For more information, see [Build Limits](limits.md#limits-builds)\.
+ A build in a queue that does not start after the number of minutes specified in its time out value is removed from the queue\. The default timeout value is eight hours\. You can override the build queue timeout with a value between five minutes and eight hours when you run your build\. For more information, see [Run a Build in CodeBuild](run-build.md)\.
+ It is not possible to predict the order in which queued builds start\. 

**Note**  
You can access the history of a build for one year\.

You can perform these tasks when working with builds:

**Topics**
+ [Run a Build in CodeBuild](run-build.md)
+ [View Build Details in CodeBuild](view-build-details.md)
+ [View a List of Build IDs in CodeBuild](view-build-list.md)
+ [View a List of Build IDs for a Build Project in CodeBuild](view-builds-for-project.md)
+ [Stop a Build in CodeBuild](stop-build.md)
+ [Delete Builds in CodeBuild](delete-builds.md)