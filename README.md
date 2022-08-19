# DDD
Domain Driven Design

A way to develop a software focusing in the core of the application (the domain). Aiming understand the rules, proccesses and complexity, separating other complex points that commonly are added during the development

- It has to be applied only in complex softwares. A simple software it is not necessary.
- Domain: The core of the software. Main function that will be developed (general vision).
```
  - Peace more important: Core Domain. If it does not exists, there is no sense to the rest subdomains exist. 
  - The core usually is the differential competitive of the company
```
- Subdomain: separated peaces that forms the domain (details). smaller domains.
```
  - Support subdomain: supports the core. It does the opperation for the core domain. Ex: Center Distribbution of cars (car factory, car is the core, distribution is the support domain) 
  - Generic subdomain: auxiliary. It is not a competitive differential. Ex: generate tickets for the clients to pay something
```
- universal language: ubiquous language. The collaborators have to comunicate with one unique language. When you listen to someone speaking, everyone has to understand. Ex: Specifications, even the variable names. Everybody has to know the business
- Helps to create a design a strategic design (Bounded Contexts)
- Bounded Contexts: it is an explicity boundary within which a domain model exists. Inside it, everyone has to communicate with ubiquous language.
```
Ex: When you have two words with two different meaning, you are in a different bounded context. Ex: Sell "Ticket", Support "Ticket". Ticket is a different entity for each context.
  Ex2: When you have two different words that have the same meaning, probably you are in a different context, too.
```
- Clarify the business complexity and the technical complexity 
- Context Mapping: ( A project that facilitates these conecpts with some images and a miro to start de design of the contexts: https://github.com/ddd-crew/context-mapping )
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
