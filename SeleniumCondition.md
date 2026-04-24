# Testing
 - There are a 2 types of testing is a company
    - 1. Automatuion Testing 
    - 2. Manual Testing
 
 ## Software Testing 
- There are two types of software testing 
    - Functional Testing = 
    - Non-Functional testing



### SELENIUM 

   #### Xpath


   #### findElement By CssSelector
      - There are 4 Type of CssSelector
          1. tag#id
            findElement(By.CssSelector("#idvalue"));
          2. tag.classname
            findElement(By.CssSelector("input.valueclass"));
          3. tag[attribute = "value"]
            findElement(By.CssSelector("input[attribute = 'value']"));
          4. tag.classname[attribute = "value"]
            findElement(By.CssSelector(input.classname[Attribute = 'value ']));
---
  #### Alart
          There are 3 type of alarts
            1. simple alart
              - driver.switchTo().alart().accept();

            2. OK and CANCEL alart
              - driver.switchTo().alart().accept();
              - driver.switchTo().alart().declien();
            
            3. value alart 
              - Alart myAlart = driver.switchTo().alart();
                myAlart.sendText("value");
          
          How to use a alart without using a SwitchTo command?
           - Using explicit wait
             `Alart myalart = mywait.until(ExopectedConditions.alartIsPresent());`
            - Where the mywait is a object of a explicit wait
              `  `
---
  #### Frames
          - driver.switchTo().frame(name/id/attribute);
          - For going to the back to the default page 
          - driver.switchTo().defaultContent();


  #### Drop Down
          - There are three types of drop down 
            1. Select dropdown 
             - For handling a Select type dropdown 
                ` Select select = new Select(Path of that dropdown); `
             - there are some methodes to select the drop down option
               1. SelectByVisibleText()
               2. selectByValue()
               3. selectByIndex()
               4. getOption() --- for only the return a every one element as a WebElement

            2. bootstrap dropdown // MultiSelect Dropdown
              
            3. Hidden drop down
          

  #### Waits 
          - There are 4 types of waits in selenium
            1. Implicit wait
                driver.manage().timeouts().implicitlywaits(Duration.ofSeconds(10));

            2. Explicit wait
                WebDriverWait wait = new WebDriverWait(Duration.ofSeconds(50));
                WebElement set = wait.until(ExpectedCondition.visibilityOfElementLocated(By.id("element")));

            3. fluent wait
                Wait<WebDriver> wt = new fluentWait<>(driver)
                      .withTimeout(Duration.ofSeconds(10))
                      .pollingEvery(Duration.ofSeconds(5))
                      .ignoring(NoSuchElementExeption.class);

  #### ScreenShot in selenium
  ```
          TakesScreenshot ts = (TakesScreenshot) driver;
          File src = ts.getScreenshotAs(OutputType.FILE);
          File dest =  new File("pic.png");
          FileUtiles.copyFile(src,dest);


  ```
  #### To handle a Muitiple windows
  ``` 
        String parentWindow  = driver.getWindowHandle(); //to get a window handle location of a window
        driver.findElement(By.id("element"));

        Set<String> allWindows = driver.getWindowHandles();

        for(String window : allWindows){
           if(!window.equals(parentWindow)){
            driver.switchTo().window(window);
           }
        }
  ```
  #### Action class methods
  ``` 
        Action act = new Action(driver);

        // for mouse hover 
        act.moveToElement(Element).perform();
        
        // for double click
        act.doubleclick(btn).perform();
        
        // for Right Click
        act.contextClick(btn).perform();
       
        // drag and drop
        act.dragAndDrop(source, Destination);
        
        //click and hold
        act.clickAndHold(btn).perform();
        
        // release
        act.release(btn).perform();
       
        // Move by offset
        act.moveByOffset(100,50).perform();
```
#### js Executer
- Java Executer is use to execute a javaScript code in a Browser
``` JavaScriptExecuter js = (JavaScriptExecuter) driver;
    js.executeScript(script, element);

```
#### POM (Page Object Module)
```
it is a every class is a seprate class which impliment a test class logic


project-name
│
├── src/test/java
│   ├── pages
│   │   ├── LoginPage.java
│   │   ├── HomePage.java
│   │   └── DashboardPage.java
│   │
│   ├── tests
│   │   ├── LoginTest.java
│   │   └── DashboardTest.java
│   │
│   └── utils
│       ├── DriverFactory.java
│       ├── TestBase.java
│       └── ConfigReader.java
│
└── pom.xml   (Maven dependencies)
```

#### To Print all dropdown function by using a selenium
``` 
Webelement dropdown  = driver.findElement(By.name("dropdown"));
Select select = new Select(dropdown);

List<WebElement> option = dropdown.getOption();

for(WebElement x : option){
  System.out.println(x.getText());
}


```
 
#### How to switch to the new Window and Tab
```
driver.switchTo().newWindow(WindowType.TAB);

driver.switchTo().newWindow(WindowType.WINDOW);

```


#### What is TestNG(Testing next generation )
- Test NG is a framwork which is use in a automation testing, 
- with the help of a TestNG we can achive a parallal execution of a testcase, grouping of a tests, priortized the test, automatic test report generation, structured test cases.

#### Anotation in a TestNG 
1. @BeforeSuit
2. @BeforeTest
3. @BeforeClass
4. @BeforeMethod
5. @Test
6. @AfterMethod
7. @AfterTest
8. @AfterClass
9. @AfterSuit

#### Limitation - 
1. Only use for a webapplication 
2. Cptcha QR code and payment gatway are not handeled 
3. Complete automation is not possible 
4. Require a good knowledge of a programming language
5. can't generate test report