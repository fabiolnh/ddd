# DDD
Domain Driven Design

A way to develop a software focusing in the core of the application (the domain). Aiming understand the rules, proccesses and complexity, separating other complex points that commonly are added during the development

1) It has to be applied only in complex softwares. A simple software it is not necessary.
2) Domain: The core of the software. Main function that will be developed (general vision).
  - Peace more important: Core Domain. If it does not exists, there is no sense to the rest subdomains exist. 
  - The core usually is the differential competitive of the company
3) Subdomain: separated peaces that forms the domain (details). smaller domains.
  - Support subdomain: supports the core. It does the opperation for the core domain. Ex: Center Distribbution of cars (car factory, car is the core, distribution is the support domain) 
  - Generic subdomain: auxiliary. It is not a competitive differential. Ex: generate tickets for the clients to pay something
4) universal language: ubiquous language. The collaborators have to comunicate with one unique language. When you listen to someone speaking, everyone has to understand
  Ex: Specifications, even the variable names. Everybody has to know the business
5) Helps to create a design a strategic design (Bounded Contexts)
6) Clarify the business complexity and the technical complexity 
