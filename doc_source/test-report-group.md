# Working with Report Groups<a name="test-report-group"></a>


|  | 
| --- |
| The test reporting feature is in preview release for CodeBuild and is subject to change\. | 

A *report group* contains test reports and specifies shared settings\. You use the buildspec file to specify the test cases to run and the commands to run them when it builds\. For each report group configured in a build project, a run of the build project creates a test report\. Multiple runs of a build project configured with a report group create multiple test reports in that report group, each with results of the the same test cases specified for that report group\. 

 The test cases are specified for a report group in the buildspec file of a build project\. You can specify up to 5 report groups in one build project\. When your run a build, all the test cases run\. A new test report is created with the results of each test case specified for a report group\. Each time you run a new build, the test cases run and a new test report is created with the new test results\. 

 Report groups can be used in more than one build project\. All test reports created with one report group share the same configuration, such as its export option and permissions, even if the test reports are created using different build projects\. Test reports created with one report group in multiple build projects can contain the results from running different sets of test cases \(one set of test cases for each build project\)\. This is because you can specify different test case files for the report group in each project's buildspec file\. You can also change the test case files for a report group in a build project by editing its buildspec file\. Subsequent build runs create new test reports that contain the results of the test case files in the updated buildspec\. 

**Topics**
+ [Create a Report Group](report-group-create.md)
+ [Update a Report Group](report-group-export-settings.md)
+ [Specify Test Files](report-group-test-cases.md)
+ [Specify Test Commands](report-group-test-case-commands.md)
+ [Report Group Naming](test-report-group-naming.md)
+ [Working with Shared Report Groups](report-groups-sharing.md)