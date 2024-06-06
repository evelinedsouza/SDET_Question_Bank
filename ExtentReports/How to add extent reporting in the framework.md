**How to add extent reporting to your project**

Extent Reports is a popular reporting library for generating detailed and visually appealing reports in automated testing frameworks. Here’s how you can integrate Extent Reports with Cucumber to generate reports.

**Step-by-Step Guide to Generate Extent Reports with Cucumber**

**1. Add Dependencies**
   First, you need to add the necessary dependencies to your project. For a Maven project, you can include the following dependencies in your pom.xml:

```

<dependencies>
<!-- Cucumber dependencies -->
<dependency>
<groupId>io.cucumber</groupId>
<artifactId>cucumber-java</artifactId>
<version>7.9.0</version>
</dependency>
<dependency>
<groupId>io.cucumber</groupId>
<artifactId>cucumber-junit</artifactId>
<version>7.9.0</version>
<scope>test</scope>
</dependency>

    <!-- Extent Reports dependencies -->
    <dependency>
        <groupId>com.aventstack</groupId>
        <artifactId>extentreports</artifactId>
        <version>5.0.9</version>
    </dependency>
    
    <!-- Gson dependency -->
    <dependency>
        <groupId>com.google.code.gson</groupId>
        <artifactId>gson</artifactId>
        <version>2.8.8</version>
    </dependency>
</dependencies>

```

**2. Create an Extent Report Listener:**
Create a class that implements ConcurrentEventListener from Cucumber and configure Extent Reports within it.


```
import com.aventstack.extentreports.ExtentReports;
import com.aventstack.extentreports.ExtentTest;
import com.aventstack.extentreports.Status;
import com.aventstack.extentreports.reporter.ExtentHtmlReporter;
import com.aventstack.extentreports.reporter.configuration.Theme;
import io.cucumber.plugin.ConcurrentEventListener;
import io.cucumber.plugin.event.*;

public class ExtentCucumberAdapter implements ConcurrentEventListener {
    private static ExtentReports extent;
    private static ExtentTest feature;
    private static ExtentTest scenario;
    
    static {
        ExtentHtmlReporter htmlReporter = new ExtentHtmlReporter("target/extent-reports/report.html");
        htmlReporter.config().setTheme(Theme.DARK);
        htmlReporter.config().setDocumentTitle("Automation Test Report");
        htmlReporter.config().setReportName("Extent Report");

        extent = new ExtentReports();
        extent.attachReporter(htmlReporter);
        extent.setSystemInfo("Operating System", System.getProperty("os.name"));
        extent.setSystemInfo("Java Version", System.getProperty("java.version"));
    }

    @Override
    public void setEventPublisher(EventPublisher publisher) {
        publisher.registerHandlerFor(TestRunStarted.class, this::onTestRunStarted);
        publisher.registerHandlerFor(TestRunFinished.class, this::onTestRunFinished);
        publisher.registerHandlerFor(TestCaseStarted.class, this::onTestCaseStarted);
        publisher.registerHandlerFor(TestCaseFinished.class, this::onTestCaseFinished);
        publisher.registerHandlerFor(TestStepStarted.class, this::onTestStepStarted);
        publisher.registerHandlerFor(TestStepFinished.class, this::onTestStepFinished);
    }

    private void onTestRunStarted(TestRunStarted event) {
        // Initialize the Extent Reports
    }

    private void onTestRunFinished(TestRunFinished event) {
        extent.flush();
    }

    private void onTestCaseStarted(TestCaseStarted event) {
        scenario = extent.createTest(event.getTestCase().getName());
    }

    private void onTestCaseFinished(TestCaseFinished event) {
        if (event.getResult().getStatus().is(Status.PASSED)) {
            scenario.log(Status.PASS, "Scenario passed");
        } else if (event.getResult().getStatus().is(Status.FAILED)) {
            scenario.log(Status.FAIL, "Scenario failed");
        } else if (event.getResult().getStatus().is(Status.SKIPPED)) {
            scenario.log(Status.SKIP, "Scenario skipped");
        }
    }

    private void onTestStepStarted(TestStepStarted event) {
        // Optional: Handle step started events
    }

    private void onTestStepFinished(TestStepFinished event) {
        if (event.getResult().getStatus().is(Status.PASSED)) {
            scenario.log(Status.PASS, event.getTestStep().toString());
        } else if (event.getResult().getStatus().is(Status.FAILED)) {
            scenario.log(Status.FAIL, event.getTestStep().toString());
        } else if (event.getResult().getStatus().is(Status.SKIPPED)) {
            scenario.log(Status.SKIP, event.getTestStep().toString());
        }
    }
}
```


**3. Update Cucumber Options:**
   Ensure that your Cucumber runner file includes the custom Extent Reports listener.

```
import org.junit.runner.RunWith;
import io.cucumber.junit.Cucumber;
import io.cucumber.junit.CucumberOptions;

@RunWith(Cucumber.class)
@CucumberOptions(
features = "src/test/resources/features",
glue = {"stepdefinitions"},
plugin = {"pretty", "html:target/cucumber-reports", "json:target/cucumber.json", "path.to.your.ExtentCucumberAdapter"},
monochrome = true
)
public class TestRunner {
}

```

4. Running the Tests:
Run your tests using JUnit or Maven as you normally would. The Extent Reports will be generated in the specified directory (target/extent-reports/report.html in this example).

Summary:

By following these steps, you can integrate Extent Reports with Cucumber to generate detailed and visually appealing test reports. This setup enhances the visibility of test results and provides better insights into the execution of your automated tests.