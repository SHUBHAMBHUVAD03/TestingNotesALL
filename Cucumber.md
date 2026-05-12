### Cucumber

1. TDD = Test Driven Development
- TestNG
2. BDD = Behaviral Driven Development
- Cucumber

- BDD is a behaviral Drivern Development tool which is use to write a test case is simple or a humal reconized language (Gherkin)
- It is use to emphasize the gap between the technical and non technical people in the testing filds like a tester or a stak holder 


``` 
1. Feature File - Enetry point, where we write a senario of test 
                - this come with a .fearture file
                - Write a steps and senarios in this file
                - language is a gherkin
                
2. Step Difination File 
                - for every method there is a java methods in this file 
                - selenium code this contain
                
3. Test Runner File
                - JUnit file 
                - java class where we set a path for athe feature file adn step difination file 
                - where we write a logs and all technical stuf

While running the flow in reverse first we run the Test runner file
```

#### Gherkin language 
##### Keywords in the Gherkin language
1. Given 
2. Then
3. When 
4. And

```
Under senario there is a 
pre-condition - Given 
Action - When
Validation - Then

```

#### Sample of a feature file
``` 
Feature : Login Functionality
    Senario: Successfull login
    Given the open the login page
    When
    Then
    And


Execute the feature file ant then we get the step defination file content with method....
```

#### Step Difination file 
                - in this file the Annotation and the Dirscription is important
                - Write a simple code in the each method using a selenium code and the method will executed

#### Runner File 
                - in this file we call the all the files we created like a feature and a step difination file 
                - Annotatation is
```
                @RunWith(Cucumber.class)
                @CucumberOptions(features = "Path")
                public void Test(){
                    //
                }
```






