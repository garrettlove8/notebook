---
title: "C3: Creation Patterns"
weight: 1
draft: false
menu:
  docs:
    parent: design-patterns
    weight: 2
---

# Chapter 3. Creational Patterns
- Abstraction that hides an object's creation, composition, and representation details
- Class creational patterns will use change the class being instantiated
- Object creational patterns will handoff instantiation to another object
- Two themes in creational patterns
    - They hold the knowledge about which classes the system uses
    - They hide how the class are created and put together
- Sometime these patterns can compete with one another and other times they can work in a complimentary form
- Creational patterns provide ways to remove remove hard-coded references to classes from the code that needs to instantiate them

## Object Creational: Abstract Factory
### My Notes
- Abstract factory is for creating objects that are within a certain category
- The abstract factory will then use each class' own factory (called a `concrete factory`) to actually instantiate a given object

### Intent
- Interface for creating related or dependent object without stating their concrete classes

### Also Known As
- Kit

### Motivation
- Make the client's creation of new objects independent of the underlying concrete classes being used
- In Go this would be an interface to allow the client to access the desired functionality
- Classes can define their own implementation and outcomes but as long as they implement the abstract factory interface they can be used by the client
- Clients only have to commit to an interface, not one single concrete class

### Applicability (when to use it)
- A system should be independent of how itc products are created, composed, and respresented
- A system should be configured with one of many families of products
- A family of related product objects is designed to be used together and you want to enforce this constraint
- You want to provide a library and only reveal their interface, not their implementation

### Collaborations
- An abstract factory defers creation of a product object to its concrete factories

### Consequences (Pros & Cons)
1. Isolates concrete classes
    - Helps control what types of objects an application can create
    - The client manipulates instances through their interfaces, instead of directly
2. Make exchanging product families easy
    - The abstract factory is only located in a single place within the application, so switching it out is simple
    - Since an abstract factory creates an entire family of object types, when it is switched out for a different abstract factory, all the products that were created will switch at the same time
3. Promotes consistency
    - If objects in a family are designed to work together it is important that only objects from that family be used at any given time
4. Supporting new kinds of products is difficult
    - Since the available products are fixed via the abstract factory class, adding products would invlove changing the class and thus would be difficult
    - `Note: This doesn't sound like something Go suffers from since it doesn't have classes and interfaces are independent from the type`

### Implementation
- Useful techniques for implementing this pattern

1. Use singletons
    - An application likely only needs one instance of a concrete factory per product family
    - Ex: Connection to a database
2. Creating the products
    - The abstract factory is only the interface which the client uses
    - Each type within the product family should define its own factory method for creating itself
    - 