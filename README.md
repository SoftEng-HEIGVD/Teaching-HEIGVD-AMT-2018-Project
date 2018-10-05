# Teaching-HEIGVD-AMT-2018-Project
## Introduction

The first goal of the project is to design and build a **gamification engine**, which you will offer to third-party developers in a **SaaS** model. The second goal of the project is to design and implement a **demonstration** to show you system in action. 

The project will be delivered in 3 phases, with corresponding to **3 Work Packages** (WPs). Every WP will be evaluated with a grade, with the following weights:

| WP # | WP Title                                                     | Weight | Deadline                    |
| ---- | ------------------------------------------------------------ | ------ | --------------------------- |
| 1    | UI to manage **developer accounts** and **applications** (with bare-bone Java EE technologies). Implementation of presentation, business, integration and resource tiers. | 35%    | Monday, November 5th 2018   |
| 2    | **Game engine** with REST APIs (UI optional).                | 40%    | Monday, December  17th 2018 |
| 3    | **Demonstration** showing that it is possible to gamify an existing application. End-to-end scenario and presentation of information in a UI. | 25%    | Monday, January 14th 2019   |

### Gamification

Gamification is defined as the application of **game principles** in contexts that are not related to games: healthcare, education, commerce, etc. Very often, gamification is used to lead users to adopt a certain **behaviour**.  

In healthcare, the goal might be to encourage users to exercise more or to quit smoking. In education, the goal might be to encourage students to interact with each other. In commerce, the goal might be to lead consumers to buy more products. The game principles are mechanics like reputation points, badges, achievements, etc. 

One service that makes use of gamification is [**Stackoverflow**](https://stackoverflow.com/users/1341338/olivier-liechti). When registered users ask questions, provide answers, add comments, they are rewarded with points. They can also be punished and have points deducted from their reputation. Users can also unlock badges by performing certain sequences of actions. On top of the raw metrics, Stackoverflow provides analytics dashboards that show how users compare with each other (e.g. rankings in various timeframes) and how the reputation has evolved over time.

### Gamification engine

From an architecture point of view, the creators of Stackoverflow perhaps have implemented everything by themselves. In other words, they have implemented the core business logic of their domain (collaborative Q&As) and also implemented the gamification features. We do not know the internal architecture of their software, but we can imagine a big Java EE application with services that deal with these two aspects.

The same could be true of other applications. Think of a gamified academic planning system, a gamified e-commerce site, a gamified sports tracker. They are different applications but share very similar needs in terms of gamification: managing reputation points, managing badges, etc. Hence, there is an opportunity to **extract this generic behaviour in a service**. This is what we call a **gamification engine**.

As a matter of fact, there are different ways to make generic behaviour available to several applications:

* One way is to develop libraries and frameworks. In this case, third-party developers package what we provide with their code and create a complete system. In Java EE terms, this can be done by adding .jar files (the libraries) to the .war file (the whole application). This is **NOT** the approach that we will take in this project. 
* Instead, we will use the cloud approach and deliver our software as an online service (SaaS). This means that we will not only write code. We will also deploy and operate it. The third-party application developers will be able to use our gamification engine by making call to a REST API that we will expose.

### REST APIs exposed by the gamification engine

It is too early to get into the specifics of the REST API that our gamification engine should expose. But at a high-level:

* There should a first endpoint that applications use to notify us that one of their user has done something in their application. Something like `/events`. This is the endpoint that Stackoverflow would use to tell us that the event "Bob has answered one question" has occurred. This is the same endpoint that an e-commerce site would use to tell us that "Alice has bought a product that costs more than 200 CHF". The payloads POSTed on this endpoint should be event objects, with properties.
* There should be a second endpoint, which application developers use to configure the rules. Something like `/rules`. How to express and evaluate rules is a question that you will have to answer during the second part of the semester. In simple terms, a rule states that "IF an event comes in with certain properties, THEN do this action". An action might be to add or remove reputation points, to award a badge, etc. 
* There should be other endpoints, to manage accounts, applications, etc.

## Work packages

The work pages are described in these pages:

* [Work package 1](WP1.md)
* Work Package 2 (coming later)
* Work Package 3 (coming later)

## Groups

**You will work on the project in teams of 3-4 students.** Please register [here](https://goo.gl/forms/hcmGxuVB3F6VqSBR2) and if you use a private repo, please invite us.

