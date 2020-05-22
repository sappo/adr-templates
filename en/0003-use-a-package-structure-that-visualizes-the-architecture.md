# Use package structure that visualizes the architecture

## Context and Problem Statement

Wouldn't it be nice to recognize the architecture just by looking at the code?

## Considered Options

* Organize by Layer
* Organize by Feature
* Organize by Expressive Package Structure

### Organize by Layer

```plantuml
skinparam Legend {
    BackgroundColor transparent
    BorderColor transparent
    FontName "Noto Serif", "DejaVu Serif", serif
    FontSize 17
}
legend
application
|_ domain
  |_ Model1
  |_ Model2
  |_ Service
  |_ IRepository
|_ web
  |_ Controller
|_ persistance
  |_ RepositoryImpl
end legend
```

For each of our layers e.g. web, domain and persistence we have a dedicated package.

### Organize by Feature

```plantuml
skinparam Legend {
    BackgroundColor transparent
    BorderColor transparent
    FontName "Noto Serif", "DejaVu Serif", serif
    FontSize 17
}
legend
application
|_ feature
  |_ Model1
  |_ Model2
  |_ Service
  |_ Controller
  |_ IRepository
  |_ RepositoryImpl
end legend
```

Each new group of features will get a new high-level package where all belonging classes will be located.

### Organize by Expressive Package Structure

```plantuml
skinparam Legend {
    BackgroundColor transparent
    BorderColor transparent
    FontName "Noto Serif", "DejaVu Serif", serif
    FontSize 17
}
legend
application
|_ feature
  |_ adapter
    |_ in
      |_ web
	    |_ Controller
    |_ out
      |_ persistance
	    |_ RepositoryImpl
  |_ domain
    |_ Model1
    |_ Model2
  |_ application
    |_ Service
    |_ port
      |_ in
        |_ UseCaseXyz
      |_ out
        |_ SaveModel
        |_ UpdateModel
        |_ DeleteModel
end legend
```

Each element of the architecture can directly be mapped to one of the packages. On the highest level, we again have a package named after the feature, indicating that this is the module implementing the use cases around this feature.

On the next level, we have the `domain` package containing our domain model. The `application` package contains a service layer around this domain model.

The `port` package contains the incoming ports that drive the application and the outgoing ports that are driven by the application. Incoming and outgoing ports are organized into `in` and `out` packages respectively to increase the expressiveness even further.

The `adapter` package contains the incoming adapters (that call the application layer’s incoming ports) and the outgoing adapters (that provide implementations for the application layer's outgoing ports). Incoming and outgoing adapters are organized into `in` and `out` packages respectively to increase the expressiveness even further.

## Decision Outcome

TBD

## Pros and Cons of the Options

### Organize by Layer

* Bad, because we have no package boundary between functional slices or features of our application.

Without further structure, this might quickly become a mess of classes leading to unwanted side effects between supposedly unrelated features of the application.

* Bad, because we can’t see which use cases our application provides.
* Bad, because our architecture is not visible.

### Organize by Feature

* Good, because package boundaries, combined with package-private visibility, enable us to avoid unwanted dependencies between features.
* Bad, because the package-by-feature approach makes our architecture even less visible than the package-by-layer approach.

We have no package names to identify our adapters, and we still don’t see the incoming and outgoing ports.

* Bad, because we cannot use package-private visibility to protect the domain code from accidental dependencies to persistence code.

### Organize by Expressive Package Structure

* Good, because each element of the architecture can directly be mapped to one of the packages.

This especially eases the exploration phase for new developers.

* Good, because it minimizes the “architecture/code gap” or “model/code gap”.
* Good, because it promotes active thinking about the architecture.
* Good, because adapters are unknown to our application. So no accidental access to adapters by design.
* Good, because it directly maps to DDD concepts:
  * High-level package = bounded context.
  * Dedicated entry and exit points (the ports) to communicate with other bounded contexts.
  * Domain package to focus on the domain model we want, using all the tools DDD provides us.
* Bad, because there are a lot of packages to maintain.
