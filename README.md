# Simple Understandable Code
A markdown version of a presentation by [DavidGregory084](https://github.com/DavidGregory084) and I

## Background
- These are thoughts distilled from working on multiple projects
- This is meant for an environment where we build for the long term
- Not everything you read here will apply in faster moving environments
- Don't be discouraged from trying out new tools and technologies if they really help

## Why Simplicity Matters
- Code is for humans first, computers second
- Simple code is performant code 
- Code is read far more than it is written
- Simpler code is easier to debug, maintain, and extend
- Performance often comes from clarity, not cleverness

## Thinking Further Ahead
- Code often lives for a long time and passes to multiple owners
- The more complex the code, the more likely that knowledge will be lost about its behaviour
- The long term owners of code are often live service teams or open source volunteers
- Neither of those groups have lots of time to dedicate to a single component
- **Show empathy for the people doing the hard work of looking after software for the long term**

## Infrastructure: Try Not to Reinvent the Wheel

- This is very much subject to your influence on a project
- If you can simplify a service by using a commonly-used, well-supported piece of infrastructure, do so
- Don't create a service that is essentially a message queue like RabbitMQ
- Don't create a service that is essentially a cache like Redis

## Infrastructure: Choose With Care

- Prefer solutions that have stable ownership and existed for some time
- **Use the simplest choices that you can**
    - Don't use Zookeeper if you can use a simple KV store
    - Don't use Kubernetes if you can use standalone containers 
    - Don't use InfluxDB if you can use Postgres
    - Don't use Postgres if you can use sqlite

## Own Your Stack
- **Don’t install a library for every small task**
    - Every dependency is a maintenance and security risk
- **Avoid libraries and frameworks that ship breaking changes often**
    - e.g. Next.js & React, OpenTelemetry
- **Prefer standard libraries and simple solutions** 
    - Only reach for external libraries when they add real value
- **Understand your dependencies**

## Own Your Stack
- **Use simpler libraries, frameworks and techniques when you can**
    - Don't use client-side JS if you can render server-side
    - Don't use "big" frameworks like Django if you can use "small" frameworks like Flask
    - Don't use ORMs if you can write SQL queries

## Avoid Framework Magic
- **Be wary of hidden behaviors and magic**
    - Implicit conventions can confuse and surprise
- **Prefer explicit configuration and wiring**
    - Make the flow of data and control obvious

## Examples of magic

* Spring Boot autoconfiguration
* Mock frameworks like Mockito and Jest
* Code generation based on code, e.g. annotation processors and macros
* Dependency injection frameworks like Spring, Guice

## Why?

* Autoconfiguration embeds too many assumptions and can be broken by environmental changes
* Mocks enable impossible behaviours, break the rules of the language and disguise real problems
* Annotation processors and macros generate code that we can't read or debug
* Dependency injection frameworks add overhead to test suites and incentivise code with too many dependencies

## Clarification

* Each of these tools are useful and have a purpose
* However, they have become *defaults* and are used without care
* They help us ignore complex code instead of tackling the complexity
* **You don't need dependency injection to create a class with two dependencies**
* **You don't need a mock to implement a class with two methods**

## Code That’s Easy to Reason About
- **Avoid unnecessary abstractions** 
    - Don’t abstract until you see repetition or complexity
    - Write boring code
    - Go suggests 3 uses before considering abstraction
- **Favor legibility over cleverness**
    - If it’s clever, it’s probably wrong

## Example: Error handling with Either/Result
- These data structures let us recover from errors without exceptions
- They let us accumulate multiple errors
- **Don't use them if you can't recover from the errors that they model**
- **Don't use them if only one error can be thrown**
- **Remember the purpose of abstractions, and use them with intention**

## Example: Double-handling
- Most web frameworks provide a top-level exception handler
- Unless you can recover from an error, **let it crash**
- Provide useful context via logging, not response messages
- There is no added value in manually creating a 500 response
- You can normally configure the exception handler if you have specific needs

## Locality vs. Separation of Concerns
- **Locality of behavior:**
    - Keep related logic together for easier understanding
- **Separation of concerns:**
    - Split code by responsibility, but not at the cost of clarity
- **Find the right balance for your team and project**

## Every component that you create has a cost
- **Wait until new components are justified**
- **Components that do little work have a cost**
    - More time to run your test suite
    - Tests that are more setup than value
    - More code to maintain during library upgrades
    - More indirection that readers have to follow
    - More logic to debug when replicating issues

## Example: Unnecessary stratification
- It's common to use certain conventions about components in MVC frameworks
    - "controllers" handle protocol interactions (e.g. HTTP requests and responses)
    - "repositories" interact with data stores
    - "connectors" / "clients" interact with external systems
    - "services" orchestrate other logic involving other components

## Example: Unnecessary stratification
- **Don't create components just to follow this structure**
- **It's fine for controllers to call connectors and repositories**
- **Create components once you have a few lines of logic to extract**
- **"Services" are particularly vulnerable to this kind of bloat** 

## Use Mocks Sparingly, Prefer Reality
- **Don’t always reach for mocks by default** 
    - Prefer real, fast, in-memory dependencies when possible
    - Mocks can hide integration issues and create false confidence
- **Test the real thing whenever you can**

## Using fakes for unit tests
- **Use interfaces to provide alternative implementations**
    - For example: a fake repository can be implemented using a hashmap
    - Fakes provide more realistic behaviour, and remove mock setup from tests
    - Fakes are normal user code, and can't break the rules of the language

## Use real components for higher level tests
- Tools like Testcontainers allow us to automatically spin up supporting components
    - You can create a Postgres or MongoDB container scoped to a test suite
    - You can spin up message queues, or AWS simulators like LocalStack
- If you can't use containers, in-memory databases like H2 and sqlite may be useful

## Remember the Purpose of Tests
- Tests exist to give us confidence that our software works
- They give us a safety net when we make changes
- They do not exist to reach a coverage number
- Hitting each line of code once is not enough
- Hitting each line of code can be impractical
- Library and framework code can still surprise you

## Remember the Purpose of Tests
- 100% coverage is impractical **and** not enough?
    - Some lines of code are more important than others
    - You might not need to test "adapter" code that calls a single method
    - The underlying method might provide information that lets you write better tests
    - Some conditions are impractical to test, e.g. discarded writes in clustered databases
- **Focus on testing behaviour**

## Takeaways
- Simplicity is a feature
- Be intentional with tools, libraries, and abstractions
- Write code your future self (and teammates) will thank you for
