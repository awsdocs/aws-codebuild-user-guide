# Step 7: View summarized build information<a name="getting-started-monitor-build-console"></a>

\(Previous step: [Step 6: Run the build](getting-started-run-build-console.md)\)

In this step, you view summarized information about the status of your build\.

## To view summarized build information<a name="getting-started-monitor-build-console-title"></a><a name="getting-started-run-build-console-procedure"></a>

1. If the **codebuild\-demo\-project:*build\-ID*** page is not displayed, in the navigation bar, choose **Build history**\. Next, in the list of build projects, for **Project**, choose the **Build run** link for **codebuild\-demo\-project**\. There should be only one matching link\. \(If you have completed this tutorial before, choose the link with the most recent value in the **Completed** column\.\)

1. On the build details page, in **Phase details**, the following build phases should be displayed, with **Succeeded** in the **Status** column:
   + **SUBMITTED**
   + **QUEUED**
   + **PROVISIONING**
   + **DOWNLOAD\_SOURCE**
   + **INSTALL**
   + **PRE\_BUILD**
   + **BUILD**
   + **POST\_BUILD**
   + **UPLOAD\_ARTIFACTS**
   + **FINALIZING**
   + **COMPLETED**

   In **Build Status**, **Succeeded** should be displayed\.

   If you see **In Progress** instead, choose the refresh button\. 

1. Next to each build phase, the **Duration** value indicates how long the build phase lasted\. The **End time** value indicates when that build phase ended\.

## Next step<a name="getting-started-monitor-build-console-next"></a>

[Step 8: View detailed build information](getting-started-build-log-console.md)