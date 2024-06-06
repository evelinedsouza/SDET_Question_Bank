**Explain runner file in cucumber**


In Cucumber, the runner file is used to configure and execute Cucumber tests. This file is essential for setting up the environment, specifying the location of feature files, and defining which glue code (step definitions and hooks) to use. It also allows for additional configurations such as tags, plugins, and other options that control the behavior of the test execution. Below are the key concepts related to the Cucumber runner file:

**1. Basic Structure of a Cucumber Runner File:**
   
In Java, the Cucumber runner file is typically a JUnit test class annotated with @RunWith and @CucumberOptions. Here is a basic example:

```
import org.junit.runner.RunWith;
import io.cucumber.junit.Cucumber;
import io.cucumber.junit.CucumberOptions;

@RunWith(Cucumber.class)
@CucumberOptions(
features = "src/test/resources/features", // Path to feature files
glue = {"stepdefinitions"},               // Package containing step definitions
tags = "@SmokeTest",                      // Tags to filter scenarios
plugin = {"pretty", "html:target/cucumber-reports"}, // Reporting plugins
monochrome = true                         // Makes the console output more readable
)
public class TestRunner {
}
```

**2. Key Annotations and Attributes:**
  
` @RunWith`
   
This annotation is used to specify that the class should use the Cucumber JUnit runner:


`@RunWith(Cucumber.class)
@CucumberOptions`

This annotation is used to configure various options for running Cucumber tests:

`features:` 
Specifies the path to the feature files. It can be a directory or a specific feature file.

`features = "src/test/resources/features"`


**glue:** Specifies the package(s) containing the step definitions and hooks.

`glue = {"stepdefinitions"}`


**tags:** 
Used to filter which scenarios to run based on tags.

`tags = "@SmokeTest"`

**plugin:** Specifies reporting and output options. Common plugins include:

**pretty:** Prints the Gherkin source with additional colors and stack traces for errors.

html:target/cucumber-reports: Generates HTML reports.
json:target/cucumber.json: Generates JSON reports.
junit:target/cucumber.xml: Generates JUnit XML reports.


`plugin = {"pretty", "html:target/cucumber-reports"}`

**monochrome:** Makes the console output more readable by removing ANSI colors.

`monochrome = true`

`strict:` Treats undefined and pending steps as errors.

`strict = true`

**dryRun:** Checks that every step in the feature file has a corresponding step definition without actually running the tests.

`dryRun = true
`

**3. Advanced Configuration**

   - Multiple Feature and Glue Paths
   - You can specify multiple paths for features and glue code:
   
```
@CucumberOptions(
features = {"src/test/resources/features", "src/test/resources/more_features"},
glue = {"stepdefinitions", "additional_stepdefinitions"}
)
```

**Specifying Tags for Selective Execution:**

You can use logical operators to run scenarios with specific tags:

- Run scenarios with either of the tags:

`tags = "@SmokeTest or @RegressionTest"`

- Run scenarios with both tags:

`tags = "@SmokeTest and @Critical"`

- Exclude scenarios with a specific tag:

`tags = "not @Ignore"`

**5. Running the Cucumber Tests**
   
- Using JUnit
  
You can run the Cucumber tests just like any other JUnit test in your IDE or using build tools like Maven or Gradle.

- Using Maven

You can configure Maven to run your Cucumber tests by adding the following plugin configuration in your pom.xml:


```
<build>
<plugins>
<plugin>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-surefire-plugin</artifactId>
<version>2.22.2</version>
<configuration>
<includes>
<include>**/TestRunner.java</include>
</includes>
</configuration>
</plugin>
</plugins>
</build>

```

**5. Generating Reports**
  
 Cucumber supports various reporting formats. By configuring the plugin option in the @CucumberOptions annotation, you can generate reports in different formats such as HTML, JSON, JUnit XML, etc. These reports help in understanding the test execution results and identifying any failures or issues.

Example for HTML and JSON Reports:
```
@CucumberOptions(
plugin = {"pretty", "html:target/cucumber-html-report", "json:target/cucumber.json"}
)
```

**Summary**

The Cucumber runner file is a crucial part of setting up and executing Cucumber tests. It allows you to configure paths to feature files and glue code, specify tags for selective execution, and define reporting options. Understanding these concepts and configurations helps in effectively managing and running your BDD tests using Cucumber.