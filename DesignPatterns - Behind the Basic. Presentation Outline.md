#### 07/05/2016
# DesignPatterns - Behind the Basic
## Presentation Outline
### Written by Ziv Gilad

# Introduction
1. Why learning design patters?
	1. Communicating a problem to another developer
	2. Good to have a common vocabulary
	3. solve common problems during the process of software design
	4. Make full use of existing successful development experience
	5. **Build an extensible solution for software systems**
	6. Maintenance and reusability of software

2. Explain SOLID design principles
3. Introduce the GoF

# Abstract Factory (creational)
1. The factory pattern:

	1. Centralized location that takes care of creating of things
	2. Separate component responsible solely for creation of objects
	3. The actual object creation is outsourced to somewhere else
	4. One shot creation and **not piece-wise** (remember this when we get to the **builder**).
	5. Demonstration - The Zap compare prices page (some show 'buy', some 'smart buy', some show PayPal icon, some show 'public trust').
	6. Demonstration - Database dll replacement on disk without recompiling

![](https://raw.githubusercontent.com/zivgi/DesignPatterns---Behind-the-Basic.-Presentation-for-Zap/master/5-7-2016%2011-04-09%20AM.jpg)
</br>
2. The abstract factory pattern:

	1. Super-factory creating other factories (factory of factories).
	2. For portable application we want to encapsulate platform dependencies (operating system, database, etc).
	3. Makes exchanging creation of families of related objects easy.
	4. The most complex creational pattern. Heavily abstracted
	4. Demonstration - The CreaditCard AbstractFactory.


# Builder Pattern (creational)

1. Separate component for building up a **complex** object
2. When piece-wise object construction is complicated
3. Can define separate builder for building different aspects of the model (However no blend of concrete builders in GoF)
4. Can force immutability - No need to expose setters
5. Prevent telescoping constructors with each parameter variation
4. The client uses the Director object to construct a complex object by specifying only its builder, type and content (no representation).
5. The Director uses its builder interfaces to create a  representation.
4. Builder encapsulates the internal stages of construction. Client code is never in contact with builder methods. Client only in contact with the director 
4. A fluent builder: chain several calls to create one large  sequential call
4. Demonstration - The yellow pages:
	1. Director: getHeader, getReviews, getAddress
	1. An header builder - buildLogo, buildTitle
	2. A review builder - buildName, buildPicture, buildText, buildRank
	3. Contact info builder - buildAddress, buildPhone

![](https://raw.githubusercontent.com/zivgi/DesignPatterns---Behind-the-Basic.-Presentation-for-Zap/master/5-7-2016%2012-11-51%20PM.jpg)
# Strategy Pattern (Behavioral)

1. Strategy pattern defines a set of algorithms for specific task that can be used interchangeably.
2. Strategy deals with how to perform a certain task - it encapsulates an algorithm
3. Algorithms can be selected by the application on-the-fly at runtime
4. Context, anything that requires changing behaviors - is composed of a Strategy
6. The choice of algorithm is fairly stable
7. Example - Sorting: `QuickSort`, `MergeSort`, `BubbleSort`
8. Example - Compression: `ZipCompression`, `RarCompression`
9. Example - PaymentStrategy - `CreditCardStrategy`, `PaypalStrategy`
10. Strategy in c#: `Array.Sort<T> Method (T[], IComparer<T>)`

![](https://raw.githubusercontent.com/zivgi/DesignPatterns---Behind-the-Basic.-Presentation-for-Zap/master/5-7-2016%203-52-17%20PM.jpg)

# State Pattern (behavioral)
1. Encapsulate object varying behavior based on its internal state (state-dependent behavior)
2. Cleaner way to change object behavior at runtime without huge switch case structures (simplifies ciclomatic complexity)
3. Useful when an object can be in several different states which effect its behavior
4. The interface used to access the object doesn't change. Change is hidden to the outside world with the use of a wrapper object, or context.
5. **The object with states delegates its calls to the state object**
6. Each State derived class has knowledge of (coupling to) its siblings - dependencies between subclasses. **Violates OCP!**
7. Implementation builds on **Strategy** pattern, where state  is selected from its "palette" of Strategy objects.
8. Demonstration - e-commerce application where an order can go through the following states: `New`, `Shipped`, `Cancelled`

## How Sate different than Strategy?
1. Although built on Strategy, State is different in its **intent**:
	* State pattern deals with **what** state an object is in.
	* Strategy pattern deals with **how** to perform a certain task
2. State is very dynamic. Strategy is a bind-once
3. State knows about next state. Strategy doesn't know of other algorithms (independent)
4. State selected from internal "palette" of strategies. Strategy externally selected by the application 

# Bridge Pattern (Structural)
1. Decouples an abstraction from its implementation so that the two can vary independently
2. Chosen when we know the details won't be right to begin with
3. If we aren't quite sure about our end product, we use bridge for level of indirection
5. Favors composition over inheritance
6. Increase complexity, difficult to plan
7. Bridge in C# - Client uses `StreamReader` (the abstraction), `StreamReader` uses the Stream hierarchy (`FileStream`, `MemoryStream`, `NetworkStream`)

# Visitor Pattern (Behavioral)
1. Separate an algorithm from the object structure
1. Represent an operation to be performed on the elements of an object structure
2. Can add new operations to existing object structures without modifying them (OCP)
3. Design lightweight Element classes. New functionality can easily be added using visitors
4. Must design around the visitor from the beginning.
4. Element - Has an `accept` method which takes a visitor and calls its `visit(this)`. This refers to the element: `accept( Visitor v ) { v.visit( this ); }`
5. Visitor - Define visit(ElementXxx) method for each Element derived type. **Visitor knows every element!**
6. Every time a new type of Element is added, every Visitor must update
6. OCP - visitor operates on public data only (cannot modify existing structures), 
7. Single responsibility principle - Visitor implement in its own class
8. When client needs an operation, it creates a visitor and calls element.accept(visitor)
9. Best fit for unrelated operations to perform across a structure of objects. Instead of adding code throughout the object structure we keep it separately
10. Examples - shipping visitor, display visitor

## Decorator
1. When want to wrap another object (composition) to add functionality to it
2. And we don't mess up hierarchy of our concrete objects
2. Compose behavior dynamically by using a subclass that decorates our object
3. Utilizes both composition and inheritance (is a, has-a)
4. Dynamically add functionality and can be chained
4. We create decorators to prevent creating permutations of classes.
5. The constructor takes an instance of the object (concrete class or another decorator)
6. Demonstration - Yellowpage item: Item can be decorated with the pay pal logo and public trust logo

![](https://raw.githubusercontent.com/zivgi/DesignPatterns---Behind-the-Basic.-Presentation-for-Zap/master/5-7-2016%2011-48-57%20PM.jpg)

# Flyweight pattern
1. Minimize memory use by sharing data with similar typed objects
2. This is an optimization pattern
3. Lots of objects pass around, we don't want to create them for every client/end user
4. Utilizes a factory pattern
5. We have large number of similar objects which are stateless/immutable
6. Strings in c# are immutable and the cLR conserves storage by maintaining a table, called the intern pool. The intern pool is the flyweight factory.
7. Same as prototype? No - Prototype clones to a new instance
8. Client requests an object from factory and doesn't know it's a flyweight object (flyweight is under the hood)
9. Demonstrations - restaurant menu, inventory management system

----------

## The more simple ones (if time permits):
#Chain of responsibility
1. Assume someone along the way will handle
2. Decouple the sender and receiver objects
3. Receiver has a reference to next receiver in line
4. If doesn't know what to do - hand off to someone else
5. Easy to add links to the chain
6. Chain length might introduce performance issue

# Command
1. Encapsulate each request as an object
2. Encapsulate all of it's functionality - never hands off to someone else
3. Decouples sender from processor
4. Object oriented callback instead of a method
5. Often supports undo functionality (using use memento)

# Memento
1. Externalize object state to provide rollback (undo) functionality
2. Restore object to previous state
3. Be careful not to violate encapsulation (do not expose internal state)
4. Examples in c#: date, `ISerializable`
5. Usage examples: Edit-Undo (stack).  
6. Can be expensive if we do not limit history.

# Observer (behavioral)
1. Subject needs to be observed by one of the observers
2. One subject can have many observers
3. Decouple object from those who want to use it
4. When subject state changes it uses broadcast to update all observers

# Template (behavioral)
1. Force algorithm but allows pieces to be configured by the user
2. Same algorithm but different implementation - Guarantees algorithm adherence
3. Class based, compile time (before hand)
4. Base class is responsible for calling the child class, **NOT** the other way around!
5. Hooks may overridden
6. Operations **must** be overridden
7. Difficult program flow
8. Same as Strategy? No - strategy algorithms selected at runtime
9. Same as builder? No - template has no object separation - generalized flow and the overridden methods are members of the same class  
10. Example - `OrderTemplate` abstract. Calls `DoCheckout` then `DoPayment` then `DoReceipt` then `DoDelivery`
11. Example - `InstallerTemplate` - `ShowWelcome`, `ShowOptionsSelection`, `ShowInstallingProgress`, `ShowThankYou`

## Prototype Pattern
 1. Get a unique instance of same object
 2. Avoid costly creation
 3. Usually implemented with a registry
 4. Each instance is still unique (oppose to FlyWeight)
 5. Implemented around a clone (Can choose shallow vs deep copy)

## Adapter pattern (structural)
1. Connection new code to legacy code, without changing the working contract of original code
2. Convert interface to an existing interface
3. Legacy - don't want to or can't change
4. Simple solution
5. Make things work after code was already designed
6. C# built in adapters - DataAdapter, collection.ToArray(), collection.ToDictionary()

## Composite Pattern
1. Hierarchical patterns for dealing with tree structures of information
2. Component represents part or whole structure
3. Individual object treated as composite - Same operations applied on individual and composites
4. Composite also knows about its childs

## Facade
1. Provides a simplified interface to a complex or difficult to use system
2. Make an API easier to use - the client is simple
3. Simplify the interface or client usage
4. Usually contains through composition, other classes
5. Deals with  flat structure or problem - no inheritance

## Proxy
1. We want to wrap areal object with proxy
2. The proxy itself is called to access the real object
3. The proxy and the underlying object share the same interface
4. Just an intermediate object that intercepts calls
