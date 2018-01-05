# PAFR SUMMARY

## Creational Patterns
### Abstract Factory Pattern
Provide an interface for creating families of related or dependent objects without specifying their concrete classes.

#### Applicability
+ a system should be independent of how its products are created, composed, and
represented.
+ a system should be configured with one of multiple families of products.
+ a family of related product objects is designed to be used together, and you
need to enforce this constraint.
+ you want to provide a class library of products, and you want to reveal just
their interfaces, not their implementations.

#### Structure
![Abstract Factory](img/abstractFactory.jpg)

#### Consequences
1. It isolates concrete classes.
2. It makes exchanging product families easy
3. It promotes consistency among products
4. Supporting new kinds of products is difficult (see extensible implementation
  below)

#### Implementation
1. It's usually best to implement factories as singletons (since you almost
  always only need one)
2. Creating the products is done by the concrete factories. The most common way
  is to do this using the Factory Method Pattern for each product.

##### Extensible Implementation
If we want to add products we have to change the abstractFactory interface and
therefore all of it's subclasses. A more flexible but less safe design is to add a parameter to operations that create objects. This parameter specifies the kind of object to be created. It could be a class identifier, an integer, a string, or anything else that identifies the kind of product. In fact with this approach, AbstractFactory only needs a single "Make" operation with a parameter indicating the kind of object to create.

---
### Builder Pattern
Separate the construction of a complex object from its representation so that the same construction process can create different representations.

#### Applicability
+ When the algorithm for creating a complex object should be independent of the parts that make up the object and how they're assembled.
+ When the construction process must allow different representations for the object that's constructed.

#### Structure
![Builder](img/builder.jpg)

#### Consequences
1. It lets you vary a product's internal representation.
2. It isolates code for construction and representation.
3. It gives you finer control over the construction process.

#### Implementation
1. **Assembly and construction interface.** Builders construct their products in step-by-step fashion. Therefore the Builder class interface must be general enough to allow the construction of products for all kinds of concrete builders.
2. **Empty methods as default in Builder.** In C++, the build methods are intentionally not declared pure virtual member functions. They're defined as empty methods instead, letting clients override only the operations they're interested in.
---
### Factory Method Pattern
Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.

#### Applicability
+ a class can't anticipate the class of objects it must create.
+ a class wants its subclasses to specify the objects it creates.
+ classes delegate responsibility to one of several helper subclasses, and you want to localize the knowledge of which helper subclass is the delegate.

#### Structure
![Abstract Factory](img/factoryMethod.jpg)

#### Consequences
1. **Provides hooks for subclasses.** Creating objects inside a class with a factory method is always more flexible than creating an object directly. Factory Method gives subclasses a hook for providing an extended version of an object.
2. **Connects parallel class hierarchies.** In the examples we've considered so far, the factory method is only called by Creators. But this doesn't have to be the case; clients can find factory methods useful, especially in the case of parallel class hierarchies.

#### Implementation
1. **Two major varieties.** The two main variations of the Factory Method pattern are (1) the case when the Creator class is an abstract class and does not provide an implementation for the factory method it declares, and (2) the case when the Creator is a concrete class and provides a default implementation for the factory method. It's also possible to have an abstract class that defines a default implementation, but this is less common.
2. **Parameterized factory methods.** Another variation on the pattern lets the factory method create multiple kinds of products. The factory method takes a parameter that identifies the kind of object to create. All objects the factory method creates will share the Product interface.
---
### Prototype Pattern
Specify the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype.

#### Applicability
1. when the classes to instantiate are specified at run-time, for example, by dynamic loading; or
2. to avoid building a class hierarchy of factories that parallels the class hierarchy of products; or
3. when instances of a class can have one of only a few different combinations of state. It may be more convenient to install a corresponding number of prototypes and clone them rather than instantiating the class manually, each time with the appropriate state.

#### Structure
![Prototype](img/prototype.jpg)

