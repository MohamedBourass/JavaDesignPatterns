# Java Design Patterns

The Gang of Four (GoF) design patterns, introduced in the book "Design Patterns: Elements of Reusable Object-Oriented Software" by Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides, are foundational to software engineering. They are categorized into three types: Creational, Structural, and Behavioral patterns.

## Creational Patterns

### 1. Abstract Factory
Provides an interface for creating families of related or dependent objects without specifying their concrete classes.

```java
```

### 2. Builder
Separates the construction of a complex object from its representation, allowing the same construction process to create different representations.

```java
```

### 3. Factory Method
Defines an interface for creating an object but allows subclasses to alter the type of objects that will be created.

```java
abstract class Product {
    abstract void use();
}

class ConcreteProduct extends Product {
    void use() {
        System.out.println("Using ConcreteProduct");
    }
}

abstract class Creator {
    public abstract Product factoryMethod();

    public void someOperation() {
        Product product = factoryMethod();
        product.use();
    }
}

class ConcreteCreator extends Creator {
    public Product factoryMethod() {
        return new ConcreteProduct();
    }
}
```

### 4. Prototype
Specifies the kinds of objects to create using a prototypical instance and creates new objects by copying this prototype.

```java
```

### 5. Singleton
Ensures a class has only one instance and provides a global point of access to it.

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

## Structural Patterns

### 6. Adapter
Converts the interface of a class into another interface clients expect. It allows classes to work together that couldn't otherwise because of incompatible interfaces.

```java
```

### 7. Bridge
Decouples an abstraction from its implementation so that the two can vary independently.

```java
```

### 8. Composite
Composes objects into tree structures to represent part-whole hierarchies, allowing clients to treat individual objects and compositions of objects uniformly.

```java
```

### 9. Decorator
Adds additional responsibilities to an object dynamically. It provides a flexible alternative to subclassing for extending functionality.

```java
```

### 10. Facade
Provides a unified interface to a set of interfaces in a subsystem, making the subsystem easier to use.

```java
```

### 11. Flyweight
Uses sharing to support large numbers of fine-grained objects efficiently.

```java
```

### 12. Proxy
Provides a surrogate or placeholder for another object to control access to it.

```java
```

## Behavioral Patterns

### 13. Chain of Responsibility
Passes a request along a chain of handlers. Each handler either processes the request or passes it to the next handler in the chain.

```java
```

### 14. Command
Encapsulates a request as an object, thereby allowing for parameterization of clients with queues, requests, and operations.

```java
```

### 15. Interpreter
Defines a representation for a language's grammar along with an interpreter that uses the representation to interpret sentences in the language.

```java
```

### 16. Iterator
Provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

```java
```

### 17. Mediator
Defines an object that encapsulates how a set of objects interact, promoting loose coupling by keeping objects from referring to each other explicitly.

```java
```

### 18. Memento
Captures and externalizes an object's internal state so that the object can be restored to this state later without violating encapsulation.

```java
```

### 19. Observer
Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

```java
```

### 20. State
Allows an object to alter its behavior when its internal state changes. The object will appear to change its class.

```java
```

### 21. Strategy
Defines a family of algorithms, encapsulates each one, and makes them interchangeable. It lets the algorithm vary independently from the clients that use it.

```java
```

### 22. Template Method
Defines the skeleton of an algorithm in an operation, deferring some steps to subclasses. It lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.

```java
```

### 23. Visitor
Represents an operation to be performed on the elements of an object structure. It lets you define a new operation without changing the classes of the elements on which it operates.

```java
```