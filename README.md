# Java Design Patterns

The Gang of Four (GoF) design patterns, introduced in the book "Design Patterns: Elements of Reusable Object-Oriented Software" by Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides, are foundational to software engineering. 
They are categorized into three types: Creational, Structural, and Behavioral patterns.

## Creational Patterns

### 1. Abstract Factory
Provides an interface for creating families of related or dependent objects without specifying their concrete classes.

```java
// Abstract Product A
public interface ProductA {
    void use();
}

// Abstract Product B
public interface ProductB {
    void use();
}
// Concrete Product A1
public class ConcreteProductA1 implements ProductA {
    @Override
    public void use() {
        System.out.println("Using ConcreteProductA1");
    }
}

// Concrete Product A2
public class ConcreteProductA2 implements ProductA {
    @Override
    public void use() {
        System.out.println("Using ConcreteProductA2");
    }
}

// Concrete Product B1
public class ConcreteProductB1 implements ProductB {
    @Override
    public void use() {
        System.out.println("Using ConcreteProductB1");
    }
}

// Concrete Product B2
public class ConcreteProductB2 implements ProductB {
    @Override
    public void use() {
        System.out.println("Using ConcreteProductB2");
    }
}
// Abstract Factory
public interface AbstractFactory {
    ProductA createProductA();
    ProductB createProductB();
}
// Concrete Factory 1
public class ConcreteFactory1 implements AbstractFactory {
    @Override
    public ProductA createProductA() {
        return new ConcreteProductA1();
    }

    @Override
    public ProductB createProductB() {
        return new ConcreteProductB1();
    }
}

// Concrete Factory 2
public class ConcreteFactory2 implements AbstractFactory {
    @Override
    public ProductA createProductA() {
        return new ConcreteProductA2();
    }

    @Override
    public ProductB createProductB() {
        return new ConcreteProductB2();
    }
}
public class Main {
    public static void main(String[] args) {
        // Using ConcreteFactory1
        AbstractFactory factory1 = new ConcreteFactory1();
        ProductA productA1 = factory1.createProductA();
        ProductB productB1 = factory1.createProductB();
        productA1.use();
        productB1.use();

        // Using ConcreteFactory2
        AbstractFactory factory2 = new ConcreteFactory2();
        ProductA productA2 = factory2.createProductA();
        ProductB productB2 = factory2.createProductB();
        productA2.use();
        productB2.use();
    }
}
```

### 2. Builder
Separates the construction of a complex object from its representation, allowing the same construction process to create different representations.

```java
// Product class
public class Product {
    private String partA;
    private String partB;
    private String partC;

    public void setPartA(String partA) {
        this.partA = partA;
    }

    public void setPartB(String partB) {
        this.partB = partB;
    }

    public void setPartC(String partC) {
        this.partC = partC;
    }

    @Override
    public String toString() {
        return "Product [partA=" + partA + ", partB=" + partB + ", partC=" + partC + "]";
    }
}
// Builder interface
public interface Builder {
    void buildPartA();
    void buildPartB();
    void buildPartC();
    Product getResult();
}
// Concrete Builder 1
public class ConcreteBuilder1 implements Builder {
    private Product product = new Product();

    @Override
    public void buildPartA() {
        product.setPartA("Part A1");
    }

    @Override
    public void buildPartB() {
        product.setPartB("Part B1");
    }

    @Override
    public void buildPartC() {
        product.setPartC("Part C1");
    }

    @Override
    public Product getResult() {
        return product;
    }
}

// Concrete Builder 2
public class ConcreteBuilder2 implements Builder {
    private Product product = new Product();

    @Override
    public void buildPartA() {
        product.setPartA("Part A2");
    }

    @Override
    public void buildPartB() {
        product.setPartB("Part B2");
    }

    @Override
    public void buildPartC() {
        product.setPartC("Part C2");
    }

    @Override
    public Product getResult() {
        return product;
    }
}
// Director class
public class Director {
    private Builder builder;

    public void setBuilder(Builder builder) {
        this.builder = builder;
    }

    public Product construct() {
        builder.buildPartA();
        builder.buildPartB();
        builder.buildPartC();
        return builder.getResult();
    }
}
public class Main {
    public static void main(String[] args) {
        Director director = new Director();

        // Using ConcreteBuilder1
        Builder builder1 = new ConcreteBuilder1();
        director.setBuilder(builder1);
        Product product1 = director.construct();
        System.out.println(product1);

        // Using ConcreteBuilder2
        Builder builder2 = new ConcreteBuilder2();
        director.setBuilder(builder2);
        Product product2 = director.construct();
        System.out.println(product2);
    }
}
```

