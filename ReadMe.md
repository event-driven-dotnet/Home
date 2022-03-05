# Event Driven .NET

An event-driven microservices platform for .NET

## Introduction

[Microservices](https://en.wikipedia.org/wiki/Microservices) is a popular paradigm for building highly scalable and flexible distributed systems based on Cloud computing. While the *what* of microservices is fairly unambiguous, the *how* is replete with pitfalls and perils. Many teams embark on the road of good intentions only to find themselves mired in a quicksand of tightly coupled systems -- the [distributed monolith](https://www.simplethread.com/youre-not-actually-building-microservices/) -- with few of the benefits but most of the difficulties of a traditional monolith. Much of this stems from using synchronous REST calls as the primary means of inter-service communication, which can result in systems that are brittle and inflexible, with teams that block one another and services that must be deployed and scaled as a single unit.

The purpose of [Event Driven .NET](https://github.com/event-driven-dotnet) is to provide a platform where .NET developers can build loosely coupled distributed systems consisting of services that can be deployed and scaled independently, replacing point-to-point communication with an [event bus abstraction](https://github.com/event-driven-dotnet/EventDriven.EventBus.Abstractions) that promotes asynchronous communication using a message broker.

## Strategy

The mission of **Event Driven .NET** is to provide a multi-layered platform upon which .NET developers can build distributed applications that deliver on the promise of microservices by enabling multiple teams working in parallel to deliver features to customers with greater speed and reliability.

### Layered Approach

**Event Driven .NET** is designed to allow you to wade gradually into the waters of event-driven microservices so that your system architecture aligns with your organizational structure and the skill set of your developers, in addition to the maturity of your DevOps processes.

<p align="center">
  <img width="900" src="images/event-driven-layers.png">
</p>

Starting at the bottom, you can start by adopting a [Domain Driven Design](https://en.wikipedia.org/wiki/Domain-driven_design) (DDD) approach, then build on this foundation by adding [Command Query Responsibility Segregation](https://martinfowler.com/bliki/CQRS.html) (CQRS). When you have the need for inter-service communication, you can move to the [Event Bus](https://martinfowler.com/eaaDev/EventCollaboration.html) layer, utilizing [Dapr](https://dapr.io/) as a [pub-sub](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) abstraction over an underlying message broker.

If you have the need to maintain eventual data consistency with updates to multiple services spanning a logically atomic operation, you are ready to advance to the next layer: [Sagas](https://microservices.io/patterns/data/saga.html). When your organization has the need for capabilities such as built-in audit trail and the ability to atomically persist and publish events, you may wish to move to the [Event Sourcing](https://microservices.io/patterns/data/event-sourcing.html) layer (to be implemented).

Lastly, in order to obtain capabilities such as real-time data analysis and transformation, and if your IT organization can support it, you may wish to venture into [Event Streams](https://hazelcast.com/glossary/event-stream-processing/) (to be implemented).

### Abstractions

Each layer is supported by **abstraction and implementation libraries**, which are distributed as [NuGet](https://www.nuget.org/) packages. Simply reference the appropriate package in your project in order to utilize its interfaces and classes.

- **Domain Driven Design**
  - [EventDriven.DDD.Abstractions](https://www.nuget.org/packages/EventDriven.DDD.Abstractions): Abstractions for implementing Domain Driven Design in .NET.
- **Event Bus**
  - [EventDriven.EventBus.Abstractions](https://www.nuget.org/packages/EventDriven.EventBus.Abstractions): Generic event bus abstraction. 
  - [EventDriven.EventBus.Dapr](https://www.nuget.org/packages/EventDriven.EventBus.Dapr): Event bus abstraction over Dapr pub/sub.
  - [EventDriven.EventBus.Dapr.EventCache.Mongo](https://www.nuget.org/packages/EventDriven.EventBus.Dapr.EventCache.Mongo): MongoDB implementation of event caching with Dapr state store.
  - [EventDriven.SchemaRegistry.Mongo](https://www.nuget.org/packages/EventDriven.SchemaRegistry.Mongo): MongoDB state store for validating messages against schemas that are stored in a registry by topic name.
- **Sagas**
  - [EventDriven.Sagas.Abstractions](https://www.nuget.org/packages/EventDriven.Sagas.Abstractions): Abstractions for sagas, steps, actions, commands, dispatchers, handlers and evaluators.
  - [EventDriven.Sagas.Configuration.Abstractions](https://www.nuget.org/packages/EventDriven.Sagas.Configuration.Abstractions): Abstractions for saga configurations.
  - [EventDriven.Sagas.Configuration.Mongo](https://www.nuget.org/packages/EventDriven.Sagas.Configuration.Mongo): MongoDB implementation for saga configuration repositories.
  - [EventDriven.Sagas.DependencyInjection](https://www.nuget.org/packages/EventDriven.Sagas.DependencyInjection): `AddSaga` service collection extension methods.
  - [EventDriven.Sagas.EventBus.Abstractions](https://www.nuget.org/packages/EventDriven.Sagas.EventBus.Abstractions): Abstractions for handling integration events and dispatching command results.
  - [EventDriven.Sagas.Persistence.Abstractions](https://www.nuget.org/packages/EventDriven.Sagas.Persistence.Abstractions): Abstractions for persisting saga snapshots.
  - [EventDriven.Sagas.Persistence.Mongo](https://www.nuget.org/packages/EventDriven.Sagas.Persistence.Mongo): MongoDB implementation for persisting saga snapshots.

### Reference Architectures

To aid you in implementing an event-driven microservices architecture, **Event Driven .NET** includes a base **reference architecture**, which includes a functioning solution that references the abstraction and implementation libraries listed above.

- [EventDriven.ReferenceArchitecture](https://github.com/event-driven-dotnet/EventDriven.ReferenceArchitecture): Reference architecture for using EventDriven abstractions and libraries for **Domain Driven Design** (DDD), **Command-Query Responsibility Segregation** (CQRS) and **Event Driven Architecture** (EDA) with Dapr Event Bus.
- [EventDriven.Sagas](https://github.com/event-driven-dotnet/EventDriven.Sagas): Abstractions and reference architecture for implementing the **Saga pattern** to orchestrate atomic operations which span multiple services.

## Roadmap

The roadmap for **Event Driven .NET** includes the following items:

- **Event Sourcing**: This will provide the ability to treat domain events as the *source of truth* for a system, so that services no longer need to treat persistence and event publishing atomically. It also provides built-in audit trail. 
- **Event Streams**: This will support real-time data analysis and transformation by means of a durable, append-only message broker, such as [Apache Kafka](https://kafka.apache.org/) or [Amazon Kinesis](https://aws.amazon.com/kinesis/).
