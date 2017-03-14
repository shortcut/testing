# Unit Testing

In this folder we'll provide a set of real world examples in different languages/platforms.

## Checklist when writing unit tests

## Naming a test
The name of the test should be consistent and meaningful. What are you testing and what you expect the result to be?

One naming convention is: **UnitOfWork** _ **InitialCondition** _ **ExpectedResult**

**UnitOfWork**: The scenario you are testing

**InitialCondition**: The initial condition for test: Do we test with valid credentials?
**ExpectedResult**: What is the expected result? Should we get a list of objects, a true value etc?

Examples:

UserLogsIn_WithValidCredentials_RedirectToHome

UserLogsIn_FailsThreeTimes_LocksOutAccount

## Prerequisites of effective unit testing
* Clean code
* Testable design
* Understanding of unit testing

## Qualities of effective unit testing

### Clear and simple
* Just like you would write your methods, try to resist assertin on more than object.
* Test one scenario/output. Split up tests if you test different scenarios in one test

### High value
* Test riskiest scenarios
* Test business rules

### Flexible
* Write tests that target public methods. Don't focus on private methods or hidden methods. Those should be hit when hitting public APIs.