### 3. Factory Method
Defines an interface for creating an object but allows subclasses to alter the type of objects that will be created.

```java
// Product interface
public interface Product {
    void use();
}

// Concrete Product 1
public class ConcreteProduct1 implements Product {
    @Override
    public void use() {
        System.out.println("Using ConcreteProduct1");
    }
}

// Concrete Product 2
public class ConcreteProduct2 implements Product {
    @Override
    public void use() {
        System.out.println("Using ConcreteProduct2");
    }
}
// Factory interface
public interface Factory {
    Product createProduct();
}
// Concrete Factory 1
public class ConcreteFactory1 implements Factory {
    @Override
    public Product createProduct() {
        return new ConcreteProduct1();
    }
}

// Concrete Factory 2
public class ConcreteFactory2 implements Factory {
    @Override
    public Product createProduct() {
        return new ConcreteProduct2();
    }
}
public class Main {
    public static void main(String[] args) {
        Factory factory1 = new ConcreteFactory1();
        Product product1 = factory1.createProduct();
        product1.use();

        Factory factory2 = new ConcreteFactory2();
        Product product2 = factory2.createProduct();
        product2.use();
    }
    }
```

### 4. Prototype
Specifies the kinds of objects to create using a prototypical instance and creates new objects by copying this prototype.

```java
// Prototype interface
public interface Prototype {
    Prototype clone();
}
// Concrete Prototype 1
public class ConcretePrototype1 implements Prototype {
    private String field;

    public ConcretePrototype1(String field) {
        this.field = field;
    }

    @Override
    public Prototype clone() {
        return new ConcretePrototype1(this.field);
    }

    @Override
    public String toString() {
        return "ConcretePrototype1 [field=" + field + "]";
    }
}

// Concrete Prototype 2
public class ConcretePrototype2 implements Prototype {
    private int number;

    public ConcretePrototype2(int number) {
        this.number = number;
    }

    @Override
    public Prototype clone() {
        return new ConcretePrototype2(this.number);
    }

    @Override
    public String toString() {
        return "ConcretePrototype2 [number=" + number + "]";
    }
}
public class Main {
    public static void main(String[] args) {
        // Create initial prototypes
        ConcretePrototype1 prototype1 = new ConcretePrototype1("Prototype1");
        ConcretePrototype2 prototype2 = new ConcretePrototype2(42);

        // Clone prototypes
        Prototype clonedPrototype1 = prototype1.clone();
        Prototype clonedPrototype2 = prototype2.clone();

        // Display cloned prototypes
        System.out.println(clonedPrototype1);
        System.out.println(clonedPrototype2);
    }
}

```

### 5. Singleton
Ensures a class has only one instance and provides a global point of access to it.

```java
// Singleton class
public class Singleton {
    // Private static instance of the class
    private static Singleton instance;

    // Private constructor to prevent instantiation
    private Singleton() {}

    // Public method to provide access to the instance
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }

    public void showMessage() {
        System.out.println("Hello from the Singleton!");
    }
}

public class Main {
    public static void main(String[] args) {
        // Get the single instance of Singleton
        Singleton singleton = Singleton.getInstance();

        // Use the singleton instance
        singleton.showMessage();
    }
}
```

## Structural Patterns

### 6. Adapter
Converts the interface of a class into another interface clients expect. It allows classes to work together that couldn't otherwise because of incompatible interfaces.

```java
//
// DataLoader class that loads data
public class DataLoader {
    public String loadData() {
        return "Data loaded";
    }
}

// DataAnalyzer class that analyzes data
public class DataAnalyzer {
    public void analyze(String data) {
        System.out.println("Analyzing data: " + data);
    }
}

// Adapter interface
public interface DataAdapter {
    String getData();
}

// Adapter implementation
public class DataLoaderAdapter implements DataAdapter {
    private DataLoader dataLoader;

    public DataLoaderAdapter(DataLoader dataLoader) {
        this.dataLoader = dataLoader;
    }

    @Override
    public String getData() {
        return dataLoader.loadData();
    }
}

public class Main {
    public static void main(String[] args) {
        // Create objects
        DataLoader dataLoader = new DataLoader();
        DataAdapter adapter = new DataLoaderAdapter(dataLoader);
        DataAnalyzer dataAnalyzer = new DataAnalyzer();

        // Use the adapter
        String data = adapter.getData();
        dataAnalyzer.analyze(data);

    }
}
```

