# Cross Architecture
_**Alessandro Meo** - Nov. 7, 2020_

The **Cross Architecture** is a SW architecture approach inspired by the combination of some well-known design techniques, like the *multi-layer architecture*, the *Onion Architecture*, the *Clean Architecture* and the *DDD*.

The main idea that led me to the definition of the **Cross Architecture** was to describe a flexible way for designing the architecture of a SW over the actual application needs, while respecting a set of basic constraints providing good decoupling, testability and logical organization of code. Also, the same set of rules should provide a guidance for extending the architecture in order to follow potential application evolutions.

In practice, the above set of rules should be able to span and serve to rather different situations, hence the name **Cross Architecture**.

## General Concepts

The **Cross Architecture** can be represented by the following cross-shaped schema.

![Cross Architecture - Schema](Alessandro Meo Cross Architecture - Schema.png)

Each box in the schema represents a logical area that may be implemented by one or more SW blocks (either unrelated or organized in loosely coupled layers) or even by no SW block in case the box is marked as optional (gray background).

The compile time dependencies between the SW blocks are explicitly expressed, together with their intended use, by means of arrows among the holding boxes:
* a *“uses” dependency* means that the dependent block uses the pointed one, moreover, in case the pointed block is an interface block, one of its implementations is required to be injected into the dependent block (more precisely, this is about the contained classes);
* an *“implements” dependency* means that the dependent block implements the pointed one (more precisely, this is about the contained classes).

The dependency between the *Application Interface* and the *Infrastructure Interfaces* boxes is only present when no SW block implements the *Application Services* box.

## Composition Chain

The schema described in the previous section only deals with loosely coupled SW block sets implementing the application features, but, in order to complete the actual SW, the composition root code should be added.

For keeping a high level of flexibility, in the **Cross Architecture** the composition root code is split into several blocks (the ones holding the "U" and "I" letters), chained together in a nested reference tree called *Composition Chain*.

![Cross Architecture - Composition Chain](Alessandro Meo Cross Architecture - Composition Chain.png)

As shown above, the *Composition Chain* is rooted in the *Application Host*, whose responsibility is to build the SW layer stack and provide any configuration or action required for the application startup. The *Application Host* is the actual SW application entry point, built as a ready-to-run executable or potentially hosted inside an application server (e.g.: an HTTP server).

## Testing

Leveraging the *Composition Chain* technique makes it possible to implement isolated automated test blocks, easily referencing any unit or group of layers without composition code duplication.

Some examples about are shown in the following sub-sections.

### Unit Testing

![Cross Architecture - Unit Testing](Alessandro Meo Cross Architecture - Unit Testing.png)

### Integration Testing

![Cross Architecture - Integration Testing](Alessandro Meo Cross Architecture - Integration Testing.png)

### Functional Testing

![Cross Architecture - Functional Testing](Alessandro Meo Cross Architecture - Functional Testing.png)

## Architecture Reduction

The most important aim of the **Cross Architecture** is to flexibly adapt to the actual application needs. This is done by the so called *Architecture Reduction*, which mainly implies:
1. dividing a logical area (box) into multiple SW blocks rather than implementing it as a single SW block;
2. making the internal design of the SW blocks more or less complex;
3. skipping the implementation of unrequired logical areas;
4. duplicating the whole architecture schema when deep separation between sub-contexts is required.

The next sub-sections present some examples of *Architecture Reduction* for some of the most common scenarios.

### DDD

![Cross Architecture - Implementing DDD](Alessandro Meo Cross Architecture - Implementing DDD.png)

Implementing a DDD approach with the **Cross Architecture** leads to a design much similar to the one produced by the *Onion/Clean Architectures*, as highlighted by the "Application Core" box, which recalls one of their typical concepts.

On the other hand, with respect to the *Onion/Clean Architectures*, the **Cross Architecture** adds:
* constraints about the references between SW blocks; e.g.: the *Application Interface* can only interact with the *Application Services* and the *Core Model*, while the *Core Services* get "hidden" by the *Application Services*; this is a different from the *Onion/Clean Architectures* where all the "inner" blocks can be referenced by the "outer" blocks;
* information about the injection points; e.g.: the *Infrastructure* get injected into the *Application Services*, but not into the *Core Services*; this enforces a pure "DDD style", where the domain services are not allowed to interact with external systems.

### Multi-Layer

![Cross Architecture - Implementing Multi Layer](Alessandro Meo Cross Architecture - Implementing Multi Layer.png)

The "classical" multi-layer architecture, composed of the UI, BLL and DAL layers, can be easily implemented in **Cross Architecture** by mapping those layers to the *Application Interface*, *Application Services* and *Infrastructure* boxes.

With respect to the "classical" multi-layer approach, the **Cross Architecture** adds loose decoupling between layers and a mandatory *Core Model*, gathering common elements, referenced by all the other layers, but also implementing the core of the application data model. The *Core Model* cannot be a bare collector of shared elements, but it should be considered as one of the main elements of the architecture when organizing concerns and distributing responsibilities.

### Simple CRUD

![Cross Architecture - Implementing Simple CRUD](Alessandro Meo Cross Architecture - Implementing Simple CRUD.png)

A simple CRUD application does not have a properly defined business logic, so it may be implemented removing the *Application Services*, which are defined as an optional box by the **Cross Architecture**; the resulting design is very dry and makes use of the optional reference between the *Application Interface* and the *Infrastructure*.

In the example above, the *Application Interface* is implemented by a web API application, eventually called by a S.P.A.; this is only to stress that, in the **Cross Architecture** the application interface is not necessarily dedicated to human users; moreover, the calling the S.P.A. was included in the schema only as an example, but it is not covered by the **Cross Architecture**.

### Simple In-Memory

![Cross Architecture - Implementing Simple In-Memory](Alessandro Meo Cross Architecture - Implementing Simple In-Memory.png)

This example shows the simplest scenario covered by the **Cross Architecture**, where the *Application Interface* exposes an in-memory working logic, condensed into the *Core Model*.

### SW Built Around a HW Device

![Cross Architecture - Implementing HW Device Based SW](Alessandro Meo Cross Architecture - Implementing HW Device Based SW.png)

This example comes from one of the applications I've been working on during the last few years, which is a SW acquiring images from a custom HW device and elaborating them by means of some algorithms. The application was designed as a loosely coupled multi-layer software and, in particular, several layers were needed to abstract from the custom HW.

Reconsidering that architecture in order to fit into the **Cross Architecture** led to the schema above, where the *Infrastructure* has been split into several SW blocks for interacting with the database, on one hand, and with the custom HW, on the other hand. Also, the presence of multiple loosely coupled layers for interacting with the custom HW (the "device manager" and the "device driver" blocks) perfectly fits into the **Cross Architecture** philosophy.

### Micro-Services

![Cross Architecture - Implementing Micro-Services](Alessandro Meo Cross Architecture - Implementing Micro-Services.png)

As already stated, the *Architecture Reduction* includes the possibility to duplicate the whole architecture schema when deep separation between sub-contexts is required; this type of reduction is particularly suited for the micro-services scenario, as shown above.

In particular, the **Cross Architecture** helps in differentiating the internal design of the micro-services starting from their actual needs, while keeping their architectures tied to the same general schema and rules.
