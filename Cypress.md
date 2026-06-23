# Cypress
- What is Cypress?



### Limitation of a Cypress
1. Can't switch between the browser windows.
2. Limited iFrame support.
3. Prallel execution in Cypress Cloud.


### Aplication use in node js
` https://playground.bondaracademy.com/pages/iot-dashboard `
` https://conduit.bondaracademy.com/ `

### process to setup the Cypress 
- first to start the node js
` npm init `

- second is to install Cypress in your project
` npm install cypress --save-div `

- Third is to start the execution in the cypress
` npx cypress open `
` npx cypress run `


### Cypress basics
1. cypress tests start from 
                    ```
                    it('name of the test ', ()=>{
                        //here we write the tests
                    })
                    ```

### locator in cypress

               1. by tag
                    cy.get('input')

               2. by id
                    cy.get('#value')
                  
               3. by class value
                    cy.get('.input-full-width')

               4. by attribute
                    cy.get('[attribute value]')
                    
               5. combine attribute
                    cy.get('[attri 1][attri 2]')

### Locator to find the parent  
               1. cy.get('locator').parents('locator').find('type')

  - This is use for to find the next element in the locator field.
               2. cy.get('locator').parent().find('type') 
               
  - It like it is use to go one level up like if we type the parent() one more time then it will be going 2 time up.
               3. cy.get('locator').parent().parent().find('type') 

  - For going until the desire parent() is not found then we use a parentUntil() method.
               4. cy.get('locator').parentUntil('TheLocatorForThePrents').find('type')

### Cypress chain 
  - For the cypress chain we need to continue the execution until there is no stoping in the code like some line not giveing the output for the code

              
               it('Cypress chain command', ()=> {
               cy.get('#inputEmail1')
               .parents('form')
               .find('button')
               .click()

               cy.get('#inputEmail1')
               .parents('form')
               .find('nb-radio')
               .first()
               .should('contain.text', 'Option 1')
                })

#### For Accertion in the Cpress we use the should method

### Reusing the locators
  -  Method 1
               For Reusing locator GLOBALY we use a as() to work as a variable for the locator.
               
               cy.get('#inputEmail1').as('input1')
               cy.get('@input1')
               .parents('form')
               .find('button')
               .click()
  - Method 2
               By using the JQuery method using the then() method. 

                 cy.get('#inputEmail1').then(input2 => {
                    cy.wrap(input2)
                         .parents('form')
                         .find('button')
                         .click()
                 })


### Extracting the value from the web page 
it.only('Extreating values', function () {
- // By using the JQuery method
        cy.get('[for="exampleInputEmail1"]').then(name => {
            const text1 = name.text()
            console.log(text1)
        })

- // Using the invoke method 
        cy.get('[for="exampleInputEmail1"]').invoke('text').then(name2 => {
            console.log(name2)
        })
        cy.get('[for="exampleInputEmail1"]').should('contain', 'Email address')

- // Using the invoke + Alise method means by using as() method
        cy.get('[for="exampleInputEmail1"]').invoke('text').as('name3')
        console.log('name3')

- // Using the invoke to get the attribute value print in the console
        cy.get('#exampleInputEmail1').invoke('attr' , 'class').then(className => {
            console.log(className)
        })

- // For invoking the input field value 
        cy.get('#exampleInputEmail1').type('shubham@gmail.com')
        cy.get('#exampleInputEmail1').invoke('prop', 'value').then(value1 => {
            console.log(value1)
        })
    })

### Cypress Assertions 