# Report group naming<a name="test-report-group-naming"></a>

 When you use the AWS CLI or the AWS CodeBuild console to create a report group, you specify a name for the report group\. If you use the buildspec to create a new report group, it is named using the format `project-name-report-group-name-specified-in-buildspec`\. All reports created by running builds of that build project belong to the new report group that has the new name\. 

 If you do not want CodeBuild to create a new report group, specify the ARN of the report group in a build project's buildspec file\. You can specify a report group's ARN in multiple build projects\. After each build project runs, the report group contains test reports created by each build project\. 

 For example, if you create one report group with the name `my-report-group`, and then use its name in two different build projects named `my-project-1` and `my-project-2` and create a build of both projects, two new report groups are created\. The result is three report groups with the following names: 
+  `my-report-group`: Does not have any test reports\. 
+  `my-project-1-my-report-group`: Contains reports with results of tests run by the build project named `my-project-1`\. 
+  `my-project-2-my-report-group`: Contains reports with results of tests run by the build project named `my-project-2`\. 

 If you use the ARN of the report group named `my-report-group` in both projects, and then run builds of each project, you still have one report group \(`my-report-group`\)\. That report group contains test reports with results of tests run by both build projects\. 

 If you choose a report group name that doesn't belong to a report group in your AWS account, and then use that name for a report group in a buildspec file and run a build of its build project, a new report group is created\. The format of name of the new report group is `project-name-new-group-name`\. For example, if there is not a report group in your AWS account with the name `new-report-group`, and specify it in a build project called `test-project`, a build run creates a new report group with the name `test-project-new-report-group`\. 