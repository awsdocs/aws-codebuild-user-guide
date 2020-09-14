# Working with test reporting in AWS CodeBuild<a name="test-reporting"></a>

You can create reports in CodeBuild that contain details about tests that are run during builds\. You can create tests such as unit tests, configuration tests, and functional tests\. 

The following test report file formats are supported:
+ Cucumber JSON
+ JUnit XML
+ NUnit XML
+ NUnit3 XML
+ TestNG XML
+ Visual Studio TRX

Create your test cases with any test framework that can create report files in one of these formats \(for example, Surefire JUnit plugin, TestNG, or Cucumber\)\.

To create a test report, you add a report group name to the buildspec file of a build project with information about your test cases\. When you run the build project, the test cases are run and a test report is created\. You do not need to create a report group before you run your tests\. If you specify a report group name, CodeBuild creates a report group for you when you run your reports\. If you want to use a report group that already exists, you specify its ARN in the buildspec file\.

You can use a test report to help troubleshoot a problem during a build run\. If you have many test reports from multiple builds of a build project, you can use your test reports to view trends and test and failure rates to help you optimize builds\. 

A report expires 30 days after it was created\. You cannot view an expired test report\. If you want to keep test reports for more than 30 days, you can export your test results' raw data files to an Amazon S3 bucket\. Exported test files do not expire\. Information about the S3 bucket is specified when you create the report group\.

**Note**  
The CodeBuild service role specified in the project is used for permissions to upload to the S3 bucket\.

**Topics**
+ [Create a test report](report-create.md)
+ [Working with report groups](test-report-group.md)
+ [Working with reports](test-report.md)
+ [Working with test report permissions](test-permissions.md)
+ [View test reports](test-view-reports.md)
+ [Test reporting with test frameworks](test-framework-reporting.md)
+ [Code coverage reports](code-coverage-report.md)