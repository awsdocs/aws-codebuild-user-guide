# Set up test reporting with Jasmine<a name="test-report-jasmine"></a>

The following procedure demonstrates how to set up test reporting in AWS CodeBuild with the [JasmineBDD testing framework](http://jasmine.github.io/)\. 

The procedure requires the following prerequisites:
+ You have an existing AWS CodeBuild project\.
+ Your project is a Node\.js project that is set up to use the Jasmine testing framework\.

Add the [https://www.npmjs.com/package/jasmine-reporters](https://www.npmjs.com/package/jasmine-reporters) package to the `devDependencies` section of your project's `package.json` file\. This package has a collection of JavaScript reporter classes that can be used with Jasmine\. 

```
npm install --save-dev jasmine-reporters
```

If it's not already present, add the `test` script to your project's `package.json` file\. The `test` script ensures that Jasmine is called when npm test is executed\.

```
{
  "scripts": {
    "test": "npx jasmine"
  }
}
```

AWS CodeBuild supports the following Jasmine test reporters:

JUnitXmlReporter  
Used to generate reports in the `JunitXml` format\.

NUnitXmlReporter  
Used to generate reports in the `NunitXml` format\.

A Node\.js project with Jasmine will, by default, have a `spec` sub\-directory, which contains the Jasmine configuration and test scripts\. 

To configure Jasmine to generate reports in the `JunitXML` format, instantiate the `JUnitXmlReporter` reporter by adding the following code to your tests\. 

```
var reporters = require('jasmine-reporters');

var junitReporter = new reporters.JUnitXmlReporter({
  savePath: <test report directory>,
  filePrefix: <report filename>,
  consolidateAll: true
});

jasmine.getEnv().addReporter(junitReporter);
```

To configure Jasmine to generate reports in the `NunitXML` format, instantiate the `NUnitXmlReporter` reporter by adding the following code to your tests\. 

```
var reporters = require('jasmine-reporters');

var nunitReporter = new reporters.NUnitXmlReporter({
  savePath: <test report directory>,
  filePrefix: <report filename>,
  consolidateAll: true
});

jasmine.getEnv().addReporter(nunitReporter)
```

The test reports are exported to the file specified by *<test report directory>*/*<report filename>*\.

In your `buildspec.yml` file, add/update the following sections\.

```
version: 0.2

phases:
  pre_build:
    commands:
      - npm install
  build:
    commands:
      - npm build
      - npm test

reports:
  jasmine_reports:
    files:
      - <report filename>
    file-format: JUNITXML
    base-directory: <test report directory>
```

If you are using the the `NunitXml` report format, change the `file-format` value to the following\.

```
    file-format: NUNITXML
```