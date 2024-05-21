# DDD - Domain Driven Design

A way to develop a software focusing on the core of the application (the domain). Aiming understand the rules, processes and complexity, separating other complex points that commonly are added during the development

- It has to be applied only in complex softwares. Simple software is not necessary.
- Domain: The core of the software. Main function that will be developed (general vision).
```
  - Peace is more important: Core Domain. If it does not exist, there is no sense to the rest of the subdomains exist. 
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

- Transaction Script: Accumulate too much responsibility. In it, you only have behavior. (You separate the characteristics of the object from the behavior of the object). You use a Oriented Language as a Procedure Language.
  * Ex: Service in Spring
  * OBS: It is not a problem. It is only a form of how you express your design.
- Domain Model: You express everything with objects (behavior and characteristics). An Object that incorporate Behaviors and Characteristics, expressing business rules more complex, allowing efficient distribution.
  * Ex: Use Case (orchestration of the domain) with a Domain Object (Ex: Domain Service)

#### Tactical Modeling and Patterns

- Tactical: How to implement the Domain layer
- Ubiquitous Language: Not always the demands are clear. (what is coded, what is written in the documentation and what is talked about). You stress the communication to get the ideal terms.
- OBS: If you want to separate the Domain Objects in the Clean Architecture Design, you put it in the "Entity" Layer (Enterprise Business Rules).  

The concept of Tactical is: Get the business rules and distribute it with these separation\: 

1) **Entity**: Represent a business rule. Abstract independent rules. They have IDENTITY, STATE and suffer MUTATION in time.
    * It has an identity and change the status as pass the time.
    * Ex: A Purchase in an online store. It can change status to approved, disapproved, delivered, canceled, in transit, etc. (UUID could be the identity. State can be the status. Mutation could be any value in it).
    * Ex2: Account (It can change the status to blocked, password changed, etc)
    * Ex3: Ride (It can change the status to in transit, finished, etc. after finished, the traffic value is updated)
2) **Value Objects**: There is an independent business rule, too. However, it protects the value to be consistent. You do not modify the value. It is always immutable, the change implics in its substitution. They are identified by its value, not the identity (as Entity is). You do not modify the Value, you re-instantiate it.
    * Represents one or more values, are immutable and when instantiates, they are re instantiated
    * Value Objects can be independent. You can use it in other Entities
    * Ex: CPF, Password, Color, Coordinate, Email, etc. (you have to send the rule to inside this object. Ex: a regex of cpf in the constructor inside of the Value Object "CPF). Then, everytime that you want to change its value, you instantiate it again.
3) **Domain Service**: Does specific tasks in the domain that do not have state.
    * It is indicated when an operation that you want to execute **does not fit in an Entity or in a Value Object**.
    * This rule has to not fit in another place. If there is a Value Object that has more sense, put it in the Value Object and does not separate it in "Domain Service".
    * Abstracts business rules that are not a part of an entity or a value object
    * Ex: DistanceCalculator: get two coordinates and calculate. (You can put it inside the Value Object "Ride" or put it outside as "Domain Service".
    * Ex2: TokenGenerator: generate a token according to the email.
    * OBS: Take care to not use Domain Service for everything.
4) **Aggregate**: It is a grouping, or cluster, of domain objects, such as Entities and Value Objects, establishing the relationship between them. ("Virtual" concept)
    * It is the relationship of Domain Objects (Entities and Value Objects) leadered by an Entity Root.
    * All the operations are done by the root (that is an Entity or Aggregate Root <AR> )
    * Good Practices:
      * Start always with little Aggregates. (start with only one entity and amplify according to the needs.
      * Reference other aggregates by identities. (ex: "Position" can have only the "rideId")
    * Ex: Account -> Account (Aggregate Root, Entity), Name (VO), Email (VO), Cpf (VO), CarPlate (VO)
    * Ex: Ride -> Ride (Aggregate Root, Entity), RideStatus (VO), Coordinate (VO), Segment (VO)
    * Ex: Position -> Position (Aggregate Root, Entity), Coord (VO), Date (VO)
    * OBS: Big Aggregates can overcharge memory. Entities are not always inside of an Entity, because usually are not the same model in the database. Usually we separate it to not overcharge the database, too.
    * The goal of aggregate is to balance the preservation of invariants with resource consuming.
    * Ex: "Position" inside "Ride" (everytime you would have to get the position when you get the Ride. It would consume too much resource in the database and memory). So, you separate it. 
    * OBS:
      * Aggregate Root or Entity Root are the same
      * One aggregate can reference another (with identity. Ex: rideId)
      * An Entity always will be a part of an Aggregate (if not, such as an entity without a VO, the own Entity is an Aggregate)
      * The Aggregate defines the repository. (we always have the relation of 1:1 from Aggregate to Repository)
      * If the repository is hard to implement, maybe the Aggregate is too big and can be separated.
      * Aggregates does not have to reflect the database. They are different things. The relationships of database were done by normalization to facilitate persistence.
      * An Entity that is a part of an Aggregate can be a part of another Aggregate? It is not good. No sense.
      * You you want to separate into directories in your project, there is no directory for "aggregates"
5) **Repositories**: It is an **extension** of a domain responsible for the persistence of aggregates, separating the infrastructure. (it is not a domain object, it is only an extension. It is separated)
     * It is the mediator of persistence of aggregates
     * The repository persists the aggregate. (not the VO, and not the Entity. Always the aggregate as a whole)
     * The repository deals with the **whole Aggregate**. While the DAO has a granularity defined (DAO does not have compromised with the Domain Object (different from Repository).
     * Questions:
       * Only a part of an Aggregate changed, can I persist only this part? The persistence is always of the whole Aggregate, however, the repository can decide what registries in the database have to be updated. OBS: The role of the Repository is to always receive and return the whole domain object.
       * Can I get only a part of an Aggregate? If you are in this situation, this means that the Aggregate is too big and it could be separated into small parts.
       * Can I use Lazy/Loading in the Aggregate?  If you use it, you will get only a part of the aggregate. With it, you can show an invalid result. So, do not do it. Or you restore the whole object or you do not restore!
       * Is it possible to use different filters to get an Aggregate? You can use the filter (resulting in one or more aggregates).
       * Can we use a repository to generate a Report or a Statistic? The problem is that the repository has to respect the Aggregate. Renderize it with a repository can be too complex. Prefer to use CQRS with separate queries. 
     * Do the persistence of aggregates. It should return the Entity, too.
6) **Factories**: Allow the creation of some domain objects following some criteria. 

#### Strategic Modeling

- Understanding the Domain as a whole and decoupling this Domain in small parts (knowledge areas).
- You do not only see the specific project, you see the organization as a whole.
- It is always applicable, different from Tactical
- Domain: "Is the problem" of what we need to solve
- We need to watch the whole
- Every domain has to have a subdomain
- Subdomains: (Knowledge Areas)
  * **Core or Basic**: More important. Brings value to the company. You put all the force to it.
  * **Support**: Complementary with the Core. Without it there is no success in the business
  * **Generic**: can be delegated to another company

  ![Branas](https://github.com/fabiolnh/ddd/blob/main/assets/Subdomains.png)

- Bounded Context: Is the project (Code Based) (in DDD, it does not worry about resources. Ex: a bounded context can be a microservice, but DDD does not know what microservice is. But DDD, it does not matter. DDD is worried about the Domain). It is a way to modularize. To reduce accouplement in the code.
- Ex: Online Product Sell System
- OBS: Subdomain and Bounder Context are not always are 1:1
  ![Branas](https://github.com/fabiolnh/ddd/blob/main/assets/Subdomains%20Decomposion.png)

- From book:
  ![Branas](https://github.com/fabiolnh/ddd/blob/main/assets/Example.png)

- Example of a system:
  ![Branas](https://github.com/fabiolnh/ddd/blob/main/assets/System.png)

  * Upstream: It is what you are accessing
  * Downstream: It is who is accessing
  * (Downstream consumes Upstream)
- Bounded context is a form to modularize, to reduce the accouplement in the code
- Shared Kernel: Shared Code. Ex: a lib shared between teams
- Anticorruption Layer: Absorb the difference that comes from an external layer. Ex: Transform something from external to something to ubiquitous language.
- OBS: Not all bounded context need to be develop as the same (ex: not all has to be developed with a tactical model)
- OBS: The border of The Bounded Context is excellent to define a microservice
- Gateway: A pattern to access another Bounded Context.
