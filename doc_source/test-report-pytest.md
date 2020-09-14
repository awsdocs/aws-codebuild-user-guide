# Set up test reporting with pytest<a name="test-report-pytest"></a>

The following procedure demonstrates how to set up test reporting in AWS CodeBuild with the [pytest testing framework](https://docs.pytest.org/)\. 

The procedure requires the following prerequisites:
+ You have an existing AWS CodeBuild project\.
+ Your project is a Python project that is set up to use the pytest testing framework\.

Add the following entry to either the `build` or `post_build` phase of your `buildspec.yml` file\. This code automatically discovers tests in the current directory and exports the test reports to the file specified by *<test report directory>*/*<report filename>*\. The report uses the `JunitXml` format\.

```
      - python -m pytest --junitxml=<test report directory>/<report filename>
```

In your `buildspec.yml` file, add/update the following sections\.

```
version: 0.2

phases:
  install:
    runtime-versions:
      python: 3.7
    commands:
      - pip3 install pytest
  build:
    commands:
      - python -m pytest --junitxml=<test report directory>/<report filename>

reports:
  pytest_reports:
    files:
      - <report filename>
    base-directory: <test report directory>
    file-format: JUNITXML
```