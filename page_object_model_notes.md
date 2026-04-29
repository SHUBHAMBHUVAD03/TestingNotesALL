# 🏗️ Page Object Model — Complete Notes

> 💡 You already know Selenium + TestNG. POM is just a **design pattern** — a smart way to organize your code so it doesn't become a mess as your project grows.

## Contents
- [What is POM and Why](#what-is-pom-and-why)
- [Without POM vs With POM](#without-pom-vs-with-pom)
- [Project Structure](#project-structure)
- [Creating a Page Class](#creating-a-page-class)
- [Creating a Test Class](#creating-a-test-class)
- [BasePage Class](#basepage-class)
- [BaseTest Class](#basetest-class)
- [Page Factory](#page-factory)
- [Real Project — Step by Step](#real-project--step-by-step)
- [Handling Common UI Elements](#handling-common-ui-elements)
- [Page Object with TestNG DataProvider](#page-object-with-testng-dataprovider)
- [Page Object with testng.xml Parameters](#page-object-with-testngxml-parameters)
- [Fluent Page Object Pattern](#fluent-page-object-pattern)
- [Multiple Pages Flow](#multiple-pages-flow)
- [Utilities Classes](#utilities-classes)
- [Full Project Structure](#full-project-structure)
- [Common Mistakes](#common-mistakes)

## What is POM and Why

Without any pattern, most beginners write all Selenium code directly inside test methods. That works for 5 tests. It becomes a nightmare with 50+.

**The problem it solves:**

```
❌ Without POM:
   - Locators are scattered across 50 test files
   - If a button's ID changes → you edit 50 files
   - Test methods are 100 lines long and hard to read
   - Same login code copy-pasted in every test

✅ With POM:
   - Every page has ONE class that owns its locators
   - If a button's ID changes → you edit 1 file
   - Test methods are 5 lines and read like plain English
   - Login code written once, reused everywhere
```

**The core idea:**

```
Each web page in your app  →  has one Java class
That class holds           →  all locators for that page
That class has             →  all actions for that page
Test classes               →  just call those actions
```

## Without POM vs With POM

#### ❌ Without POM — everything in the test

```java
@Test
public void loginTest() {
    driver.get("https://app.com/login");
    driver.findElement(By.id("username")).sendKeys("admin");
    driver.findElement(By.id("password")).sendKeys("admin123");
    driver.findElement(By.id("loginBtn")).click();
    String url = driver.getCurrentUrl();
    Assert.assertTrue(url.contains("dashboard"));
}

@Test
public void loginWithWrongPassword() {
    driver.get("https://app.com/login");
    driver.findElement(By.id("username")).sendKeys("admin");    // duplicated!
    driver.findElement(By.id("password")).sendKeys("wrong");   // duplicated!
    driver.findElement(By.id("loginBtn")).click();             // duplicated!
    String msg = driver.findElement(By.id("error")).getText();
    Assert.assertEquals(msg, "Invalid credentials");
}
```

#### ✅ With POM — clean and reusable

```java
// Test class — reads like plain English
@Test
public void loginTest() {
    loginPage.enterUsername("admin");
    loginPage.enterPassword("admin123");
    loginPage.clickLogin();
    Assert.assertTrue(driver.getCurrentUrl().contains("dashboard"));
}

@Test
public void loginWithWrongPassword() {
    loginPage.enterUsername("admin");
    loginPage.enterPassword("wrong");
    loginPage.clickLogin();
    Assert.assertEquals(loginPage.getErrorMessage(), "Invalid credentials");
}
```

## Project Structure

```
src/
  main/
    java/
      com.yourproject/
        pages/
          LoginPage.java
          HomePage.java
          CartPage.java
          CheckoutPage.java
        base/
          BasePage.java
        utils/
          WaitUtils.java
          ScreenshotUtils.java
          ExcelUtils.java
          ConfigReader.java

  test/
    java/
      com.yourproject/
        tests/
          LoginTest.java
          HomeTest.java
          CartTest.java
        base/
          BaseTest.java
        listeners/
          TestListener.java

  resources/
    config.properties
    testdata/
      loginData.xlsx

testng.xml
pom.xml
```

## Creating a Page Class

A page class has three parts: **locators**, **constructor**, **actions**.

```java
package com.yourproject.pages;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;

public class LoginPage {

    // ─── 1. WebDriver instance ──────────────────────────────────
    private WebDriver driver;

    // ─── 2. Locators ────────────────────────────────────────────
    private By usernameField = By.id("username");
    private By passwordField = By.id("password");
    private By loginButton   = By.id("loginBtn");
    private By errorMessage  = By.id("error");
    private By forgotPassword = By.linkText("Forgot Password?");

    // ─── 3. Constructor ─────────────────────────────────────────
    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }

    // ─── 4. Actions (methods) ────────────────────────────────────
    public void enterUsername(String username) {
        driver.findElement(usernameField).clear();
        driver.findElement(usernameField).sendKeys(username);
    }

    public void enterPassword(String password) {
        driver.findElement(passwordField).clear();
        driver.findElement(passwordField).sendKeys(password);
    }

    public void clickLogin() {
        driver.findElement(loginButton).click();
    }

    public String getErrorMessage() {
        return driver.findElement(errorMessage).getText();
    }

    public void clickForgotPassword() {
        driver.findElement(forgotPassword).click();
    }

    // Combine multiple actions into one convenience method
    public void login(String username, String password) {
        enterUsername(username);
        enterPassword(password);
        clickLogin();
    }
}
```

## Creating a Test Class

```java
package com.yourproject.tests;

import com.yourproject.pages.LoginPage;
import com.yourproject.base.BaseTest;
import org.testng.Assert;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

public class LoginTest extends BaseTest {

    LoginPage loginPage;

    @BeforeMethod
    public void setUpPage() {
        driver.get("https://yourapp.com/login");
        loginPage = new LoginPage(driver);   // pass the driver into the page
    }

    @Test
    public void validLoginTest() {
        loginPage.login("admin", "admin123");
        Assert.assertTrue(driver.getCurrentUrl().contains("dashboard"));
    }

    @Test
    public void invalidLoginTest() {
        loginPage.login("wrong", "wrong");
        Assert.assertEquals(loginPage.getErrorMessage(), "Invalid credentials");
    }

    @Test
    public void emptyUsernameTest() {
        loginPage.enterPassword("admin123");
        loginPage.clickLogin();
        Assert.assertEquals(loginPage.getErrorMessage(), "Username is required");
    }
}
```

## BasePage Class

A `BasePage` holds **common methods** that every page needs — waits, scrolling, checking visibility, etc. All page classes extend this.

```java
package com.yourproject.base;

import org.openqa.selenium.*;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.Select;
import org.openqa.selenium.support.ui.WebDriverWait;

import java.time.Duration;

public class BasePage {

    protected WebDriver driver;
    protected WebDriverWait wait;

    public BasePage(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    }

    // Wait for element and click
    protected void click(By locator) {
        wait.until(ExpectedConditions.elementToBeClickable(locator)).click();
    }

    // Wait for element and type
    protected void type(By locator, String text) {
        WebElement el = wait.until(ExpectedConditions.visibilityOfElementLocated(locator));
        el.clear();
        el.sendKeys(text);
    }

    // Get text of an element
    protected String getText(By locator) {
        return wait.until(ExpectedConditions.visibilityOfElementLocated(locator)).getText();
    }

    // Check if element is visible
    protected boolean isVisible(By locator) {
        try {
            return driver.findElement(locator).isDisplayed();
        } catch (NoSuchElementException e) {
            return false;
        }
    }

    // Select dropdown by visible text
    protected void selectByText(By locator, String text) {
        Select select = new Select(driver.findElement(locator));
        select.selectByVisibleText(text);
    }

    // Select dropdown by value
    protected void selectByValue(By locator, String value) {
        Select select = new Select(driver.findElement(locator));
        select.selectByValue(value);
    }

    // Scroll to an element
    protected void scrollTo(By locator) {
        WebElement el = driver.findElement(locator);
        ((JavascriptExecutor) driver).executeScript("arguments[0].scrollIntoView(true);", el);
    }

    // Wait for element to disappear
    protected void waitForInvisibility(By locator) {
        wait.until(ExpectedConditions.invisibilityOfElementLocated(locator));
    }

    // Get current page URL
    protected String getCurrentUrl() {
        return driver.getCurrentUrl();
    }

    // Get current page title
    protected String getPageTitle() {
        return driver.getTitle();
    }
}
```

#### Now LoginPage extends BasePage:

```java
public class LoginPage extends BasePage {

    private By usernameField = By.id("username");
    private By passwordField = By.id("password");
    private By loginButton   = By.id("loginBtn");
    private By errorMessage  = By.id("error");

    public LoginPage(WebDriver driver) {
        super(driver);   // calls BasePage constructor
    }

    public void enterUsername(String username) {
        type(usernameField, username);   // using BasePage method — no more findElement!
    }

    public void enterPassword(String password) {
        type(passwordField, password);
    }

    public void clickLogin() {
        click(loginButton);
    }

    public String getErrorMessage() {
        return getText(errorMessage);
    }

    public void login(String username, String password) {
        enterUsername(username);
        enterPassword(password);
        clickLogin();
    }
}
```

## BaseTest Class

`BaseTest` handles **browser setup and teardown** for all test classes. All test classes extend this.

```java
package com.yourproject.base;

import io.github.bonigarcia.wdm.WebDriverManager;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.testng.annotations.*;

public class BaseTest {

    protected WebDriver driver;

    @Parameters("browser")
    @BeforeMethod
    public void setUp(@Optional("chrome") String browser) {
        if (browser.equalsIgnoreCase("chrome")) {
            WebDriverManager.chromedriver().setup();
            driver = new ChromeDriver();
        } else if (browser.equalsIgnoreCase("firefox")) {
            WebDriverManager.firefoxdriver().setup();
            driver = new FirefoxDriver();
        }
        driver.manage().window().maximize();
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(5));
    }

    @AfterMethod
    public void tearDown() {
        if (driver != null) {
            driver.quit();
        }
    }
}
```

#### Test class just extends BaseTest — no setup needed:

```java
public class LoginTest extends BaseTest {   // driver is ready to use

    LoginPage loginPage;

    @BeforeMethod(alwaysRun = true)
    public void setUpPage() {
        driver.get("https://yourapp.com/login");
        loginPage = new LoginPage(driver);
    }

    @Test
    public void validLogin() {
        loginPage.login("admin", "admin123");
        Assert.assertTrue(getCurrentUrl().contains("dashboard"));
    }
}
```

## Page Factory

Page Factory is a **built-in TestNG/Selenium feature** that initializes locators automatically using `@FindBy` annotations instead of `By` objects.

#### Without Page Factory:

```java
private By usernameField = By.id("username");

public void enterUsername(String text) {
    driver.findElement(usernameField).sendKeys(text);
}
```

#### With Page Factory:

```java
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;

public class LoginPage {

    @FindBy(id = "username")
    private WebElement usernameField;

    @FindBy(id = "password")
    private WebElement passwordField;

    @FindBy(id = "loginBtn")
    private WebElement loginButton;

    @FindBy(id = "error")
    private WebElement errorMessage;

    @FindBy(xpath = "//a[text()='Forgot Password?']")
    private WebElement forgotPasswordLink;

    // Constructor MUST call PageFactory.initElements
    public LoginPage(WebDriver driver) {
        PageFactory.initElements(driver, this);   // ← this line initializes all @FindBy fields
    }

    public void enterUsername(String username) {
        usernameField.clear();
        usernameField.sendKeys(username);
    }

    public void enterPassword(String password) {
        passwordField.clear();
        passwordField.sendKeys(password);
    }

    public void clickLogin() {
        loginButton.click();
    }

    public String getErrorMessage() {
        return errorMessage.getText();
    }

    public void login(String username, String password) {
        enterUsername(username);
        enterPassword(password);
        clickLogin();
    }
}
```

#### `@FindBy` locator options:

```java
@FindBy(id = "username")
@FindBy(name = "email")
@FindBy(className = "btn-primary")
@FindBy(tagName = "h1")
@FindBy(linkText = "Click here")
@FindBy(partialLinkText = "Click")
@FindBy(css = ".login-form input[type='text']")
@FindBy(xpath = "//button[@type='submit']")
```

#### Find multiple elements (returns a List):

```java
@FindAll({
    @FindBy(css = ".product-card"),
    @FindBy(className = "item")
})
private List<WebElement> productCards;
```

> 💡 Page Factory is slightly cleaner to look at but `By` locators give you more
> flexibility (you can pass them as parameters). Use whichever feels comfortable.

## Real Project — Step by Step

Let's build a full login → dashboard flow.

#### Step 1 — LoginPage.java

```java
package com.yourproject.pages;

import com.yourproject.base.BasePage;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;

public class LoginPage extends BasePage {

    private By usernameField = By.id("username");
    private By passwordField = By.id("password");
    private By loginButton   = By.cssSelector("button[type='submit']");
    private By errorMessage  = By.className("error-text");

    public LoginPage(WebDriver driver) {
        super(driver);
    }

    public void login(String username, String password) {
        type(usernameField, username);
        type(passwordField, password);
        click(loginButton);
    }

    public String getError() {
        return getText(errorMessage);
    }

    public boolean isErrorDisplayed() {
        return isVisible(errorMessage);
    }
}
```

#### Step 2 — DashboardPage.java

```java
package com.yourproject.pages;

import com.yourproject.base.BasePage;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;

public class DashboardPage extends BasePage {

    private By welcomeMessage = By.id("welcome");
    private By userAvatar     = By.className("user-avatar");
    private By logoutButton   = By.id("logout");
    private By notificationBadge = By.className("notification-count");

    public DashboardPage(WebDriver driver) {
        super(driver);
    }

    public String getWelcomeMessage() {
        return getText(welcomeMessage);
    }

    public boolean isAvatarVisible() {
        return isVisible(userAvatar);
    }

    public int getNotificationCount() {
        return Integer.parseInt(getText(notificationBadge));
    }

    public void logout() {
        click(logoutButton);
    }
}
```

#### Step 3 — LoginTest.java

```java
package com.yourproject.tests;

import com.yourproject.base.BaseTest;
import com.yourproject.pages.LoginPage;
import com.yourproject.pages.DashboardPage;
import org.testng.Assert;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

public class LoginTest extends BaseTest {

    LoginPage loginPage;

    @BeforeMethod
    public void openLoginPage() {
        driver.get("https://yourapp.com/login");
        loginPage = new LoginPage(driver);
    }

    @Test(groups = "smoke")
    public void validLogin() {
        loginPage.login("admin", "admin123");

        DashboardPage dashboard = new DashboardPage(driver);
        Assert.assertTrue(dashboard.isAvatarVisible(), "Avatar not visible after login");
        Assert.assertTrue(dashboard.getWelcomeMessage().contains("Welcome"));
    }

    @Test(groups = "regression")
    public void invalidLogin() {
        loginPage.login("wrong@email.com", "wrongpass");
        Assert.assertTrue(loginPage.isErrorDisplayed(), "Error not shown");
        Assert.assertEquals(loginPage.getError(), "Invalid credentials");
    }

    @Test(groups = "regression")
    public void emptyFields() {
        loginPage.login("", "");
        Assert.assertEquals(loginPage.getError(), "All fields are required");
    }
}
```

## Handling Common UI Elements

#### Dropdowns

```java
private By countryDropdown = By.id("country");

public void selectCountry(String country) {
    selectByText(countryDropdown, country);   // from BasePage
}

public void selectCountryByValue(String value) {
    selectByValue(countryDropdown, value);
}
```

#### Checkboxes & Radio Buttons

```java
private By rememberMe  = By.id("rememberMe");
private By termsCheck  = By.id("terms");

public void checkRememberMe() {
    WebElement cb = driver.findElement(rememberMe);
    if (!cb.isSelected()) {
        cb.click();
    }
}

public boolean isRememberMeChecked() {
    return driver.findElement(rememberMe).isSelected();
}
```

#### Tables

```java
private By tableRows = By.cssSelector("table tbody tr");

public int getRowCount() {
    return driver.findElements(tableRows).size();
}

public String getCellValue(int row, int col) {
    // row and col are 1-based
    By cell = By.cssSelector("table tbody tr:nth-child(" + row + ") td:nth-child(" + col + ")");
    return driver.findElement(cell).getText();
}

public List<String> getAllRowsFirstColumn() {
    List<WebElement> rows = driver.findElements(tableRows);
    List<String> values = new ArrayList<>();
    for (WebElement row : rows) {
        values.add(row.findElement(By.tagName("td")).getText());
    }
    return values;
}
```

#### Alerts

```java
public String getAlertText() {
    return driver.switchTo().alert().getText();
}

public void acceptAlert() {
    driver.switchTo().alert().accept();
}

public void dismissAlert() {
    driver.switchTo().alert().dismiss();
}
```

#### File Upload

```java
private By uploadInput = By.id("fileUpload");

public void uploadFile(String filePath) {
    driver.findElement(uploadInput).sendKeys(filePath);
}
```

## Page Object with TestNG DataProvider

```java
// In LoginTest.java
@DataProvider(name = "loginData")
public Object[][] getLoginData() {
    return new Object[][] {
        {"admin@test.com",  "admin123", true,  ""},
        {"user@test.com",   "user123",  true,  ""},
        {"bad@test.com",    "wrong",    false, "Invalid credentials"},
        {"",                "",         false, "All fields are required"},
    };
}

@Test(dataProvider = "loginData")
public void loginWithMultipleUsers(String email, String pass, boolean shouldPass, String errorMsg) {
    loginPage.login(email, pass);

    if (shouldPass) {
        DashboardPage dashboard = new DashboardPage(driver);
        Assert.assertTrue(dashboard.isAvatarVisible());
    } else {
        Assert.assertEquals(loginPage.getError(), errorMsg);
    }
}
```

## Page Object with testng.xml Parameters

```java
// In BaseTest.java
@Parameters({"baseUrl", "browser"})
@BeforeMethod
public void setUp(String baseUrl, @Optional("chrome") String browser) {
    // setup driver based on browser param
    driver.get(baseUrl);
}
```

```xml
<!-- testng.xml -->
<suite name="Suite">
    <test name="StagingTests">
        <parameter name="baseUrl" value="https://staging.yourapp.com"/>
        <parameter name="browser" value="chrome"/>
        <classes>
            <class name="com.yourproject.tests.LoginTest"/>
        </classes>
    </test>

    <test name="ProdTests">
        <parameter name="baseUrl" value="https://yourapp.com"/>
        <parameter name="browser" value="firefox"/>
        <classes>
            <class name="com.yourproject.tests.LoginTest"/>
        </classes>
    </test>
</suite>
```

## Fluent Page Object Pattern

Fluent pattern means each action **returns the page object** so you can chain methods.

```java
public class LoginPage extends BasePage {

    private By usernameField = By.id("username");
    private By passwordField = By.id("password");
    private By loginButton   = By.id("loginBtn");

    public LoginPage(WebDriver driver) {
        super(driver);
    }

    public LoginPage enterUsername(String username) {
        type(usernameField, username);
        return this;   // ← return the same page
    }

    public LoginPage enterPassword(String password) {
        type(passwordField, password);
        return this;
    }

    public DashboardPage clickLogin() {
        click(loginButton);
        return new DashboardPage(driver);   // ← return the next page
    }
}
```

#### Now your test reads like a sentence:

```java
@Test
public void loginAndCheckDashboard() {
    DashboardPage dashboard = loginPage
        .enterUsername("admin")
        .enterPassword("admin123")
        .clickLogin();

    Assert.assertTrue(dashboard.isAvatarVisible());
}
```

## Multiple Pages Flow

Real tests often go across multiple pages. Each page action returns the next page.

```java
// Pages
LoginPage      →  login()       returns DashboardPage
DashboardPage  →  goToCart()    returns CartPage
CartPage       →  checkout()    returns CheckoutPage
CheckoutPage   →  placeOrder()  returns OrderConfirmPage
```

```java
// CartPage.java
public class CartPage extends BasePage {

    private By checkoutBtn   = By.id("checkoutBtn");
    private By cartItems     = By.className("cart-item");
    private By removeButtons = By.className("remove-btn");
    private By totalPrice    = By.id("total");

    public CartPage(WebDriver driver) {
        super(driver);
    }

    public int getItemCount() {
        return driver.findElements(cartItems).size();
    }

    public String getTotalPrice() {
        return getText(totalPrice);
    }

    public CheckoutPage proceedToCheckout() {
        click(checkoutBtn);
        return new CheckoutPage(driver);
    }
}
```

```java
// Full end-to-end test
@Test
public void fullPurchaseFlow() {
    DashboardPage dashboard = loginPage
        .enterUsername("admin")
        .enterPassword("admin123")
        .clickLogin();

    CartPage cart = dashboard.goToCart();
    Assert.assertEquals(cart.getItemCount(), 3);

    CheckoutPage checkout = cart.proceedToCheckout();
    checkout.enterShippingDetails("Shubham", "Mumbai", "400001");

    OrderConfirmPage confirm = checkout.placeOrder();
    Assert.assertTrue(confirm.getOrderId().startsWith("ORD-"));
}
```

## Utilities Classes

#### ConfigReader.java — read from config.properties

```java
package com.yourproject.utils;

import java.io.FileInputStream;
import java.util.Properties;

public class ConfigReader {

    private static Properties properties = new Properties();

    static {
        try {
            FileInputStream fis = new FileInputStream("src/test/resources/config.properties");
            properties.load(fis);
        } catch (Exception e) {
            throw new RuntimeException("config.properties not found");
        }
    }

    public static String get(String key) {
        return properties.getProperty(key);
    }
}
```

```properties
# config.properties
base.url=https://yourapp.com
username=admin
password=admin123
browser=chrome
timeout=10
```

```java
// Usage
driver.get(ConfigReader.get("base.url"));
loginPage.login(ConfigReader.get("username"), ConfigReader.get("password"));
```

#### ScreenshotUtils.java — take screenshot on failure

```java
package com.yourproject.utils;

import org.openqa.selenium.*;
import java.io.File;
import java.nio.file.Files;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class ScreenshotUtils {

    public static void takeScreenshot(WebDriver driver, String testName) {
        try {
            TakesScreenshot ts = (TakesScreenshot) driver;
            File src = ts.getScreenshotAs(OutputType.FILE);

            String timestamp = LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyyMMdd_HHmmss"));
            String path = "screenshots/" + testName + "_" + timestamp + ".png";

            Files.copy(src.toPath(), new File(path).toPath());
            System.out.println("Screenshot saved: " + path);
        } catch (Exception e) {
            System.out.println("Screenshot failed: " + e.getMessage());
        }
    }
}
```

#### WaitUtils.java — custom waits

```java
package com.yourproject.utils;

import org.openqa.selenium.*;
import org.openqa.selenium.support.ui.*;
import java.time.Duration;

public class WaitUtils {

    public static WebElement waitForElement(WebDriver driver, By locator, int seconds) {
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(seconds));
        return wait.until(ExpectedConditions.visibilityOfElementLocated(locator));
    }

    public static void waitForUrlToContain(WebDriver driver, String urlPart, int seconds) {
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(seconds));
        wait.until(ExpectedConditions.urlContains(urlPart));
    }

    public static void waitForPageLoad(WebDriver driver) {
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(30));
        wait.until(d -> ((JavascriptExecutor) d)
            .executeScript("return document.readyState").equals("complete"));
    }
}
```

## Full Project Structure

```
my-selenium-project/
│
├── pom.xml
├── testng.xml
│
├── src/
│   ├── main/
│   │   └── java/
│   │       └── com/yourproject/
│   │           ├── base/
│   │           │   └── BasePage.java
│   │           ├── pages/
│   │           │   ├── LoginPage.java
│   │           │   ├── DashboardPage.java
│   │           │   ├── CartPage.java
│   │           │   └── CheckoutPage.java
│   │           └── utils/
│   │               ├── ConfigReader.java
│   │               ├── WaitUtils.java
│   │               └── ScreenshotUtils.java
│   │
│   └── test/
│       ├── java/
│       │   └── com/yourproject/
│       │       ├── base/
│       │       │   └── BaseTest.java
│       │       ├── tests/
│       │       │   ├── LoginTest.java
│       │       │   ├── DashboardTest.java
│       │       │   └── CartTest.java
│       │       └── listeners/
│       │           └── TestListener.java
│       │
│       └── resources/
│           ├── config.properties
│           └── testdata/
│               └── loginData.xlsx
│
├── screenshots/
└── test-output/          ← TestNG reports go here
```

## Common Mistakes

#### ❌ Putting driver.findElement directly in test classes

```java
// WRONG — defeats the whole point of POM
@Test
public void loginTest() {
    driver.findElement(By.id("username")).sendKeys("admin");  // ← No!
}

// CORRECT — all locators stay in the page class
@Test
public void loginTest() {
    loginPage.enterUsername("admin");
}
```

#### ❌ Not calling super(driver) in page class constructor

```java
// WRONG — BasePage driver is null, everything crashes
public class LoginPage extends BasePage {
    public LoginPage(WebDriver driver) {
        // forgot super(driver)!
    }
}

// CORRECT
public class LoginPage extends BasePage {
    public LoginPage(WebDriver driver) {
        super(driver);   // ← must call this
    }
}
```

#### ❌ Forgetting PageFactory.initElements when using @FindBy

```java
// WRONG — @FindBy fields are all null, NullPointerException
public class LoginPage {
    @FindBy(id = "username")
    private WebElement usernameField;

    public LoginPage(WebDriver driver) {
        // forgot PageFactory.initElements!
    }
}

// CORRECT
public LoginPage(WebDriver driver) {
    PageFactory.initElements(driver, this);   // ← initializes @FindBy fields
}
```

#### ❌ Creating new page objects without navigating to that page

```java
// WRONG — creating CartPage but browser is still on LoginPage
CartPage cart = new CartPage(driver);   // page object created
cart.getItemCount();                    // element not found — wrong page!

// CORRECT — navigate first, then create the page object
loginPage.login("admin", "admin123");
// browser navigates to dashboard
DashboardPage dashboard = new DashboardPage(driver);
CartPage cart = dashboard.goToCart();   // now browser is on cart page
```

#### ❌ Using Thread.sleep in page methods

```java
// WRONG — hardcoded waits make tests slow and still flaky
public void clickLogin() {
    driver.findElement(loginButton).click();
    Thread.sleep(3000);   // ← never do this
}

// CORRECT — use explicit wait in BasePage methods
protected void click(By locator) {
    wait.until(ExpectedConditions.elementToBeClickable(locator)).click();
}
```

#### ❌ One massive page class for the whole site

```java
// WRONG — one class with 200 methods and every locator
public class AllPagesInOne {
    // login locators, dashboard locators, cart locators...
}

// CORRECT — one class per page
LoginPage.java
DashboardPage.java
CartPage.java
```

#### ❌ Returning void from every page method (breaks chaining)

```java
// WRONG — can't chain, can't navigate between pages
public void clickLogin() {
    click(loginButton);
    // returns nothing
}

// CORRECT — return the next page object
public DashboardPage clickLogin() {
    click(loginButton);
    return new DashboardPage(driver);
}
```

> 📌 Open this file in VS Code with **Ctrl + Shift + V** to read it rendered
