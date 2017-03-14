# Unit Testing

In this folder we'll provide a set of real world examples in different languages/platforms.

## Checklist when writing unit tests
This is a checklist you can use to proof your unit tests.

My unit test:
1. Name describes the scenario being tested with conditions and the expected result
2. Uses the Arrange, Act and Assert pattern
3. Is a state, value or interaction test
4. Fakes all dependencies
5. Mocks at most one dependency
6. Favors public methods
7. Asserts against one object

## What is a Unit Test?
There are different ways of defining what a Unit is:

1. Unit as in a single class
2. Unit as a Unit of Work

### Unit as in a single class
* Testing methods in a specific class

### Unit as a Unit of Work
* Defines unit as a scenario/behavior: what happens from invoking a public method to it returning a result
* Unit of Work will often be like testing the business rules/logic
* (Can be interpreted as an integration test depending on your/your team's definition as a Unit of Work can touch different parts of your project in one run)

### What should I choose?
When deciding on a unit testing strategy or choosing what to test:
* Discuss with your team and agree on a strategy:
    1. Is it important to have X % coverage?
    2. 
* Highest risk (riskiest code) often equals highest value and is sure to touch a lot of your code, which in turn gives you a lot of coverage
* If you already have a project and want to add unit tests to it, map out what are the most important parts (highest risk / highest value) and prioritize these to figure out where to start.

## Prerequisites of effective unit testing
* Clean code
* Testable design
* Understanding of unit testing

## Naming a test
The name of the test should be consistent, meaningful and can be read like a sentence. What are you testing and what is the expected result?

One naming convention is the three part naming convention: **UnitOfWork** _ **InitialCondition** _ **ExpectedResult**

**UnitOfWork**: The scenario or method you are testing.

**InitialCondition**: The initial condition for test: Do we test with valid credentials?

**ExpectedResult**: What is the expected result? Should we get a list of objects, a bool value etc?

Creating names in this fashion makes tests just like reading the business rules.

Examples of non-descriptive names:

| Name | Description |
|------|-------------|
| ConstructorTest | Ok, we can deduct that this tests a constructor (or many) for a class. In most cases there's no reason to test the constructor of a class since all other tests that touches the same class will test that for you. |
| LogInUserTest | We can assert that this test tests logging in an user, but it does not say anything on what's being tested. Is every scenario tested in this test? Are there multiple asserts here? What is the result of this test? When it fails will you know why? |

Examples of descriptive names:

| Name | Description|
|------|------------|
| UserLogsIn_WithValidCredentials_RedirectToHome | Just from looking at the name you know what is being tested, with what conditions and what the result should be. This test tests the scenario when the user logs in with valid credentials that a redirect should be issued to the Home (page) |
| UserLogsIn_FailsThreeTimes_LocksOutAccount | Like the one above, this one tests the same scenario but when the user has tried to log in unsuccessfully three times we expect the account to be locked |

## Why giving tests a meaningful name is important

Giving tests names like the ones in the example above can help not only you but others who picks up the code later on to understand what failed and where to look in the code if that test should fail just by the name alone.

A good, descriptive and meaningful name along with a small, simple and clear body will help you minmize the need for debugging the test. 

## Keep your tests simple
* Test body should be clear and simple (as few lines as possible. <15)
* Write the test code as you would production code: DRY (Don't Repeat Yourself) and DAMP (Descriptive And Meaningful Phrases)
* Use the Arrange, Act and Assert pattern to keep the tests concise
* Test one expectation per test
* Multiple asserts on same object can be OK, but try to avoid it.
* The test should point to the precise location of the problem (avoiding debugging)

## Writing testable code
* Ask for what you need in the constructor or parameter method (the actual unit you need to do what you want)
* Avoid "new" -- Pass in dependencies / objects you need in the constructor or method. If your class news up a dependency it will be harder to test
* Avoid mutable global state
* Beware of static methods (difficult to fake)
* Composition over inheritance (The O in the SOLID design principle)
* Add seams to existing code: refactor parts of a system to adhere to principles listed above. 

### Write testable code using the SOLID principles
* Single Responsibility
* Open for extension, closed for modification
* Liskov substitution principle
* Interface segregation
* Dependency inversion

## Qualities of effective unit testing

### Clear and simple
* Just like you would write your methods, try to resist assertin on more than object.
* Test one scenario/output. Split up tests if you test different scenarios in one test

### High value
* Test riskiest scenarios
* Test business rules

### Flexible
* Write tests that target public methods. Don't focus on private methods or hidden methods. Those should be hit when hitting public APIs.
* Testing non-public methods / contracts can create a state where refactoring your code becomes painful because you also have to update and maintain your unit tests.

## Types of unit test
If you are unsure whether your test is simple enough, ask yourself if your test falls into one of these categories and if your test contains two or more then you'll be sure that your test is asserting too much:

1. Value test
2. State test
3. Interaction test

### Value test
A value test verifies your business logic. Given a certain condition we expect a certain value to be returned

### State test
A state test validates that your operation changes state as it should. One example can be that you want to validate that the *UpdatedAt* timestamp on a TodoItem entity changes when updating that entity.

### Interaction test
An interaction test verifies that invoking a certain method calls specific other methods (once, twice etc). An interaction test for saving a TodoItem can check whether ValidateTodoItem was called exactly once, and if the SaveChanges on the Database layer got called.

### Defining your unit test
What is your test doing? Is your test verifying a bit of business logic as well as checking state? Or maybe you're checking that states changes whilst asserting that certain methods were hit?

If your test falls into more than one category then you probably should split up the test into multiple tests: one for each type

## Do I have enough coverage?
When do I have enough coverage? It depends on what your testing strategy is of course and the system in testing of course. If it's a life critical application then maybe make sure you're covered in most areas.

In any case, you can ask yourself the following to get an indication that you've got the coverage you need:
* Have you tested business rules? This is important. If you can't test them all, then map out which ones are the most riskiest and most prone to failure
* Have you tested all the happy and sad scenarions? Don't just test the successful path, try to think of all the (sad) scenarios that can cause failure like logging in with wrong credentials or not providing an description for a TodoItem when saving to database 

## Writing a unit test

### Arrange, act and assert
The Arrange, act and assert pattern provides a consistent pattern for you to use in your tests.

Your test method is grouped into these three parts:
#### Arrange
This is the place where you setup your test: Your initial conditions, dependencies and instances needed to perform the test

#### Act
This is the place where you perform the actual test. This is usally a one-liner like `var result = Instance.DoSomething(initialCondition);`

#### Assert
This is the part where you assert that the result is the expected result.

#### How does that look like?
All the examples follow this pattern so take a look at your language of choice.

```csharp
public void SaveTodoItem_WithValidTitleAndDescription_ReturnsTodoItemWithGeneratedIdValue()
{
    // Arrange
    var todoItem = GetTodoItemWithTitleAndDescription(title: "Update README.md", description: "Organize it better dude!");
    var todoItemService = GetMockedTodoItemService();

    // Act
    var savedTodoItem = todoItemService.SaveTodoItem(todoItem);

    // Assert
    Assert.IsNotNull(savedTodoItem.Id);
}
```

### Stubs, mocks and fakes

* Stubs: A substitute for a dependency in the code under test that allows the code to compile and the dependency to return data as specified by the test but importantly cannot itself directly make a test fail
* Mocks: A substitute for a dependency in the code under test that knows how many times each of its methods were called an in what order so that it can validate an assumption about how the dependency was used and therefore make a test fail
* Fakes: Generic term for stubs/mocks/faking externa dependencies

# Resources

This unit testing framework is based on the following guides, courses, tutorials etc:

* [Effective C# Unit Testing for Enterprise Applications ](https://app.pluralsight.com/library/courses/csharp-unit-testing-enterprise-applications/table-of-contents)