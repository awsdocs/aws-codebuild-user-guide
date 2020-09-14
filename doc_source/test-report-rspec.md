# Set up test reporting with RSpec<a name="test-report-rspec"></a>

The following procedure demonstrates how to set up test reporting in AWS CodeBuild with the [RSpec testing framework](https://rspec.info/)\. 

The procedure requires the following prerequisites:
+ You have an existing AWS CodeBuild project\.
+ Your project is a Ruby project that is set up to use the RSpec testing framework\.

Add/update the following in your `buildspec.yml` file\. This code runs the tests in the *<test source directory>* directory and exports the test reports to the file specified by *<test report directory>*/*<report filename>*\. The report uses the `JunitXml` format\.

```
version: 0.2

phases:
  install:
    runtime-versions:
      ruby: 2.6
  pre_build:
    commands:
      - gem install rspec
      - gem install rspec_junit_formatter
  build:
    commands:
      - rspec <test source directory>/* --format RspecJunitFormatter --out <test report directory>/<report filename>
reports:
    rspec_reports:
        files:
            - <report filename>
        base-directory: <test report directory>
        file-format: JUNITXML
```