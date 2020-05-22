# Use <Option> Architecture for backends

* Status: proposed
* Deciders: [list everyone involved in the decision]
* Date: [YYYY-MM-DD]

## Context and Problem Statement

## Considered Options

* Layered Architecture
* Hexagonal Architecture

### Layered Architecture

The layered architecture, otherwise known as n-tier architecture, is one of the most common architecture pattern. It is the de facto standard for most backend application and is therefore widely known by most architects, designers and developers. Although the layered architecture doesn't specify the number of layers most application consist of three layers: presentation, business logic and persistence. Layers are stacked upon each other meaning that a request must move from layer to layer. For example a request originating from the presentation layer must first go through the business layer before it goes to the persistence layer. This is enforced by the rule that a component MUST only access components in the same layer or the layer below.

### Hexagonal Architecture

The Hexagonal Architecture is an implementation of the Clean Architecture advocated by Robert C. Martin in his book with the same title. In his definition of a clean architecture the business rules are testable by design and independent of frameworks, databases, UI technologies and other external applications or interfaces. That implies that the domain code must not have any outward facing dependencies. Further all dependencies point toward the domain code.

The layers in this architecture are wrapped around each other in concentric circles. The main rule in such an architecture is the Dependency Rule, which states that all dependencies between those layers must point inward.

The core of the architecture contains the domain entities which are accessed by the surrounding use cases. The use cases are what we call services the layered architecture. Use cases are restricted in their width allowing them only to have a single responsibility (i.e. a single reason to change).

Around this domain core we can find all the other components of our application that connect our application to external systems. Typically this support means providing persistence or providing a user interface.

Because the domain code has no knowledge about frameworks used in the supporting layers, it cannot contain any code specific to those frameworks and can  concentrate on the business rules.

The Hexagonal Architecture is represented as a hexagon, giving this architecture style it’s name. The hexagon shape, however, has no meaning it is simply used instead of the common rectangle to show that an application can have more than 4 sides connecting it to other systems or adapters. It might as well be visualised as an octagon.

Within the hexagon, we find our domain entities and the use cases that work with them. Note that the hexagon has no outgoing dependencies, so that the Dependency Rule from Martin’s Clean Architecture holds true. Instead, all dependencies point towards the center.

Outside of the hexagon, we find various adapters that interact with the application. There might be a web adapter that interacts with a web browser, some adapters interacting with external systems and an adapter that interacts with a database.

The adapters on the left side are adapters that drive our application (because they call our application core) while the adapters on the right side are driven by our application (because they are called by our application core).

To allow communication between the application core and the adapters, the application core provides specific ports. For driving adapters, such a port might be an interface that is implemented by one of the use case classes in the core and called by the adapter. For a driven adapter, it might be an interface that is implemented by the adapter and called by the core.

Due to its central concepts this architecture style is also known as a "Ports and Adapters" architecture.

## Decision Outcome

TBD

## Pros and Cons of the Options

### Layered Architecture

* Good, because it can quickly adapt to changing requirements and external factors.
* Good, because it allows to switch a layer without affecting the others.
* Good, because it allows to add new features alongside existing one's.
* Good, because each layer has a clear responsibility.

* Bad, because it's prone for bad habits to creep in.

**Promotes Database-Driven Design**

**Why?** The flow of dependencies in a layered architecture points downwards, usually towards the persistence layer.

**But how is that bad?** We are modeling business behaviour not state in our application that drives the business. Hence we should build the domain logic before anything else because only then we can figure out if we understood it correctly. So why should the database be the foundation of our architecture and not the domain?

The driving force for this bad habit are ORM frameworks which tempt us to mix business rules with persistence because they manage ORM-entities. Thereby virtually fusing the two layers which makes it hard to change one without the other.

* Bad, because it's prone for shortcuts.

**Pushing components**. Remember the rule that components must only access other components in the same layer or the layer below. So if we need access to a component above just push it a layer down and problem solved! Doing this once may be okay especially with looming deadlines but this opens the door for doing it again and again. Also if someone else was allowed, why shouldn't I?

**Skipping layers**. Usually projects start to take shortcuts from the presentation layer to the persistence layer for simple CRUD use cases which eventually causes more and more domain logic to be integrated into the presentation layer.

This habit makes testing more difficult because more mocking is required in the unit tests.

* Bad, because it hides use cases

In general we will more often change existing use cases instead of creating new ones. But where to we add or change functionality. Due to the bad habits 2+3 this MAY be tricky. Further layers don't make any rules on the width of a service which often leads to services handling multiple use cases. This makes it hard to find the service responsible for a use case. It makes it even harder to write tests because we need to mock everything not involved in the use case under test.

* Bad, because it makes parallel work difficult.

Working on different features in a broad services does leads inevitable to conflicts.

Different developers working on different layers doesn't work as well because the topmost layer depends on the layer below and so on. This could be solved by interfaces but only if we prevent Database-Driven Design. The Domain layer needs to go first.

### Hexagonal Architecture

* Good, because it promotes Domain-Driven Development.

* Good, because ports and adapters are interchangeable, just swap them.

Same is theoretically true for the layered architecture but reality usually disappoints.

* Good, because decisions can be deferred until the very end.

We can focus on the domain logic without worrying about UI or persistence concerns.

* Good, because features can be implemented faster.

Use cases are first class citizen of the hexagonal architecture hence there's no question where to add or modify a feature.

When you know how to convert requests and responses from the outside world it's really easy to implement features. This architecture facilitates us with the separation of concerns which allows us to focus on one specific task which allows us to develop faster.

* Bad, because [YAGNI](https://en.wikipedia.org/wiki/You_aren't_gonna_need_it).

The main concern is that you'll write a lot of boilerplate mapping code to facility the separation of the layers. This can be mitigated by applying guidelines for different types of use cases (i.e. modifying, querying or CRUD). Depending on the type a less or more strict mapping strategy may be chosen.

Most use case start as simple CRUD features and then evolve into something more complex. When deciding to use the hexagonal architecture additional ADRs are required to specify the mapping behaviour of different use case types.

## Links

* [Implement rich/anemic domain models]()
* [Use "no-mapping" strategy for queries]()
* [Use "full-mapping" strategy for modifying use case between web and application layer]()
* [Use "no-mapping" strategy for modifying use case between application and persistence layer]()