#### Consequences
1. **Adding and removing products at run-time.** Prototypes let you incorporate a new concrete product class into a system simply by registering a prototypical instance with the client.
2. **Specifying new objects by varying values.** Highly dynamic systems let you define new behavior through object composition—by specifying values for an object's variables, for example—and not by defining new classes.
3. **Specifying new objects by varying structure.** Many applications build objects from parts and subparts. Editors for circuit design, for example, build circuits out of subcircuits.1 For convenience, such applications often let you instantiate complex, user-defined structures, say, to use a specific subcircuit again and again.
4. **Configuring an application with classes dynamically.** Some run-time environments let you load classes into an application dynamically. The Prototype pattern is the key to exploiting such facilities in a language like C++.

#### Implementation
1. **Using a prototype manager.** When the number of prototypes in a system isn't fixed (that is, they can be created and destroyed dynamically), keep a registry of available prototypes. Clients won't manage prototypes themselves but will store and retrieve them from the registry. A client will ask the registry for a prototype before cloning it. We call this registry a prototype manager.
2. **Implementing the Clone operation.** The hardest part of the Prototype pattern is implementing the Clone operation correctly. It's particularly tricky when object structures contain circular references.
3. **Initializing clones.** While some clients are perfectly happy with the clone as is, others will want to initialize some or all of its internal state to values of their choosing. You generally can't pass these values in the Clone operation, because their number will vary between classes of prototypes. We can use setters in these cases.

---
### Singleton Pattern
Ensure a class only has one instance, and provide a global point of access to it.

#### Applicability
+ there must be exactly one instance of a class, and it must be accessible to clients from a well-known access point.
+ when the sole instance should be extensible by subclassing, and clients should be able to use an extended instance without modifying their code.

#### Structure
![Singleton](img/singleton.jpg)

#### Consequences
1. **Controlled access to sole instance.** Because the Singleton class encapsulates its sole instance, it can have strict control over how and when clients access it.
2. **Reduced name space.** The Singleton pattern is an improvement over global variables. It avoids polluting the name space with global variables that store sole instances.
3. **Permits refinement of operations and representation.** The Singleton class may be subclassed, and it's easy to configure an application with an instance of this extended class. You can configure the application with an instance of the class you need at run-time.
4. **Permits a variable number of instances.** The pattern makes it easy to change your mind and allow more than one instance of the Singleton class. Moreover, you can use the same approach to control the number of instances that the application uses. Only the operation that grants access to the Singleton instance needs to change.
5. **More flexible than class operations.** Another way to package a singleton's functionality is to use class operations (that is, static member functions in C++ or class methods in Smalltalk). But both of these language techniques make it hard to change a design to allow more than one instance of a class. Moreover, static member functions in C++ are never virtual, so subclasses can't override them polymorphically.

#### Implementation
1. **Ensuring a unique instance.** The Singleton pattern makes the sole instance a normal instance of a class, but that class is written so that only one instance can ever be created. A common way to do this is to hide the operation that creates the instance behind a class operation (that is, either a static member function or a class method) that guarantees only one instance is created. This operation has access to the variable that holds the unique instance, and it ensures the variable is initialized with the unique instance before returning its value. This approach ensures that a singleton is created and initialized before its first use.
2. **Subclassing the Singleton class.** The main issue is not so much defining the subclass but installing its unique instance so that clients will be able to use it. In essence, the variable that refers to the singleton instance must get initialized with an instance of the subclass. The simplest technique is to determine which singleton you want to use in the Singleton's Instance operation. An example in the Sample Code shows how to implement this technique with environment variables.
---

## Structural Patterns

### Adapter Pattern
Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.

#### Applicability
When:
+ You want to use an existing class, and its interface does not match the one you need.
+ You want to create a reusable class that cooperates with unrelated or unforeseen classes, that is, classes that don't necessarily have compatible interfaces.
+ (object adapter only) You need to use several existing subclasses, but it's impractical to adapt their interface by subclassing

#### Structure
##### Adapter through inheritance
![Adapter through inheritance](img/adapter1.jpg)
##### Adapter through composition
![Adapter through composition](img/adapter1.jpg)

#### Consequences
Class and object adapters have different trade-offs.

