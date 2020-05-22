# Specify test commands<a name="report-group-test-case-commands"></a>

 You specify the commands that run your test cases in the `commands` section of your buildspec file\. These commands run the test cases specified for your report groups in the `reports` section of your buildspec file\. The following is a sample `commands` section that includes commands to run the tests in test files: 

```
commands:
    - echo Running tests for surefire junit
    - mvn test -f surefire/pom.xml -fn
    - echo
    - echo Running tests for cucumber with json plugin
    - mvn test -Dcucumber.options="--plugin json:target/cucumber-json-report.json" -f cucumber-json/pom.xml -fn
```