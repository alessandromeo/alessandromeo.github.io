# Cross Architecture
_**Alessandro Meo** - Nov. 7, 2020_

The **Cross Architecture** is a SW architecure approach inspired by the combination of some well known architecures and design techniques, like the *multi-layer architecture*, the *Onion Architecture*, the *Clean Architecture* and the *DDD*.

The main idea that led me to the definition of the **Cross Architecture** was to describe a flexible way for designing the architecture of a SW over the actual needs of the application, while respecting a set of basic constraints providing good decoupling, testability and logical organization of the code. Also, the same set of rules, should provide a guidance for extending the architecture in order to follow potential evolutions of the application.

In practice, the above set of rules should be able span and serve to rather different situations, hence the name **Cross Architecture**.

## General Concepts

The **Cross Architecture** can be represented by the following cross-shaped schema.

![Cross Architecture - Schema](Alessandro Meo Cross Architecture - Schema.png)

Each box represents one or more SW blocks tied to the described logical area (e.g.: the Infrastructure may be composed of a DB access block and a mail send block).

Each dependency displayed is a compile time dependency; also, its use is explicitly expressed:
* a “uses” dependency means that the dependent block uses the pointed one, moreover, in case the pointed block is and interface block, one of its implementations is required to the dependent block as an injection (more precisely, this is about the contained classes);
* the “implements” dependency means that the dependent block implements the pointed one (more precisely, this is about the contained classes).

Some blocks and dependencies are optional.

## Composition Chain

![Cross Architecture - Composition Chain](Alessandro Meo Cross Architecture - Composition Chain.png)

## Testing

### Unit Testing

![Cross Architecture - Unit Testing](Alessandro Meo Cross Architecture - Unit Testing.png)

### Integration Testing

![Cross Architecture - Integration Testing](Alessandro Meo Cross Architecture - Integration Testing.png)

### Functional Testing

![Cross Architecture - Functional Testing](Alessandro Meo Cross Architecture - Functional Testing.png)

## Architecture Reduction

### DDD

![Cross Architecture - Implementing DDD](Alessandro Meo Cross Architecture - Implementing DDD.png)

### Multi-Layer

![Cross Architecture - Implementing Multi Layer](Alessandro Meo Cross Architecture - Implementing Multi Layer.png)

### Simple CRUD

![Cross Architecture - Implementing Simple CRUD](Alessandro Meo Cross Architecture - Implementing Simple CRUD.png)

### Simple In-Memory

![Cross Architecture - Implementing Simple In-Memory](Alessandro Meo Cross Architecture - Implementing Simple In-Memory.png)

### SW Built over an HW Device

![Cross Architecture - Implementing HW Device Based SW](Alessandro Meo Cross Architecture - Implementing HW Device Based SW.png)

### Micro-Services

![Cross Architecture - Implementing Micro-Services](Alessandro Meo Cross Architecture - Implementing Micro-Services.png)
