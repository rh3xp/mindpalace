#### The Clean Architecture

![[the clean architecture.png]]



[The Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) is the system architecture guideline proposed by Robert C. Martin (Uncle Bob) derived from many architectural guidelines like Hexagonal Architecture, Onion Architecture, etc... over the years.

This is one of the guidelines adhered to by software engineers to build scalable, testable, and maintainable software.

#### Advantages of Proper Architecture

-   Testable
-   Maintainable
-   Changeable
-   Easy to Develop
-   Easy to Deploy
-   Independent

![[the clean architecture original.png]]


Each circle represents different areas of the software. The outermost layer is the lowest level of the software and as we move in deeper, the level will be higher.


#### The Dependency Rule

The Dependency Rule states that the source code dependencies can only point inwards.

This means nothing in an inner circle can know anything at all about something in an outer circle. i.e. the inner circle shouldn’t depend on anything in the outer circle.



![[the clean architecture line.png]]

Remember, the arrow should be read as “depend on”. i.e. `Frameworks and Drivers` should depend on `Interface Adapters`, which depend on `Application Business Rules` which depend on `Enterprise Business Rules`.

****Nothing in the bottom layer should depend on the top layer.***

#### Frameworks and Drivers

Software areas that reside inside this layer are

-   User Interface
-   Database
-   External Interfaces (eg: Native platform API)
-   Web (eg: Network Request)
-   Devices (eg: Printers and Scanners)

#### Interface Adapters

This layer holds

-   `Presenters` (UI Logic, States)
-   `Controllers` (Interface that holds methods needed by the application which is implemented by Web, Devices or External Interfaces)
-   `Gateways` (Interface that holds every CRUD operation performed by the application, implemented by DB)

#### Application Business Rules

Rules which are not Core-business-rules but essential for this particular application come under this. This layer holds `Use Cases`**.** As the name suggests, it should provide every use case of the application. i.e. it holds each and every functionality provided by the application.

Also, this is the layer that determines which `Controller` / `Gateway` to be called for the particular use case.

>This is where different modules are coordinated. For instance, we want to apply a discount for the user who purchased for x amount within a month.

>Here we need to get the amount the user has spent on this month from the `purchase module` and then with the result we need to apply the discount for the user in the `checkout module`**.** Here `applyDiscountUseCase` calls the purchase module’s controller for the data and then applies the discount in the checkout module.


#### Enterprise Business Rules

This is the layer that holds core-business rules or domain-specific business rules. Also, this layer is the least prone to change.


##### Let's check this example

How can we architect an app that translates the sentence given by the user using a translation API?

![[the clean architecture example.png]]

Each layer does a specific thing. Looks good right? Let’s check the dependency flow for this above architecture to know if anything is wrong.

Remember Dependency Rule? “The Dependency Rule states that the source code dependencies can only point inwards”.

![[the clean arch 4.png]]


UI → Presenter (✅ Not Violating)

Presenter → Translate Usecase (✅ Not Violating)

Translate Usecase → Translate Controller (❌ Violating)

Translate Controller → Web (❌ Violating)

But it seems correct, right?

`UI` requests data from `Presenter` which requests data from `Use Case` which should request data from `Controller` which should request data from `Web`.

_But the Dependency Rule strictly says dependencies can only point inwards. It adds up by saying this is the rule that makes the architecture work._

In order to pass this rule, we need to invert the arrow to the opposite direction. Is that possible? Here comes *[[Polymorphism]]*. When we include some Polymorphism here, something magic happens.

Simply by having an `Interface` between these 2 layers, we could invert the dependency. This is known as [The Dependency Inversion Principle](https://stackify.com/dependency-inversion-principle/).


##### The Dependency Inversion Principle

![[Dependency Inversion Principle.png]]


![[Dependency Inversion Principle Clean Arch.png]]


Now we can see that no inner layer depends on any outer layer. Rather, the outer layer depends on the inner layer.

> _So why should the outer layer depend on the inner layer but not the other way around?_

Imagine you’re in a hotel. We want the hotel to serve us what we want, but not what they offer right?. The same thing is happening here, we want the DB to give the data the application needs but not the data it has.

Application orders what data it wants and it doesn’t care how DB or API prepares the data. This way, the application doesn’t depend on DB or API. If we need/want to change the DB or API Schema in the future, we can simply change it. As far as it gives what the application asks for, the application doesn’t even know the change in DB or API.

Also, the single-way dependency rule saves the application from the deadlock state. i.e. imagine in a 2 layer architecture, the first layer depends on the second layer, and the second layer depends on the first layer. In such a case, If we need to change anything in the first layer, it breaks the second layer. If we need to change anything in the second layer, it breaks the first layer. This can be rejected by following the deadlock state.




See this medium article - [The Clean Architecture — Beginner’s Guide](https://betterprogramming.pub/the-clean-architecture-beginners-guide-e4b7058c1165) 


***

Status: #✅ 

Tags: [[wip]] - [[MOC - Coding Notes]] - [[tech]]

Date: ==[[2022-02-13]]==

