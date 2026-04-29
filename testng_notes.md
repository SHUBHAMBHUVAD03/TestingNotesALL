# 🧪 TestNG — Complete Notes for Selenium Developers

> 💡 You already know Selenium. TestNG is just the **framework that controls how your tests run.**
> Think of Selenium as the tool that drives the browser, and TestNG as the manager that organizes, runs, and reports your tests.

## Contents
- [What is TestNG](#what-is-testng)
- [Setup in Maven](#setup-in-maven)
- [Your First TestNG Test](#your-first-testng-test)
- [Core Annotations](#core-annotations)
- [Assertions](#assertions)
- [TestNG with Selenium](#testng-with-selenium)
- [Groups](#groups)
- [Priority & Order](#priority--order)
- [Parameters from XML](#parameters-from-xml)
- [DataProvider](#dataprovider)
- [Listeners](#listeners)
- [Soft Assertions](#soft-assertions)
- [Parallel Execution](#parallel-execution)
- [Dependency Between Tests](#dependency-between-tests)
- [testng.xml — Full Reference](#testngxml--full-reference)
- [Reporting](#reporting)
- [Common Mistakes](#common-mistakes)

## What is TestNG

TestNG = **Test Next Generation**

Without TestNG you would need to manually run each Selenium test and check results yourself. TestNG gives you:

- Run tests in a specific **order**
- Run tests in **parallel** (multiple browsers at once)
- **Skip** tests conditionally
- Pass **data** to tests automatically
- Generate **HTML reports** automatically
- **Group** tests like Smoke, Regression, Sanity
- **Retry** failed tests

```
Selenium alone   →  opens browser, clicks, types
TestNG           →  decides WHAT runs, WHEN, HOW MANY TIMES, with WHAT DATA
```

## Setup in Maven

Add this to your `pom.xml` inside `<dependencies>`:

```xml
<!-- TestNG -->
<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.8.0</version>
    <scope>test</scope>
</dependency>

<!-- Selenium -->
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>4.18.1</version>
</dependency>
```

Also add the Maven Surefire plugin to run TestNG via `mvn test`:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.2.5</version>
            <configuration>
                <suiteXmlFiles>
                    <suiteXmlFile>testng.xml</suiteXmlFile>
                </suiteXmlFiles>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## Your First TestNG Test

```java
import org.testng.annotations.Test;

public class MyFirstTest {

    @Test
    public void openGoogle() {
        System.out.println("This is my first TestNG test!");
        // Selenium code goes here
    }
}
```

> 💡 The only difference from plain Java is the `@Test` annotation above your method.
> TestNG sees `@Test` and knows — *this method is a test case.*

#### How to Run
- Right-click the file in IntelliJ → **Run as TestNG**
- Or run via terminal: `mvn test`

## Core Annotations

Annotations tell TestNG **when to run a method** — before tests, after tests, etc.

#### Execution Order (top to bottom)

```
@BeforeSuite      → runs once before all suites
@BeforeTest       → runs once before all tests in <test> tag
@BeforeClass      → runs once before all methods in a class
@BeforeMethod     → runs before EACH test method
@Test             → the actual test
@AfterMethod      → runs after EACH test method
@AfterClass       → runs once after all methods in a class
@AfterTest        → runs once after all tests in <test> tag
@AfterSuite       → runs once after all suites
```

#### Real Example

```java
import org.testng.annotations.*;

public class AnnotationDemo {

    @BeforeSuite
    public void beforeSuite() {
        System.out.println("1 — Before Suite: Setup DB connection");
    }

    @BeforeClass
    public void beforeClass() {
        System.out.println("2 — Before Class: runs once for this class");
    }

    @BeforeMethod
    public void beforeMethod() {
        System.out.println("3 — Before Method: runs before each @Test");
    }

    @Test
    public void testOne() {
        System.out.println("4 — Test One running");
    }

    @Test
    public void testTwo() {
        System.out.println("4 — Test Two running");
    }

    @AfterMethod
    public void afterMethod() {
        System.out.println("5 — After Method: runs after each @Test");
    }

    @AfterClass
    public void afterClass() {
        System.out.println("6 — After Class: cleanup");
    }

    @AfterSuite
    public void afterSuite() {
        System.out.println("7 — After Suite: close DB");
    }
}
```

#### Output order will be:
```
Before Suite
Before Class
  Before Method → Test One → After Method
  Before Method → Test Two → After Method
After Class
After Suite
```

## Assertions

Assertions **verify** your test result. If an assertion fails, the test is marked as **FAILED**.

#### Hard Assertions (stop test on failure)

```java
import org.testng.Assert;

Assert.assertEquals(actual, expected);           // Check actual equals expected
Assert.assertNotEquals(actual, expected);        // Check they are NOT equal
Assert.assertTrue(condition);                    // Check condition is true
Assert.assertFalse(condition);                   // Check condition is false
Assert.assertNull(object);                       // Check object is null
Assert.assertNotNull(object);                    // Check object is not null
```

#### Real Example

```java
@Test
public void checkTitle() {
    driver.get("https://google.com");
    String actualTitle = driver.getTitle();
    String expectedTitle = "Google";

    Assert.assertEquals(actualTitle, expectedTitle, "Page title did not match!");
    // If titles don't match → test FAILS immediately here
}
```

> ⚠️ With hard assertions, if line 1 fails, lines 2, 3, 4 do NOT run.
> Use Soft Assertions if you want all checks to run before failing.

## TestNG with Selenium

This is the standard pattern you will use in every project:

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.Assert;
import org.testng.annotations.*;

public class LoginTest {

    WebDriver driver;

    @BeforeMethod
    public void setUp() {
        // Launch browser before each test
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        driver.get("https://yourapp.com/login");
    }

    @Test
    public void validLogin() {
        driver.findElement(By.id("username")).sendKeys("admin");
        driver.findElement(By.id("password")).sendKeys("admin123");
        driver.findElement(By.id("loginBtn")).click();

        String actualUrl = driver.getCurrentUrl();
        Assert.assertTrue(actualUrl.contains("dashboard"), "Login failed!");
    }

    @Test
    public void invalidLogin() {
        driver.findElement(By.id("username")).sendKeys("wronguser");
        driver.findElement(By.id("password")).sendKeys("wrongpass");
        driver.findElement(By.id("loginBtn")).click();

        String errorMsg = driver.findElement(By.id("error")).getText();
        Assert.assertEquals(errorMsg, "Invalid credentials");
    }

    @AfterMethod
    public void tearDown() {
        // Close browser after each test
        if (driver != null) {
            driver.quit();
        }
    }
}
```

> 💡 `@BeforeMethod` opens browser. `@AfterMethod` closes it.
> Each test gets a fresh browser — this is the clean way to do it.

## Groups

Groups let you **tag** tests and run only specific ones.

```java
@Test(groups = "smoke")
public void verifyHomePage() { }

@Test(groups = "smoke")
public void verifyLoginPage() { }

@Test(groups = "regression")
public void verifyCheckout() { }

@Test(groups = {"smoke", "regression"})
public void verifySearch() { }   // belongs to both groups
```

#### Run only smoke tests in testng.xml:

```xml
<groups>
    <run>
        <include name="smoke"/>
    </run>
</groups>
```

#### Exclude a group:

```xml
<groups>
    <run>
        <include name="regression"/>
        <exclude name="smoke"/>
    </run>
</groups>
```

## Priority & Order

By default TestNG runs tests in **alphabetical order**. Use `priority` to control the order.

```java
@Test(priority = 1)
public void openBrowser() { }

@Test(priority = 2)
public void login() { }

@Test(priority = 3)
public void addToCart() { }

@Test(priority = 4)
public void checkout() { }
```

> 💡 Lower number = runs first. Negative numbers are allowed: `-1, 0, 1, 2`

```java
@Test(priority = -1)   // runs before priority 0
public void preCheck() { }
```

## Parameters from XML

Pass data from `testng.xml` into your test method.

#### In testng.xml:

```xml
<parameter name="browser" value="chrome"/>
<parameter name="url" value="https://google.com"/>
```

#### In your test class:

```java
import org.testng.annotations.Parameters;

@Parameters({"browser", "url"})
@BeforeMethod
public void setUp(String browser, String url) {
    if (browser.equals("chrome")) {
        driver = new ChromeDriver();
    } else if (browser.equals("firefox")) {
        driver = new FirefoxDriver();
    }
    driver.get(url);
}
```

> 💡 Great for running the same tests on different browsers or environments
> without changing your code — just change the XML.

## DataProvider

Run the **same test multiple times with different data.**

#### Example — Login with multiple users:

```java
import org.testng.annotations.DataProvider;

// Step 1: Create the data provider
@DataProvider(name = "loginData")
public Object[][] getLoginData() {
    return new Object[][] {
        {"admin@test.com",   "admin123",  true},   // valid user
        {"user@test.com",    "user123",   true},   // valid user
        {"wrong@test.com",   "wrongpass", false},  // invalid user
    };
}

// Step 2: Use it in your test
@Test(dataProvider = "loginData")
public void testLogin(String email, String password, boolean shouldPass) {
    driver.findElement(By.id("email")).sendKeys(email);
    driver.findElement(By.id("password")).sendKeys(password);
    driver.findElement(By.id("loginBtn")).click();

    if (shouldPass) {
        Assert.assertTrue(driver.getCurrentUrl().contains("dashboard"));
    } else {
        Assert.assertTrue(driver.findElement(By.id("error")).isDisplayed());
    }
}
```

This will run `testLogin` **3 times** — once for each row of data.

#### DataProvider from a separate class:

```java
// In DataProviderClass.java
public class DataProviderClass {

    @DataProvider(name = "searchTerms")
    public static Object[][] searchData() {
        return new Object[][] {
            {"laptop"},
            {"phone"},
            {"tablet"}
        };
    }
}

// In your test class
@Test(dataProvider = "searchTerms", dataProviderClass = DataProviderClass.class)
public void testSearch(String keyword) {
    driver.findElement(By.id("searchBox")).sendKeys(keyword);
}
```

## Listeners

Listeners **listen to test events** (pass, fail, skip) and let you take action.

#### Most useful listener: `ITestListener`

```java
import org.testng.*;

public class MyListener implements ITestListener {

    @Override
    public void onTestStart(ITestResult result) {
        System.out.println("▶ Starting: " + result.getName());
    }

    @Override
    public void onTestSuccess(ITestResult result) {
        System.out.println("✅ Passed: " + result.getName());
    }

    @Override
    public void onTestFailure(ITestResult result) {
        System.out.println("❌ Failed: " + result.getName());
        // Take screenshot here automatically
        takeScreenshot(result.getName());
    }

    @Override
    public void onTestSkipped(ITestResult result) {
        System.out.println("⏭ Skipped: " + result.getName());
    }

    private void takeScreenshot(String testName) {
        // Your screenshot code here
        TakesScreenshot ts = (TakesScreenshot) driver;
        File src = ts.getScreenshotAs(OutputType.FILE);
        // copy to your screenshots folder
    }
}
```

#### Add listener to your class:

```java
@Listeners(MyListener.class)
public class LoginTest {
    // your tests
}
```

#### Or add in testng.xml (applies to all tests):

```xml
<listeners>
    <listener class-name="com.yourpackage.MyListener"/>
</listeners>
```

## Soft Assertions

With **hard assertions** (`Assert`), the test stops at the first failure.
With **soft assertions**, ALL assertions run and all failures are reported together.

```java
import org.testng.asserts.SoftAssert;

@Test
public void checkProfilePage() {
    SoftAssert softAssert = new SoftAssert();   // create instance

    String name = driver.findElement(By.id("name")).getText();
    String email = driver.findElement(By.id("email")).getText();
    String phone = driver.findElement(By.id("phone")).getText();

    softAssert.assertEquals(name, "Shubham",          "Name mismatch");
    softAssert.assertEquals(email, "s@test.com",      "Email mismatch");
    softAssert.assertEquals(phone, "9999999999",      "Phone mismatch");

    softAssert.assertAll();   // ← MUST call this at the end
    // If any assertion failed above, the test fails here with ALL errors listed
}
```

> ⚠️ Always call `softAssert.assertAll()` at the end.
> Without it, failures are silently ignored and the test will always pass.

## Parallel Execution

Run tests at the **same time** to save time.

#### Parallel at test level (different `<test>` blocks):

```xml
<suite name="MySuite" parallel="tests" thread-count="2">
    <test name="ChromeTest">
        <parameter name="browser" value="chrome"/>
        <classes>
            <class name="com.tests.LoginTest"/>
        </classes>
    </test>
    <test name="FirefoxTest">
        <parameter name="browser" value="firefox"/>
        <classes>
            <class name="com.tests.LoginTest"/>
        </classes>
    </test>
</suite>
```

#### Parallel at method level (each `@Test` in a new thread):

```xml
<suite name="MySuite" parallel="methods" thread-count="3">
```

#### Parallel at class level:

```xml
<suite name="MySuite" parallel="classes" thread-count="2">
```

> ⚠️ For parallel tests, use `ThreadLocal<WebDriver>` to avoid threads sharing the same browser.

```java
ThreadLocal<WebDriver> driver = new ThreadLocal<>();

@BeforeMethod
public void setUp() {
    driver.set(new ChromeDriver());
}

public WebDriver getDriver() {
    return driver.get();
}

@AfterMethod
public void tearDown() {
    driver.get().quit();
    driver.remove();
}
```

## Dependency Between Tests

Make a test run only if another test passes.

```java
@Test
public void login() {
    // login steps
}

@Test(dependsOnMethods = "login")
public void addToCart() {
    // only runs if login() passed
}

@Test(dependsOnMethods = "addToCart")
public void checkout() {
    // only runs if addToCart() passed
}
```

#### Depend on a group:

```java
@Test(dependsOnGroups = "smoke")
public void regressionTest() {
    // runs only after all smoke tests pass
}
```

> 💡 If `login()` fails, `addToCart()` and `checkout()` are automatically **skipped** — not failed.

#### Soft dependency (run even if dependency failed):

```java
@Test(dependsOnMethods = "login", alwaysRun = true)
public void takeScreenshot() {
    // runs even if login() failed
}
```

## testng.xml — Full Reference

This file **controls everything** — which tests run, in what order, with what data.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">

<suite name="Full Test Suite" parallel="tests" thread-count="2" verbose="2">

    <!-- Global listeners -->
    <listeners>
        <listener class-name="com.tests.listeners.MyListener"/>
    </listeners>

    <!-- Global parameters -->
    <parameter name="env" value="staging"/>

    <!-- Test block 1 -->
    <test name="Smoke Tests">
        <parameter name="browser" value="chrome"/>

        <groups>
            <run>
                <include name="smoke"/>
            </run>
        </groups>

        <classes>
            <class name="com.tests.LoginTest"/>
            <class name="com.tests.HomeTest">
                <methods>
                    <include name="checkTitle"/>    <!-- run only this method -->
                    <exclude name="slowTest"/>      <!-- skip this method -->
                </methods>
            </class>
        </classes>
    </test>

    <!-- Test block 2 -->
    <test name="Regression Tests">
        <parameter name="browser" value="firefox"/>

        <packages>
            <package name="com.tests.regression"/>   <!-- run all tests in package -->
        </packages>
    </test>

</suite>
```

#### Key XML attributes:

```
parallel="tests"       → run <test> blocks in parallel
parallel="methods"     → run @Test methods in parallel
parallel="classes"     → run classes in parallel
thread-count="3"       → number of parallel threads
verbose="2"            → console output level (0=none, 10=all)
preserve-order="true"  → run classes in the order listed in XML
```

## Reporting

TestNG generates reports **automatically** after every run.

#### Default Report Location:
```
your-project/
  test-output/
    index.html        ← open this in browser
    emailable-report.html
    testng-results.xml
```

#### Open the report:
```bash
# After running tests, open this file in your browser
test-output/index.html
```

#### Better reports with Extent Reports (popular choice):

```xml
<!-- Add to pom.xml -->
<dependency>
    <groupId>com.aventstack</groupId>
    <artifactId>extentreports</artifactId>
    <version>5.1.1</version>
</dependency>
```

```java
// Basic Extent Reports setup in your Listener
ExtentReports extent = new ExtentReports();
ExtentSparkReporter spark = new ExtentSparkReporter("test-output/ExtentReport.html");
extent.attachReporter(spark);

ExtentTest test = extent.createTest("loginTest");

// On pass:
test.pass("Login test passed");

// On fail:
test.fail("Login test failed: " + result.getThrowable());

extent.flush();   // always call at end
```

## Common Mistakes

#### ❌ Forgetting `softAssert.assertAll()`

```java
// WRONG — failures are silently ignored
softAssert.assertEquals(actual, expected);
// missing softAssert.assertAll() — test always passes!

// CORRECT
softAssert.assertEquals(actual, expected);
softAssert.assertAll();   // ← always add this
```

#### ❌ Using `driver.close()` instead of `driver.quit()`

```java
// WRONG — closes tab but leaves ChromeDriver process running
driver.close();

// CORRECT — closes browser AND kills the driver process
driver.quit();
```

#### ❌ Sharing WebDriver across parallel tests

```java
// WRONG — parallel tests share the same driver and crash each other
static WebDriver driver;

// CORRECT — each thread gets its own driver
ThreadLocal<WebDriver> driver = new ThreadLocal<>();
```

#### ❌ No `@AfterMethod` cleanup

```java
// WRONG — browser never closes, memory leak
@Test
public void myTest() {
    driver = new ChromeDriver();
    // no cleanup
}

// CORRECT
@AfterMethod
public void tearDown() {
    driver.quit();
}
```

#### ❌ Calling `Assert` after driver action fails

```java
// The click throws an exception before Assert even runs
driver.findElement(By.id("missing")).click();
Assert.assertTrue(something);   // never reached

// CORRECT — use try/catch or ensure element exists first
WebElement btn = driver.findElement(By.id("loginBtn"));
Assert.assertNotNull(btn, "Login button not found");
btn.click();
```

#### ❌ Hard-coding waits instead of WebDriverWait

```java
// WRONG — flaky, slow
Thread.sleep(3000);

// CORRECT
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.elementToBeClickable(By.id("loginBtn")));
```

> 📌 Open this file in VS Code with **Ctrl + Shift + V** to read it rendered
