# DDD - Domain Driven Design (Currently Studying)

A way to develop a software focusing on the core of the application (the domain). Aiming understand the rules, processes and complexity, separating other complex points that commonly are added during the development

- It has to be applied only in complex softwares. Simple software is not necessary.
- Domain: The core of the software. Main function that will be developed (general vision).
```
  - Peace is more important: Core Domain. If it does not exist, there is no sense to the rest subdomains exist. 
  - The core usually is the differential competitive of the company
```
- Subdomain: separated pieces that form the domain (details). smaller domains.
```
  - Support subdomain: supports the core. It does the operation for the core domain. Ex: Center Distribution of cars (car factory, car is the core, distribution is the support domain) 
  - Generic subdomain: auxiliary. It is not a competitive differential. Ex: generate tickets for the clients to pay something
```
- universal language: ubiquitous language. The collaborators have to communicate with one unique language. When you listen to someone speaking, everyone has to understand. Ex: Specifications, even the variable names. Everybody has to know the business
- Helps to create a design a strategic design (Bounded Contexts)
- Bounded Contexts: it is an explicit boundary within which a domain model exists. Inside it, everyone has to communicate with ubiquitous language.
```
Ex: When you have two words with two different meanings, you are in a different bounded context. Ex: Sell "Ticket", Support "Ticket". Ticket is a different entity for each context.
  Ex2: When you have two different words that have the same meaning, probably you are in a different context, too.
```
- Clarify the business complexity and the technical complexity 
- Context Mapping: ( A project that facilitates these concepts with some images and a miro to start de design of the contexts: https://github.com/ddd-crew/context-mapping )
```
  - Partnership: Communication between two teams
  - Shared Kernel: Ex: a lib shared between teams
  - Customer-Supplier Development: One service consumes and the other supply
  - Conformist: Ex: An external Service that defines something that cannot be changed
  - Anticorruption Layer: A interface layer that helps the conformist
  - Open host service: one context supplies a service with some protocol (rest, grpc, etc.)
  - Publisher language: The consumer has to use the same language to consume the service
  - Separate ways: the contexts do not communicate with each other
  - Big Ball of Mud: a big system with complex things mixed and you have to deal with it
```

### Branas

- The domain has to be distributed the complexity in different domain objects

#### Transaction Script VS Domain Model

- Transaction Script: Accumullate too much responsability. In it, you only have behavior. (You separate the caracteristics of the object from the behavior of the object). Uou use a Oriented Language as a Procedure Language.
  * Ex: Service
  * OBS: It is not a problem. It is only a form of how you express your design.
- Domain Model: You put the express everything with objects (behavior and caracteristics). Espressing business rules more complexity. A Object that incorporate Behaviors and Caracteristics, expressing business rules more complex, allowing efficient distribution.
  * Ex: Use Case (orchestration of the domain) with a Domain Object (Ex: Domain Service)

#### Tactical Modeling and Patterns

- Get the business rule and distribute it with these separation:

- **Entity**: Represent a business rule. Abstract independent rules. They have identity, state and suffer mutation in time.
  * Is has identity and change the status as pass the time.
  * Ex: A Buy in an online store. It can change status to aproved, disaproved, delivered, etc. (UUID could be the identity. State can be the status. Mutation could be any value in it). It has to be Identity to have rastreability.
- **Value Objects**: There is an independent busiless rule, too. However, it protects the value to be consistent. You do not modify the value. It is always imutable.
    * Represents one or more values, are imutable and when instantiates, they are reinstantiates
    * Ex: CPF, Password, Color, Coordanate, Email, etc. (you have to send the rule to inside this object. Ex: a regex of cpf in the constructor inside of the Value Object "CPF)
- **Domain Service**: Does specific tasks in the domain that do not have state. This rule has to not fit in another place. If there is an Value Object that has more sense, put it in Value Object and does not separate it in "Domain Service".
    * Abstracts business rules that are not a part of an entity or a value object
    * Ex: DistanceCalculator: get two coordanates and calculate. (You can put it inside the Value Object "Ride" or put it outsite as "Domain Service".
    * OBS: Take care to not use Domain Service for everything.
- **Aggregate**: Is is the relashionship of Domain Objects (Entities and Value Objects) leadered by an Root Entity.
    * Ex: Account -> Account (Aggregate Root, Entity), Name (VO), Email (VO), Cpf (VO), CarPlate (VO)
    * OBS: One aggregate can reference another aggregate
    * OBS: The entity always will be a part of an Aggregate
    * OBS: There is the Aggregate Root (
    * OBS: Create little aggregates.
    * OBS: Reference it with identities (IDs)
    * OBS: When you feel that the aggregate is big,
    * OBS: Aggregates dos not have to reflect the database. They are different things.
- **Repositories**: Do the persistence of aggregates.