**A class adapter:**
+ Adapts Adaptee to Target by committing to a concrete Adapter class. As a consequence, a class adapter won't work when we want to adapt a class and all its subclasses.
+ Lets Adapter override some of Adaptee's behavior, since Adapter is a subclass of Adaptee.
+ Introduces only one object, and no additional pointer indirection is needed to get to the adaptee.
**An object adapter:**
+ Lets a single Adapter work with many Adaptees—that is, the Adaptee itself and all of its subclasses (if any). The Adapter can also add functionality to all Adaptees at once.
+ Makes it harder to override Adaptee behavior. It will require subclassing Adaptee and making Adapter refer to the subclass rather than the Adaptee itself.

#### Implementation
Quite complex, see book


### Bridge Pattern
Decouple an abstraction from its implementation so that the two can vary independently.

#### Applicability
When:
+ You want to avoid a permanent binding between an abstraction and its implementation. This might be the case, for example, when the implementation must be selected or switched at run-time.
+ Both the abstractions and their implementations should be extensible by subclassing. In this case, the Bridge pattern lets you combine the different abstractions and implementations and extend them independently.
+ Changes in the implementation of an abstraction should have no impact on clients; that is, their code should not have to be recompiled.
+ You have a proliferation of classes as shown earlier in the first Motivation diagram. Such a class hierarchy indicates the need for splitting an object into two parts.
+ You want to share an implementation among multiple objects (perhaps using reference counting), and this fact should be hidden from the client.

#### Structure
![Bridge](img/bridge.jpg)

#### Consequences
1. **Decoupling interface and implementation.** An implementation is not bound permanently to an interface. The implementation of an abstraction can be configured at run-time. It's even possible for an object to change its implementation at run-time.
Decoupling Abstraction and Implementor also eliminates compile-time dependencies on the implementation. Changing an implementation class doesn't require recompiling the Abstraction class and its clients. This property is essential when you must ensure binary compatibility between different versions of a class library.
Furthermore, this decoupling encourages layering that can lead to a better-structured system. The high-level part of a system only has to know about Abstraction and Implementor.
2. **Improved extensibility.** You can extend the Abstraction and Implementor hierarchies independently.
3. **Hiding implementation details from clients.** You can shield clients from implementation details, like the sharing of implementor objects and the accompanying reference count mechanism (if any).

#### Implementation
1. **Only one Implementor.** In situations where there's only one implementation, creating an abstract Implementor class isn't necessary. This is a degenerate case of the Bridge pattern; there's a one-to-one relationship between Abstraction and Implementor. Nevertheless, this separation is still useful when a change in the implementation of a class must not affect its existing clients—that is, they shouldn't have to be recompiled, just relinked.
2. **Creating the right Implementor object.** How, when, and where do you decide which Implementor class to instantiate when there's more than one? If Abstraction knows about all ConcreteImplementor classes, then it can instantiate one of them in its constructor; it can decide between them based on parameters passed to its constructor. If, for example, a collection class supports multiple implementations, the decision can be based on the size of the collection. A linked list implementation can be used for small collections and a hash table for larger ones.

### Composite Pattern
Compose objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly.

#### Applicability
When:
+ you want to represent part-whole hierarchies of objects.
+ you want clients to be able to ignore the difference between compositions of objects and individual objects. Clients will treat all objects in the composite structure uniformly.

#### Structure
![Composite](img/composite.jpg)

#### Consequences
This pattern:
1. Defines class hierarchies consisting of primitive objects and composite objects. Primitive objects can be composed into more complex objects, which in turn can be composed, and so on recursively. Wherever client code expects a primitive object, it can also take a composite object.
2. Makes the client simple. Clients can treat composite structures and individual objects uniformly. Clients normally don't know (and shouldn't care) whether they're dealing with a leaf or a composite component. This simplifies client
158
code, because it avoids having to write tag-and-case-statement-style functions over the classes that define the composition.
3. Makes it easier to add new kinds of components. Newly defined Composite or Leaf subclasses work automatically with existing structures and client code. Clients don't have to be changed for new Component classes.
4. Can make your design overly general. The disadvantage of making it easy to add new components is that it makes it harder to restrict the components of a composite. Sometimes you want a composite to have only certain components. With Composite, you can't rely on the type system to enforce those constraints for you. You'll have to use run-time checks instead.
5. 
#### Implementation
