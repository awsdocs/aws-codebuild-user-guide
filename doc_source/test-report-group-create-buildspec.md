# Create a report group \(buildspec\)<a name="test-report-group-create-buildspec"></a>

A report group created using the buildspec does not export raw test result files\. You can view your report group and specify export settings\. For more information, see [Update a report group](report-group-export-settings.md)\. 

**To create a report group using a buildspec file**

1.  Choose a report group name that is not associated with a report group in your AWS account\. 

1.  Configure the `reports` section of the buildspec file with this name\. In this example, the report group name is `new-report-group` and the use test cases are created with the JUnit framework: 

   ```
   reports:
    new-report-group: #surefire junit reports
      files:
        - '**/*'
      base-directory: 'surefire/target/surefire-reports'
   ```

    For more information, see [Specify test files](report-group-test-cases.md) and [Reports syntax in the buildspec file](build-spec-ref.md#reports-buildspec-file)\. 

1. In the `commands` section, specify the command to run your tests\. For more information, see [ Specify test commands ](report-group-test-case-commands.md)\. 

1.  Run the build\. When the build is complete, a new report group is created with a name that uses the format `project-name-report-group-name`\. For more information, see [Report group naming](test-report-group-naming.md)\. 