### 7. Bridge
Decouples an abstraction from its implementation so that the two can vary independently.

```java
// Implementor Interface
public interface Implementor {
    void operationImpl();
}

// Concrete Implementor 1
public class ConcreteImplementor1 implements Implementor {
    @Override
    public void operationImpl() {
        System.out.println("ConcreteImplementor1 operation");
    }
}

// Concrete Implementor 2
public class ConcreteImplementor2 implements Implementor {
    @Override
    public void operationImpl() {
        System.out.println("ConcreteImplementor2 operation");
    }
}

// Abstraction class
public abstract class Abstraction {
    protected Implementor implementor;

    protected Abstraction(Implementor implementor) {
        this.implementor = implementor;
    }

    public abstract void operation();
}

// Refined Abstraction
public class RefinedAbstraction extends Abstraction {
    public RefinedAbstraction(Implementor implementor) {
        super(implementor);
    }

    @Override
    public void operation() {
        implementor.operationImpl();
    }
}

public class Main {
    public static void main(String[] args) {
        Implementor implementor1 = new ConcreteImplementor1();
        Abstraction abstraction1 = new RefinedAbstraction(implementor1);
        abstraction1.operation();

        Implementor implementor2 = new ConcreteImplementor2();
        Abstraction abstraction2 = new RefinedAbstraction(implementor2);
        abstraction2.operation();
    }
}
```

### 8. Composite
Composes objects into tree structures to represent part-whole hierarchies, allowing clients to treat individual objects and compositions of objects uniformly.

```java
// Component interface
public interface Component {
    void operation();
}
// Leaf class
public class Leaf implements Component {
    private String name;

    public Leaf(String name) {
        this.name = name;
    }

    @Override
    public void operation() {
        System.out.println("Leaf " + name + " operation");
    }
}
// Composite class
public class Composite implements Component {
    private List<Component> children = new ArrayList<>();

    public void add(Component component) {
        children.add(component);
    }

    public void remove(Component component) {
        children.remove(component);
    }

    public Component getChild(int index) {
        return children.get(index);
    }

    @Override
    public void operation() {
        for (Component component : children) {
            component.operation();
        }
    }
}
public class Main {
    public static void main(String[] args) {
        // Create leaf components
        Component leaf1 = new Leaf("1");
        Component leaf2 = new Leaf("2");

        // Create composite component and add leaf components
        Composite composite = new Composite();
        composite.add(leaf1);
        composite.add(leaf2);

        // Create another composite component and add the first composite
        Composite composite2 = new Composite();
        composite2.add(composite);

        // Perform operations
        composite.operation();
        composite2.operation();
    }
}
```

### 9. Decorator
Adds additional responsibilities to an object dynamically. It provides a flexible alternative to subclassing for extending functionality.

```java
// Component interface
public interface Component {
    void operation();
}
// Concrete Component class
public class ConcreteComponent implements Component {
    @Override
    public void operation() {
        System.out.println("ConcreteComponent operation");
    }
}
// Decorator class
public abstract class Decorator implements Component {
    protected Component component;

    public Decorator(Component component) {
        this.component = component;
    }

    @Override
    public void operation() {
        component.operation();
    }
}
// Concrete Decorator 1
public class ConcreteDecorator1 extends Decorator {
    public ConcreteDecorator1(Component component) {
        super(component);
    }

    @Override
    public void operation() {
        super.operation();
        addedBehavior();
    }

    private void addedBehavior() {
        System.out.println("ConcreteDecorator1 added behavior");
    }
}

// Concrete Decorator 2
public class ConcreteDecorator2 extends Decorator {
    public ConcreteDecorator2(Component component) {
        super(component);
    }

    @Override
    public void operation() {
        super.operation();
        addedBehavior();
    }

    private void addedBehavior() {
        System.out.println("ConcreteDecorator2 added behavior");
    }
}
public class Main {
    public static void main(String[] args) {
        Component component = new ConcreteComponent();
        Component decorator1 = new ConcreteDecorator1(component);
        Component decorator2 = new ConcreteDecorator2(decorator1);

        decorator2.operation();
    }
}
```

