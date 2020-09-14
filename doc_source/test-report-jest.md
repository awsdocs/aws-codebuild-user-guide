# Set up test reporting with Jest<a name="test-report-jest"></a>

The following procedure demonstrates how to set up test reporting in AWS CodeBuild with the [Jest testing framework](https://jestjs.io/)\. 

The procedure requires the following prerequisites:
+ You have an existing AWS CodeBuild project\.
+ Your project is a Node\.js project that is set up to use the Jest testing framework\.

Add the [https://www.npmjs.com/package/jest-junit](https://www.npmjs.com/package/jest-junit) package to the `devDependencies` section of your project's `package.json` file\. AWS CodeBuild uses this package to generate reports in the `JunitXml` format\.

```
npm install --save-dev jest-junit
```

If it's not already present, add the `test` script to your project's `package.json` file\. The `test` script ensures that Jest is called when npm test is executed\.

```
{
  "scripts": {
    "test": "jest"
  }
}
```

Configure Jest to use the `JunitXml` reporter by adding the following to your Jest configuration file\. If your project does not have a Jest configuration file, create a file named `jest.config.js` in the root of your project and add the following\. The test reports are exported to the file specified by *<test report directory>*/*<report filename>*\.

```
module.exports = {
  reporters: [
    'default',
    [ 'jest-junit', {
      outputDirectory: <test report directory>,
      outputName: <report filename>,
    } ]
  ]
};
```

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
  jest_reports:
    files:
      - <report filename>
    file-format: JUNITXML
    base-directory: <test report directory>
```