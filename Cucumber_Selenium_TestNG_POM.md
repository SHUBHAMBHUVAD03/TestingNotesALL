# 🥒 Cucumber with Java, Selenium, TestNG & Page Object Model (POM)
## Complete Detailed Guide

---

## 📌 Table of Contents

1. [What is Cucumber?](#1-what-is-cucumber)
2. [What is BDD?](#2-what-is-bdd-behavior-driven-development)
3. [Cucumber Architecture](#3-cucumber-architecture)
4. [Tools & Technologies Stack](#4-tools--technologies-stack)
5. [Project Setup & Maven POM.xml](#5-project-setup--maven-pomxml)
6. [Project Folder Structure](#6-project-folder-structure)
7. [Page Object Model (POM)](#7-page-object-model-pom)
8. [Writing Feature Files (Gherkin)](#8-writing-feature-files-gherkin)
9. [Step Definitions](#9-step-definitions)
10. [Hooks (Before & After)](#10-hooks-before--after)
11. [TestNG Runner with Cucumber](#11-testng-runner-with-cucumber)
12. [Data-Driven Testing](#12-data-driven-testing)
13. [Tags in Cucumber](#13-tags-in-cucumber)
14. [Reporting](#14-reporting)
15. [Complete End-to-End Example](#15-complete-end-to-end-example)
16. [Common Annotations & Gherkin Keywords](#16-common-annotations--gherkin-keywords)
17. [Best Practices](#17-best-practices)
18. [Common Interview Questions](#18-common-interview-questions)

---

## 1. What is Cucumber?

**Cucumber** is an open-source **BDD (Behavior Driven Development)** testing framework that allows you to write test cases in **plain English** using the **Gherkin** language. It bridges the gap between non-technical stakeholders and developers/testers.

### Key Points

| Feature | Details |
|---|---|
| **Language** | Java (also supports Ruby, JavaScript, Python) |
| **Test Language** | Gherkin (Given/When/Then) |
| **Integration** | Selenium WebDriver, TestNG, JUnit, Spring |
| **File Extension** | `.feature` (Gherkin), `.java` (Step Definitions) |
| **License** | MIT |
| **Official Site** | [cucumber.io](https://cucumber.io) |

### Why Cucumber?

- ✅ Tests written in plain English — understood by everyone
- ✅ Bridges gap between business and technical teams
- ✅ Living documentation — feature files serve as documentation
- ✅ Reusable step definitions
- ✅ Supports data-driven testing with `Scenario Outline`
- ✅ Integrates seamlessly with Selenium and TestNG

---

## 2. What is BDD (Behavior Driven Development)?

**BDD** is a software development approach where tests are written based on the **behavior** of the application from the **user's perspective**.

### BDD vs TDD vs ATDD

| Aspect | TDD | BDD | ATDD |
|---|---|---|---|
| **Focus** | Code units | Behavior | Acceptance criteria |
| **Written by** | Developers | Dev + QA + Business | All stakeholders |
| **Language** | Code | Gherkin (English) | English/Code |
| **Framework** | JUnit | Cucumber | FitNesse |

### BDD Workflow

```
Business Analyst writes User Stories
         │
         ▼
Team collaborates → Writes Gherkin Scenarios
         │
         ▼
Developer writes Step Definitions
         │
         ▼
Selenium automates browser actions
         │
         ▼
Tests run via TestNG Runner
         │
         ▼
Reports generated
```

---

## 3. Cucumber Architecture

```
┌──────────────────────────────────────────────┐
│              FEATURE FILE (.feature)          │
│  Given / When / Then scenarios in Gherkin    │
└───────────────────┬──────────────────────────┘
                    │
                    ▼
┌──────────────────────────────────────────────┐
│           STEP DEFINITIONS (.java)            │
│  Maps Gherkin steps to Java methods          │
└───────────────────┬──────────────────────────┘
                    │
                    ▼
┌──────────────────────────────────────────────┐
│        PAGE OBJECT MODEL (POM)               │
│  Page classes with locators & actions        │
└───────────────────┬──────────────────────────┘
                    │
                    ▼
┌──────────────────────────────────────────────┐
│        SELENIUM WEBDRIVER                    │
│  Automates browser interactions              │
└───────────────────┬──────────────────────────┘
                    │
                    ▼
┌──────────────────────────────────────────────┐
│         TESTNG RUNNER                        │
│  Triggers test execution                     │
└──────────────────────────────────────────────┘
```

---

## 4. Tools & Technologies Stack

| Tool/Technology | Purpose | Version (Recommended) |
|---|---|---|
| **Java** | Programming language | Java 11 or 17 |
| **Maven** | Build & dependency management | 3.8+ |
| **Cucumber** | BDD framework | 7.x |
| **Selenium WebDriver** | Browser automation | 4.x |
| **TestNG** | Test runner & assertions | 7.x |
| **WebDriverManager** | Auto-manage browser drivers | 5.x |
| **Extent Reports** | Rich HTML test reporting | 5.x |
| **Log4j** | Logging | 2.x |
| **Apache POI** | Read/write Excel files (Data-driven) | 5.x |

---

## 5. Project Setup & Maven POM.xml

### Complete `pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.automation</groupId>
    <artifactId>cucumber-selenium-testng</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <cucumber.version>7.14.0</cucumber.version>
        <selenium.version>4.15.0</selenium.version>
        <testng.version>7.8.0</testng.version>
        <webdrivermanager.version>5.6.3</webdrivermanager.version>
        <extent.version>5.1.1</extent.version>
    </properties>

    <dependencies>

        <!-- ✅ Cucumber Core -->
        <dependency>
            <groupId>io.cucumber</groupId>
            <artifactId>cucumber-java</artifactId>
            <version>${cucumber.version}</version>
        </dependency>

        <!-- ✅ Cucumber + TestNG Integration -->
        <dependency>
            <groupId>io.cucumber</groupId>
            <artifactId>cucumber-testng</artifactId>
            <version>${cucumber.version}</version>
        </dependency>

        <!-- ✅ Cucumber PicoContainer (Dependency Injection) -->
        <dependency>
            <groupId>io.cucumber</groupId>
            <artifactId>cucumber-picocontainer</artifactId>
            <version>${cucumber.version}</version>
        </dependency>

        <!-- ✅ Selenium WebDriver -->
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>${selenium.version}</version>
        </dependency>

        <!-- ✅ WebDriverManager (Auto-download browser drivers) -->
        <dependency>
            <groupId>io.github.bonigarcia</groupId>
            <artifactId>webdrivermanager</artifactId>
            <version>${webdrivermanager.version}</version>
        </dependency>

        <!-- ✅ TestNG -->
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>${testng.version}</version>
        </dependency>

        <!-- ✅ Extent Reports for HTML Reports -->
        <dependency>
            <groupId>com.aventstack</groupId>
            <artifactId>extentreports</artifactId>
            <version>${extent.version}</version>
        </dependency>

        <!-- ✅ tech.grasshopper for Extent Cucumber Adapter -->
        <dependency>
            <groupId>tech.grasshopper</groupId>
            <artifactId>extentreports-cucumber7-adapter</artifactId>
            <version>1.14.0</version>
        </dependency>

        <!-- ✅ Apache POI (Excel Data-Driven Testing) -->
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi-ooxml</artifactId>
            <version>5.2.3</version>
        </dependency>

        <!-- ✅ Log4j Logging -->
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.21.1</version>
        </dependency>

        <!-- ✅ AssertJ (Fluent assertions) -->
        <dependency>
            <groupId>org.assertj</groupId>
            <artifactId>assertj-core</artifactId>
            <version>3.24.2</version>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <!-- Maven Surefire Plugin to run TestNG -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.2.2</version>
                <configuration>
                    <suiteXmlFiles>
                        <suiteXmlFile>testng.xml</suiteXmlFile>
                    </suiteXmlFiles>
                </configuration>
            </plugin>

            <!-- Maven Compiler Plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.11.0</version>
                <configuration>
                    <source>17</source>
                    <target>17</target>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

---

## 6. Project Folder Structure

```
cucumber-selenium-testng/
│
├── src/
│   ├── main/
│   │   └── java/
│   │       └── com/automation/
│   │           └── utils/
│   │               ├── ConfigReader.java
│   │               └── ExcelUtils.java
│   │
│   └── test/
│       ├── java/
│       │   └── com/automation/
│       │       ├── pages/                   ← Page Object Model
│       │       │   ├── BasePage.java
│       │       │   ├── LoginPage.java
│       │       │   └── HomePage.java
│       │       │
│       │       ├── stepdefinitions/         ← Step Definitions
│       │       │   ├── LoginSteps.java
│       │       │   └── HomeSteps.java
│       │       │
│       │       ├── hooks/                   ← Hooks
│       │       │   └── Hooks.java
│       │       │
│       │       ├── runner/                  ← TestNG Runner
│       │       │   └── TestRunner.java
│       │       │
│       │       └── utils/
│       │           └── DriverManager.java
│       │
│       └── resources/
│           ├── features/                    ← Feature Files
│           │   ├── login.feature
│           │   └── checkout.feature
│           │
│           ├── config.properties
│           ├── extent.properties
│           └── testng.xml
│
├── test-output/                             ← TestNG Reports
├── reports/                                 ← Extent Reports
├── screenshots/                             ← Failure Screenshots
└── pom.xml
```

---

## 7. Page Object Model (POM)

POM is a design pattern that creates an **Object Repository** for web UI elements. Each web page is represented as a Java class.

### Benefits of POM

- ✅ Separation of concerns (UI locators vs test logic)
- ✅ Reduces code duplication
- ✅ Easy to maintain — change locator in one place
- ✅ Improves code readability

### `DriverManager.java` — WebDriver Singleton

```java
package com.automation.utils;

import io.github.bonigarcia.wdm.WebDriverManager;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.firefox.FirefoxDriver;

public class DriverManager {

    private static ThreadLocal<WebDriver> driver = new ThreadLocal<>();

    public static WebDriver getDriver() {
        return driver.get();
    }

    public static void initDriver(String browser) {
        WebDriver webDriver;

        switch (browser.toLowerCase()) {
            case "firefox":
                WebDriverManager.firefoxdriver().setup();
                webDriver = new FirefoxDriver();
                break;
            case "chrome":
            default:
                WebDriverManager.chromedriver().setup();
                ChromeOptions options = new ChromeOptions();
                options.addArguments("--start-maximized");
                options.addArguments("--disable-notifications");
                // options.addArguments("--headless"); // Uncomment for headless
                webDriver = new ChromeDriver(options);
                break;
        }

        driver.set(webDriver);
        getDriver().manage().window().maximize();
    }

    public static void quitDriver() {
        if (getDriver() != null) {
            getDriver().quit();
            driver.remove();
        }
    }
}
```

### `BasePage.java` — Parent Page with Reusable Methods

```java
package com.automation.pages;

import com.automation.utils.DriverManager;
import org.openqa.selenium.*;
import org.openqa.selenium.support.PageFactory;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.Select;
import org.openqa.selenium.support.ui.WebDriverWait;

import java.time.Duration;

public class BasePage {

    protected WebDriver driver;
    protected WebDriverWait wait;

    public BasePage() {
        this.driver = DriverManager.getDriver();
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(20));
        PageFactory.initElements(driver, this);
    }

    // ─── Navigation ───────────────────────────────────────────────────────────

    public void navigateTo(String url) {
        driver.get(url);
    }

    public String getPageTitle() {
        return driver.getTitle();
    }

    // ─── Element Interactions ─────────────────────────────────────────────────

    public void click(WebElement element) {
        wait.until(ExpectedConditions.elementToBeClickable(element)).click();
    }

    public void type(WebElement element, String text) {
        wait.until(ExpectedConditions.visibilityOf(element)).clear();
        element.sendKeys(text);
    }

    public String getText(WebElement element) {
        return wait.until(ExpectedConditions.visibilityOf(element)).getText();
    }

    public boolean isDisplayed(WebElement element) {
        try {
            return wait.until(ExpectedConditions.visibilityOf(element)).isDisplayed();
        } catch (TimeoutException e) {
            return false;
        }
    }

    // ─── Dropdown ─────────────────────────────────────────────────────────────

    public void selectByVisibleText(WebElement dropdown, String text) {
        new Select(dropdown).selectByVisibleText(text);
    }

    public void selectByValue(WebElement dropdown, String value) {
        new Select(dropdown).selectByValue(value);
    }

    // ─── Waits ────────────────────────────────────────────────────────────────

    public void waitForElement(WebElement element) {
        wait.until(ExpectedConditions.visibilityOf(element));
    }

    public void waitForUrl(String url) {
        wait.until(ExpectedConditions.urlContains(url));
    }

    // ─── Screenshot ───────────────────────────────────────────────────────────

    public byte[] takeScreenshot() {
        return ((TakesScreenshot) driver).getScreenshotAs(OutputType.BYTES);
    }

    // ─── JavaScript Executor ─────────────────────────────────────────────────

    public void scrollToElement(WebElement element) {
        ((JavascriptExecutor) driver).executeScript("arguments[0].scrollIntoView(true);", element);
    }

    public void jsClick(WebElement element) {
        ((JavascriptExecutor) driver).executeScript("arguments[0].click();", element);
    }
}
```

### `LoginPage.java` — Page Object for Login Page

```java
package com.automation.pages;

import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;

public class LoginPage extends BasePage {

    // ─── Locators (using @FindBy annotation) ─────────────────────────────────

    @FindBy(id = "username")
    private WebElement usernameField;

    @FindBy(id = "password")
    private WebElement passwordField;

    @FindBy(id = "loginBtn")
    private WebElement loginButton;

    @FindBy(css = ".error-message")
    private WebElement errorMessage;

    @FindBy(xpath = "//a[@href='/forgot-password']")
    private WebElement forgotPasswordLink;

    // ─── Page Methods ─────────────────────────────────────────────────────────

    public void navigateToLoginPage() {
        navigateTo("https://www.example.com/login");
    }

    public void enterUsername(String username) {
        type(usernameField, username);
    }

    public void enterPassword(String password) {
        type(passwordField, password);
    }

    public void clickLoginButton() {
        click(loginButton);
    }

    public void login(String username, String password) {
        enterUsername(username);
        enterPassword(password);
        clickLoginButton();
    }

    public String getErrorMessage() {
        return getText(errorMessage);
    }

    public boolean isErrorMessageDisplayed() {
        return isDisplayed(errorMessage);
    }

    public void clickForgotPassword() {
        click(forgotPasswordLink);
    }
}
```

### `HomePage.java` — Page Object for Home Page

```java
package com.automation.pages;

import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;

public class HomePage extends BasePage {

    @FindBy(css = ".welcome-message")
    private WebElement welcomeMessage;

    @FindBy(id = "userMenu")
    private WebElement userMenuDropdown;

    @FindBy(linkText = "Logout")
    private WebElement logoutLink;

    @FindBy(css = ".dashboard-header")
    private WebElement dashboardHeader;

    // ─── Page Methods ─────────────────────────────────────────────────────────

    public String getWelcomeMessage() {
        return getText(welcomeMessage);
    }

    public boolean isUserLoggedIn() {
        return isDisplayed(welcomeMessage);
    }

    public void logout() {
        click(userMenuDropdown);
        click(logoutLink);
    }

    public String getDashboardTitle() {
        return getText(dashboardHeader);
    }
}
```

---

## 8. Writing Feature Files (Gherkin)

Feature files use **Gherkin** syntax — a plain English language with specific keywords.

### Gherkin Keywords

| Keyword | Purpose |
|---|---|
| `Feature` | High-level description of the feature |
| `Background` | Steps that run before each scenario |
| `Scenario` | A single test case |
| `Scenario Outline` | Parameterized/Data-driven test |
| `Examples` | Data table for Scenario Outline |
| `Given` | Precondition / initial state |
| `When` | Action / user behavior |
| `Then` | Expected outcome / assertion |
| `And` | Continuation of Given/When/Then |
| `But` | Negative continuation |
| `@TagName` | Tag to group/filter scenarios |
| `#` | Comment |

### `login.feature`

```gherkin
@Login @Smoke
Feature: User Login Functionality
  As a registered user
  I want to log into the application
  So that I can access the dashboard

  Background:
    Given the user is on the login page

  @ValidLogin @Regression
  Scenario: Successful login with valid credentials
    When the user enters username "admin@example.com"
    And the user enters password "Admin@123"
    And the user clicks the login button
    Then the user should be redirected to the dashboard
    And the welcome message should contain "Welcome"

  @InvalidLogin
  Scenario: Failed login with invalid password
    When the user enters username "admin@example.com"
    And the user enters password "WrongPass"
    And the user clicks the login button
    Then an error message "Invalid username or password" should be displayed

  @BlankLogin
  Scenario: Login attempt with blank credentials
    When the user clicks the login button
    Then an error message "Username is required" should be displayed

  @DataDriven
  Scenario Outline: Login with multiple credentials
    When the user enters username "<username>"
    And the user enters password "<password>"
    And the user clicks the login button
    Then the result should be "<result>"

    Examples:
      | username             | password   | result  |
      | admin@example.com    | Admin@123  | success |
      | user@example.com     | User@123   | success |
      | wrong@example.com    | wrong123   | failure |
      | admin@example.com    |            | failure |
```

### `checkout.feature`

```gherkin
@Checkout @E2E
Feature: Shopping Cart and Checkout
  As a logged-in user
  I want to add products to cart and checkout

  Background:
    Given the user is logged in as "admin@example.com" with password "Admin@123"
    And the user is on the products page

  Scenario: Add product to cart
    When the user clicks "Add to Cart" on product "Laptop"
    Then the cart count should be "1"
    And the cart should contain "Laptop"

  Scenario Outline: Add multiple products to cart
    When the user adds product "<product>" to the cart
    Then the cart total should be "<total>"

    Examples:
      | product  | total  |
      | Laptop   | $999   |
      | Phone    | $599   |
      | Tablet   | $449   |
```

---

## 9. Step Definitions

Step Definitions map Gherkin steps to Java code using **annotations**.

### `LoginSteps.java`

```java
package com.automation.stepdefinitions;

import com.automation.pages.HomePage;
import com.automation.pages.LoginPage;
import io.cucumber.java.en.*;
import org.testng.Assert;

public class LoginSteps {

    private LoginPage loginPage;
    private HomePage homePage;

    public LoginSteps() {
        this.loginPage = new LoginPage();
        this.homePage = new HomePage();
    }

    // ─── Given Steps ──────────────────────────────────────────────────────────

    @Given("the user is on the login page")
    public void theUserIsOnTheLoginPage() {
        loginPage.navigateToLoginPage();
    }

    // ─── When Steps ───────────────────────────────────────────────────────────

    @When("the user enters username {string}")
    public void theUserEntersUsername(String username) {
        loginPage.enterUsername(username);
    }

    @When("the user enters password {string}")
    public void theUserEntersPassword(String password) {
        loginPage.enterPassword(password);
    }

    @When("the user clicks the login button")
    public void theUserClicksTheLoginButton() {
        loginPage.clickLoginButton();
    }

    // ─── Then Steps ───────────────────────────────────────────────────────────

    @Then("the user should be redirected to the dashboard")
    public void theUserShouldBeRedirectedToTheDashboard() {
        Assert.assertTrue(homePage.isUserLoggedIn(),
            "User is NOT on dashboard after login");
    }

    @Then("the welcome message should contain {string}")
    public void theWelcomeMessageShouldContain(String expectedMessage) {
        String actual = homePage.getWelcomeMessage();
        Assert.assertTrue(actual.contains(expectedMessage),
            "Welcome message mismatch! Expected: " + expectedMessage + " | Actual: " + actual);
    }

    @Then("an error message {string} should be displayed")
    public void anErrorMessageShouldBeDisplayed(String expectedError) {
        Assert.assertTrue(loginPage.isErrorMessageDisplayed(),
            "Error message is NOT displayed");
        Assert.assertEquals(loginPage.getErrorMessage(), expectedError,
            "Error message text mismatch!");
    }

    @Then("the result should be {string}")
    public void theResultShouldBe(String result) {
        if (result.equals("success")) {
            Assert.assertTrue(homePage.isUserLoggedIn(), "Login should have succeeded!");
        } else {
            Assert.assertTrue(loginPage.isErrorMessageDisplayed(),
                "Login should have failed with error!");
        }
    }
}
```

### `HomeSteps.java`

```java
package com.automation.stepdefinitions;

import com.automation.pages.HomePage;
import com.automation.pages.LoginPage;
import io.cucumber.java.en.*;
import org.testng.Assert;

public class HomeSteps {

    private LoginPage loginPage;
    private HomePage homePage;

    public HomeSteps() {
        this.loginPage = new LoginPage();
        this.homePage = new HomePage();
    }

    @Given("the user is logged in as {string} with password {string}")
    public void theUserIsLoggedIn(String username, String password) {
        loginPage.navigateToLoginPage();
        loginPage.login(username, password);
        Assert.assertTrue(homePage.isUserLoggedIn(), "Login failed!");
    }

    @Then("the user should be logged out")
    public void theUserShouldBeLoggedOut() {
        homePage.logout();
        Assert.assertTrue(loginPage.isDisplayed(/* loginButton */null),
            "User should be on login page after logout");
    }
}
```

---

## 10. Hooks (Before & After)

Hooks run **before** and **after** each scenario or the entire test suite.

### `Hooks.java`

```java
package com.automation.hooks;

import com.automation.utils.DriverManager;
import io.cucumber.java.*;
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;

public class Hooks {

    // ─── Before Each Scenario ─────────────────────────────────────────────────

    @Before(order = 1)
    public void setUp(Scenario scenario) {
        System.out.println("============================");
        System.out.println("Starting Scenario: " + scenario.getName());
        System.out.println("Tags: " + scenario.getSourceTagNames());
        System.out.println("============================");
        DriverManager.initDriver("chrome");
    }

    // ─── After Each Scenario ──────────────────────────────────────────────────

    @After(order = 1)
    public void tearDown(Scenario scenario) {
        // Take screenshot on failure
        if (scenario.isFailed()) {
            System.out.println("❌ SCENARIO FAILED: " + scenario.getName());
            byte[] screenshot = ((TakesScreenshot) DriverManager.getDriver())
                .getScreenshotAs(OutputType.BYTES);
            scenario.attach(screenshot, "image/png", scenario.getName());
        } else {
            System.out.println("✅ SCENARIO PASSED: " + scenario.getName());
        }

        DriverManager.quitDriver();
    }

    // ─── Before Specific Tag ─────────────────────────────────────────────────

    @Before("@Login")
    public void beforeLoginTests() {
        System.out.println("Setting up for Login tests...");
    }

    // ─── After Specific Tag ───────────────────────────────────────────────────

    @After("@Checkout")
    public void afterCheckoutTests() {
        System.out.println("Cleaning up after Checkout tests...");
    }

    // ─── Before All Tests ─────────────────────────────────────────────────────

    @BeforeAll
    public static void globalSetup() {
        System.out.println("========= TEST SUITE STARTED =========");
    }

    // ─── After All Tests ──────────────────────────────────────────────────────

    @AfterAll
    public static void globalTeardown() {
        System.out.println("========= TEST SUITE COMPLETED =========");
    }
}
```

---

## 11. TestNG Runner with Cucumber

### `TestRunner.java`

```java
package com.automation.runner;

import io.cucumber.testng.AbstractTestNGCucumberTests;
import io.cucumber.testng.CucumberOptions;
import org.testng.annotations.DataProvider;

@CucumberOptions(
    // Path to feature files
    features = "src/test/resources/features",

    // Package of step definitions
    glue = {
        "com.automation.stepdefinitions",
        "com.automation.hooks"
    },

    // Tags to run (use empty string "" for all)
    tags = "@Smoke or @Regression",

    // Plugin for reporting
    plugin = {
        "pretty",                                          // Console output
        "html:reports/cucumber-html-report.html",         // HTML report
        "json:reports/cucumber.json",                     // JSON report
        "junit:reports/cucumber.xml",                     // JUnit XML
        "com.aventstack.extentreports.cucumber.adapter.ExtentCucumberAdapter:" // Extent
    },

    // Show monochrome output (no color codes in console)
    monochrome = true,

    // Show step definitions that don't match any steps (for debugging)
    dryRun = false,

    // Order of step definitions search
    stepNotifications = true
)
public class TestRunner extends AbstractTestNGCucumberTests {

    // ─── Parallel Execution ───────────────────────────────────────────────────
    @Override
    @DataProvider(parallel = true)
    public Object[][] scenarios() {
        return super.scenarios();
    }
}
```

### `testng.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">

<suite name="Cucumber Automation Suite" parallel="tests" thread-count="3">

    <test name="Smoke Tests">
        <parameter name="browser" value="chrome"/>
        <classes>
            <class name="com.automation.runner.TestRunner"/>
        </classes>
    </test>

    <test name="Regression Tests">
        <parameter name="browser" value="firefox"/>
        <classes>
            <class name="com.automation.runner.TestRunner"/>
        </classes>
    </test>

</suite>
```

---

## 12. Data-Driven Testing

### Using `Scenario Outline` + `Examples`

```gherkin
Scenario Outline: Validate login with different users
  Given the user is on the login page
  When the user enters username "<email>"
  And the user enters password "<pass>"
  And the user clicks the login button
  Then the result should be "<outcome>"

  Examples:
    | email                | pass       | outcome |
    | admin@example.com    | Admin@123  | success |
    | user@example.com     | User@123   | success |
    | test@example.com     | wrong123   | failure |
```

### Using Data Tables in Step Definitions

```gherkin
Scenario: Fill registration form
  Given the user fills the registration form with:
    | Field     | Value              |
    | FirstName | John               |
    | LastName  | Doe                |
    | Email     | john@example.com   |
    | Phone     | +91-9876543210     |
```

```java
@Given("the user fills the registration form with:")
public void theUserFillsForm(DataTable dataTable) {
    Map<String, String> formData = dataTable.asMap(String.class, String.class);
    registrationPage.enterFirstName(formData.get("FirstName"));
    registrationPage.enterLastName(formData.get("LastName"));
    registrationPage.enterEmail(formData.get("Email"));
    registrationPage.enterPhone(formData.get("Phone"));
}
```

### Using List of Maps

```gherkin
Scenario: Add multiple users
  Given the following users exist:
    | name  | email              | role  |
    | Alice | alice@example.com  | admin |
    | Bob   | bob@example.com    | user  |
```

```java
@Given("the following users exist:")
public void theFollowingUsersExist(List<Map<String, String>> users) {
    for (Map<String, String> user : users) {
        adminPage.createUser(
            user.get("name"),
            user.get("email"),
            user.get("role")
        );
    }
}
```

---

## 13. Tags in Cucumber

Tags are used to **filter, group, and organize** scenarios.

### Defining Tags

```gherkin
@Smoke @Regression
Feature: Login Feature

  @ValidLogin
  Scenario: Successful login

  @InvalidLogin @NegativeTest
  Scenario: Failed login
```

### Running Tags from `@CucumberOptions`

```java
// Run ONLY Smoke tests
tags = "@Smoke"

// Run Smoke AND Regression
tags = "@Smoke or @Regression"

// Run Smoke but NOT Negative tests
tags = "@Smoke and not @NegativeTest"

// Run tests that have BOTH tags
tags = "@Login and @Smoke"

// Run all tests (no tag filter)
tags = ""
```

### Running Tags from Command Line

```bash
# Run specific tag
mvn test -Dcucumber.filter.tags="@Smoke"

# Run multiple tags
mvn test -Dcucumber.filter.tags="@Smoke or @Login"

# Exclude a tag
mvn test -Dcucumber.filter.tags="not @WIP"
```

---

## 14. Reporting

### 1. Built-in Cucumber HTML Report

Set in `@CucumberOptions`:
```java
plugin = { "html:reports/cucumber-report.html" }
```

### 2. Extent Reports Configuration

**`extent.properties`** (in `src/test/resources/`):
```properties
extent.reporter.spark.start=true
extent.reporter.spark.out=reports/ExtentReport.html
screenshot.dir=reports/screenshots/
screenshot.rel.path=screenshots/
```

### 3. Generating Reports via Code

```java
import com.aventstack.extentreports.*;
import com.aventstack.extentreports.reporter.ExtentSparkReporter;

public class ExtentReportManager {

    private static ExtentReports extent;

    public static ExtentReports getInstance() {
        if (extent == null) {
            ExtentSparkReporter reporter = new ExtentSparkReporter("reports/ExtentReport.html");
            reporter.config().setDocumentTitle("Automation Test Report");
            reporter.config().setReportName("Cucumber + Selenium + TestNG");
            reporter.config().setTheme(Theme.DARK);

            extent = new ExtentReports();
            extent.attachReporter(reporter);
            extent.setSystemInfo("OS", System.getProperty("os.name"));
            extent.setSystemInfo("Browser", "Chrome");
            extent.setSystemInfo("Environment", "QA");
        }
        return extent;
    }
}
```

---

## 15. Complete End-to-End Example

### Feature: `ecommerce_login.feature`

```gherkin
@E2E @Smoke
Feature: E-Commerce Login and Product Search

  Background:
    Given the user navigates to the homepage

  @ValidLogin
  Scenario: Login and search for a product
    When the user logs in with "user@shop.com" and "Shop@123"
    Then the user should see the welcome banner
    When the user searches for "Laptop"
    Then the search results should display products containing "Laptop"
    And the first product price should be greater than "$0"
```

### `EcommerceSteps.java`

```java
package com.automation.stepdefinitions;

import com.automation.pages.*;
import io.cucumber.java.en.*;
import org.testng.Assert;

public class EcommerceSteps {

    private HomePage homePage = new HomePage();
    private LoginPage loginPage = new LoginPage();
    private SearchResultsPage searchPage = new SearchResultsPage();

    @Given("the user navigates to the homepage")
    public void navigateToHomepage() {
        homePage.navigateTo("https://www.shop.com");
    }

    @When("the user logs in with {string} and {string}")
    public void loginUser(String email, String password) {
        homePage.clickLoginLink();
        loginPage.login(email, password);
    }

    @Then("the user should see the welcome banner")
    public void verifyWelcomeBanner() {
        Assert.assertTrue(homePage.isWelcomeBannerDisplayed(),
            "Welcome banner is not displayed!");
    }

    @When("the user searches for {string}")
    public void searchProduct(String productName) {
        homePage.searchFor(productName);
    }

    @Then("the search results should display products containing {string}")
    public void verifySearchResults(String keyword) {
        Assert.assertTrue(searchPage.areResultsDisplayed(),
            "No search results found!");
        Assert.assertTrue(searchPage.getFirstProductName().contains(keyword),
            "Product name doesn't match keyword!");
    }

    @Then("the first product price should be greater than {string}")
    public void verifyProductPrice(String minPrice) {
        double price = searchPage.getFirstProductPrice();
        Assert.assertTrue(price > 0, "Product price should be greater than 0!");
    }
}
```

---

## 16. Common Annotations & Gherkin Keywords

### Cucumber Annotations (Java)

| Annotation | Usage |
|---|---|
| `@Given("...")` | Maps to Gherkin `Given` step |
| `@When("...")` | Maps to Gherkin `When` step |
| `@Then("...")` | Maps to Gherkin `Then` step |
| `@And("...")` | Maps to Gherkin `And` step |
| `@But("...")` | Maps to Gherkin `But` step |
| `@Before` | Runs before each scenario |
| `@After` | Runs after each scenario |
| `@BeforeAll` | Runs once before all scenarios |
| `@AfterAll` | Runs once after all scenarios |
| `@BeforeStep` | Runs before each step |
| `@AfterStep` | Runs after each step |

### Parameter Types in Step Definitions

| Expression | Matches | Java Type |
|---|---|---|
| `{string}` | "text in quotes" | `String` |
| `{int}` | Integer numbers | `int` |
| `{float}` | Decimal numbers | `float` |
| `{word}` | Single word (no spaces) | `String` |
| `{double}` | Double numbers | `double` |
| `(.*)` | Any string (regex) | `String` |
| `(\\d+)` | Digit sequence (regex) | `String` / `int` |

### Example

```java
@When("the user adds {int} items of {string} to cart")
public void addItemsToCart(int quantity, String productName) {
    cartPage.addProduct(productName, quantity);
}
```

---

## 17. Best Practices

### Feature Files
- ✅ Write features from the **user's perspective**
- ✅ Keep scenarios **short and focused** (3-7 steps)
- ✅ Use `Background` for repeated `Given` steps
- ✅ Use descriptive, meaningful scenario names
- ✅ Avoid technical details in Gherkin (no XPath, no SQL)
- ✅ Use `Scenario Outline` for data-driven tests

### Step Definitions
- ✅ Keep step definitions **generic and reusable**
- ✅ Use **parameter types** (`{string}`, `{int}`) instead of regex
- ✅ Do NOT put assertions in `@Given` steps — only in `@Then`
- ✅ Create a separate class per feature for step definitions

### POM Design
- ✅ One Page class per web page
- ✅ Never use `driver.findElement()` directly in step definitions
- ✅ Use `@FindBy` annotations + `PageFactory.initElements()`
- ✅ Keep all locators private in page classes
- ✅ Use fluent/builder pattern for chaining page actions

### General
- ✅ Use `ThreadLocal<WebDriver>` for parallel execution
- ✅ Take screenshots on failure in `@After` hook
- ✅ Use `@Tags` to organize and selectively run tests
- ✅ Separate test data from test logic
- ✅ Run tests in CI/CD (Jenkins) using `mvn test`

---

## 18. Common Interview Questions

**Q1. What is Cucumber and how does it support BDD?**
> Cucumber is a BDD testing framework where test scenarios are written in plain English using Gherkin syntax (Given/When/Then), making them understandable by all stakeholders.

**Q2. What is the difference between Scenario and Scenario Outline?**
> `Scenario` is a single test case. `Scenario Outline` is a parameterized template that runs multiple times with different data from the `Examples` table.

**Q3. What is the purpose of the `Background` keyword?**
> `Background` contains steps that run before every scenario in the feature file, avoiding duplication of common setup steps.

**Q4. What is Page Object Model (POM)?**
> POM is a design pattern where each web page has a corresponding Java class containing its locators and action methods, promoting code reuse and maintainability.

**Q5. How do you run specific scenarios using tags?**
> By specifying tags in `@CucumberOptions` like `tags = "@Smoke"`, or from command line: `mvn test -Dcucumber.filter.tags="@Smoke"`.

**Q6. What are Hooks in Cucumber?**
> Hooks (`@Before`, `@After`) are blocks of code that run before/after scenarios for setup and teardown (browser launch, screenshots, driver quit).

**Q7. How do you pass data between step definitions?**
> Using shared state through constructor injection (PicoContainer), Cucumber's `World` object, or a shared context class.

**Q8. How do you run Cucumber tests in parallel?**
> By using `@DataProvider(parallel = true)` in the TestNG runner and `ThreadLocal<WebDriver>` in `DriverManager`.

**Q9. What is the difference between `@CucumberOptions` `features` and `glue`?**
> `features` specifies the path to `.feature` files. `glue` specifies the package containing step definitions and hooks.

**Q10. How do you handle dynamic web elements in POM?**
> Using explicit waits (`WebDriverWait` + `ExpectedConditions`) in the `BasePage` class before interacting with elements.

---

## ▶ Running Tests

```bash
# Run all tests
mvn test

# Run specific tag
mvn test -Dcucumber.filter.tags="@Smoke"

# Run with specific TestNG suite
mvn test -DsuiteXmlFile=testng.xml

# Generate reports
mvn test -Dcucumber.plugin="html:reports/report.html"

# Run in headless mode (set in DriverManager)
mvn test -Dheadless=true
```

---

*Official Documentation:*
- *Cucumber: https://cucumber.io/docs*
- *Selenium: https://www.selenium.dev/documentation*
- *TestNG: https://testng.org/doc*