### 10. Facade
Provides a unified interface to a set of interfaces in a subsystem, making the subsystem easier to use.

```java
// PaymentService class
public class PaymentService {
    public boolean processPayment(Order order) {
        System.out.println("Processing payment for order " + order.getId());
        return true;
    }
}

// InventoryService class
public class InventoryService {
    public boolean checkStock(Order order) {
        System.out.println("Checking stock for order " + order.getId());
        return true;
    }
}

// ShippingService class
public class ShippingService {
    public void ship(Order order) {
        System.out.println("Shipping order " + order.getId());
    }
}

// Order class
public class Order {
    private String id;

    public Order(String id) {
        this.id = id;
    }

    public String getId() {
        return id;
    }
}
// OrderFacade class
public class OrderFacade {
    private PaymentService paymentService;
    private InventoryService inventoryService;
    private ShippingService shippingService;

    public OrderFacade() {
        this.paymentService = new PaymentService();
        this.inventoryService = new InventoryService();
        this.shippingService = new ShippingService();
    }

    public boolean placeOrder(Order order) {
        if (paymentService.processPayment(order)) {
            if (inventoryService.checkStock(order)) {
                shippingService.ship(order);
                return true;
            }
        }
        return false;
    }
}

public class Main {
    public static void main(String[] args) {
        Order order = new Order("12345");
        OrderFacade facade = new OrderFacade();

        if (facade.placeOrder(order)) {
            System.out.println("Order placed successfully.");
        } else {
            System.out.println("Failed to place order.");
        }
    }
}
```

### 11. Flyweight
Uses sharing to support large numbers of fine-grained objects efficiently.

```java
// Flyweight interface
public interface Flyweight {
    void operation(String extrinsicState);
}
// Concrete Flyweight class
public class ConcreteFlyweight implements Flyweight {
    private String intrinsicState;

    public ConcreteFlyweight(String intrinsicState) {
        this.intrinsicState = intrinsicState;
    }

    @Override
    public void operation(String extrinsicState) {
        System.out.println("Intrinsic State: " + intrinsicState + ", Extrinsic State: " + extrinsicState);
    }
}
// Flyweight Factory class
public class FlyweightFactory {
    private Map<String, Flyweight> flyweights = new HashMap<>();

    public Flyweight getFlyweight(String key) {
        if (!flyweights.containsKey(key)) {
            flyweights.put(key, new ConcreteFlyweight(key));
        }
        return flyweights.get(key);
    }
}
public class Main {
    public static void main(String[] args) {
        FlyweightFactory factory = new FlyweightFactory();

        Flyweight flyweight1 = factory.getFlyweight("A");
        flyweight1.operation("First Call");

        Flyweight flyweight2 = factory.getFlyweight("B");
        flyweight2.operation("Second Call");

        Flyweight flyweight3 = factory.getFlyweight("A");
        flyweight3.operation("Third Call");
    }
}
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
// Strategy interface
public interface Strategy {
    void execute();
}
// Concrete Strategy 1
public class ConcreteStrategy1 implements Strategy {
    @Override
    public void execute() {
        System.out.println("Executing strategy 1");
    }
}

// Concrete Strategy 2
public class ConcreteStrategy2 implements Strategy {
    @Override
    public void execute() {
        System.out.println("Executing strategy 2");
    }
}
// Context class
public class Context {
    private Strategy strategy;

    public void setStrategy(Strategy strategy) {
        this.strategy = strategy;
    }

    public void executeStrategy() {
        strategy.execute();
    }
}
public class Main {
    public static void main(String[] args) {
        Context context = new Context();

        // Using ConcreteStrategy1
        Strategy strategy1 = new ConcreteStrategy1();
        context.setStrategy(strategy1);
        context.executeStrategy();

        // Using ConcreteStrategy2
        Strategy strategy2 = new ConcreteStrategy2();
        context.setStrategy(strategy2);
        context.executeStrategy();
    }
}
```

### 22. Template Method
Defines the skeleton of an algorithm in an operation, deferring some steps to subclasses. It lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.

```java
```

### 23. Visitor
Represents an operation to be performed on the elements of an object structure. It lets you define a new operation without changing the classes of the elements on which it operates.

```java
```