# Teaching-HEIGVD-AMT-2018-Project


## WP 2: the core engine and its robust REST APIs

### Overview

In WP1, we have implemented features that any developer-oriented SaaS service has to provide. The developer of a gamified application can register (create an account). He can then create one or more applications for which he will later define gamification rules.

In WP2, the goal is to implement a simple **domain model** (a set of collaborating business classes). It is then to expose this domain model via a REST API. But the goal is not only to comply with functional requirements. As it was the case for WP1, we also want to address non-functional requirements (e.g. scalability, reliability, etc.). We will therefore implement automated testing.

### A simple domain model for gamification

A gamification engine can be very rich and support a lot of game mechanics. Because we have limited time, we will implement a simple model that will be enough to demonstrate the concept of the service. The model has already been presented during the lectures (see slides), but here are key elements:

- Every gamified application will send a stream of `events` to the gamification platform. Sending an event to the platform is a way to say: "This user has done this action". A gamified GAPS could send an event to say that "Bob has obtained a grade for a course". A gamified Stackoverflow could send an event to say that "Alice has answered a question about Javascript". It is a good idea to include a map of "properties" in the event data structure, so that every application can freely define its own event attributes. At the minimum, an event should have a user id (the user id in the gamified application), a timestamp, an event type and a map of properties. In terms of REST APIs, there should be a `/events` endpoint, accepting `POST` requests.
- The developer doing the configuration for a gamified application should be able to configure two game mechanics: **badges** and **point scales**. For a gamified application, the developer could decide that he wants a badge named "Newbie" and another one named "Guru". The developer could decide that he wants a point scale named "Experience" and another one named "Creativity". The key point is that badges and point scales are not defined globally. They are defined in the scope of one gamified application. In other words, every application defines its own list of badges and scales. In terms of REST APIs, there should be a `/badges `and a `/pointScales` endpoints for the usual CRUD operations (but think about the implications of `DELETE` and make an informed decision whether to support it or not).
- We also need some sort of rule system to make the link between the stream of events and the award of badges and points to users. Simply stated, the developer must be able to state "**If** an event with these properties comes in, **then** give this reward to the user". The rule system is where we can be very creative and more or less flexible. It is actually simple to implement a basic system, for instance by working with the event type property in the event.

### Implementation choices

- The back-end is implemented with **Springboot**. This framework integrates **Spring MVC** (for the controllers in the presentation tier) and **Spring Data** (for the DAOs in the integration tier). Spring Data supports different database technologies. One of them is the Java Persistence API (JPA), which we have seen in the lectures and which is a good choice for the project. But if you will like working with Mongo DB, it is also possible.
- On the testing side, we will use the Behaviour Driven Development (**BDD**) approach and write acceptance scenarios in natural language. We will use the **Gherkin** language for this purpose and its implementation by the tool **CucumberJVM**.
- We also need to pay attention to **concurrency issues**. Remember that gamified applications may send high-volume streams of events. It is possible that two events for the same user arrive at the same time. A particular case is when the first two events for a user arrive at the same time (because on the gamification engine side, the user object is created when the first event arrives).

### Specifications: functional and non-functional requirements

| Description                                                  | Functional / non-functional | Points |
| ------------------------------------------------------------ | --------------------------- | ------ |
| As an application developer with an **API client**, I can perform CRUD operations on **badges**. When I ask the list of badges, I only see the badges of my application (thanks to the API key provided in the request). | FR                          | 1.0 |
| As an application developer with an **API client**, I can perform CRUD operations on **point scales**. When I ask the list of badges, I only see the badges of my application (thanks to the API key provided in the request). | FR |1.0|
| As an application developer with an **API client**, I can perform CRUD operations on **rules**. When I ask the list of badges, I only see the badges of my application (thanks to the API key provided in the request). *The simplest feature is to support stateless rules that evaluate only the event type*. | FR                          | 2.0 |
| As a gamified application, I can send a stream of application events. The correct rules are evaluated and the correct rewards are given to the user. | FR                          | 4.0 |
| As an application developer, I can define rules that evaluate more than the event type: I can write expressions that evaluate the properties attached to the event (e.g. give the badge if the event is type "askQuestion" and if event has a property "difficulty" with a value higher than 5). | FR |2.0|
| As an application developer, I can define rules that allow me manage some state (e.g. give the badge after 5 events of type "askQuestion") have been asked. | FR |2.0|
| | **Total** | **12.0** |
| As a **spiritual guide**, I can clone the repo, move to a documented repository and type `docker-compose up` to start the system. I also have instructions for running the automated tests. | NFR-maintainability         | 1.0 |
| As a **spiritual guide**, I have complete Cucumber tests for the badges, point scales and rules endpoints. | NFR-testability             | 3.0 |
| As a **spiritual guide**, I have advanced Cucumber tests for the /events endpoints, that validate the business logic (has the rule been triggered and are the side effects correct?) | NFR-testability             | 4.0 |
| As a **spiritual guide**, I have load testing script (e.g. JMeter) that I can use to evaluate if the engine behaves correctly when several events for the same user arrive at the same time. The experiments that have been done are well documented (e.g. under which conditions did we have a bug, how did we solve it, etc.) | NFR-reliability | 4.0 |
|                                                              | **Total** | **12.0** |



### Grading

Number of points / 24.0 * 5 + 1.





