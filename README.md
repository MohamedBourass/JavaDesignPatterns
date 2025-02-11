# Java Design Patterns

The Gang of Four (GoF) design patterns, introduced in the book "Design Patterns: Elements of Reusable Object-Oriented Software" by Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides, are foundational to software engineering. 
They are categorized into three types: Creational, Structural, and Behavioral patterns.

## Creational Patterns

### 1. Abstract Factory
<details>
  <summary>Provides an interface for creating families of related or dependent objects without specifying their concrete classes.</summary>

```java
// Abstract Factory interface
public interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

// Concrete Factory classes
public class WinFactory implements GUIFactory {

    @Override
    public Button createButton() {
        return new WinButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new WinCheckbox();
    }
}

public class MacFactory implements GUIFactory {

    @Override
    public Button createButton() {
        return new MacButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}

// Abstract Product interfaces
public interface Button {
    void paint();
}

public interface Checkbox {
    void paint();
}

// Concrete Product classes
public class WinButton implements Button {

    @Override
    public void paint() {
        System.out.println("You have created a Windows Button.");
    }
}

public class MacButton implements Button {

    @Override
    public void paint() {
        System.out.println("You have created a MacOS Button.");
    }
}

public class WinCheckbox implements Checkbox {

    @Override
    public void paint() {
        System.out.println("You have created a Windows Checkbox.");
    }
}

public class MacCheckbox implements Checkbox {

    @Override
    public void paint() {
        System.out.println("You have created a MacOS Checkbox.");
    }
}

// Main class to demonstrate Abstract Factory Pattern
public class AbstractFactoryPatternDemo {
    private static Application configureApplication() {
        Application app;
        GUIFactory factory;

        String osName = System.getProperty("os.name").toLowerCase();
        if (osName.contains("win")) {
            factory = new WinFactory();
        } else {
            factory = new MacFactory();
        }
        app = new Application(factory);
        return app;
    }

    public static void main(String[] args) {
        Application app = configureApplication();
        app.paint();
    }
}

// Application class that uses the Abstract Factory
public class Application {
    private Button button;
    private Checkbox checkbox;

    public Application(GUIFactory factory) {
        button = factory.createButton();
        checkbox = factory.createCheckbox();
    }

    public void paint() {
        button.paint();
        checkbox.paint();
    }
}
```
</details>




### 2. Builder
<details>
  <summary>Separates the construction of a complex object from its representation, allowing the same construction process to create different representations.</summary>

```java
// Item interface
public interface Item {
    String name();
    Packing packing();
    float price();
}

// Packing interface
public interface Packing {
    String pack();
}

// Concrete Packing classes
public class Wrapper implements Packing {
    @Override
    public String pack() {
        return "Wrapper";
    }
}

public class Bottle implements Packing {
    @Override
    public String pack() {
        return "Bottle";
    }
}

// Abstract class implementing Item interface providing default behavior
public abstract class Burger implements Item {
    @Override
    public Packing packing() {
        return new Wrapper();
    }

    @Override
    public abstract float price();
}

public abstract class Drink implements Item {
    @Override
    public Packing packing() {
        return new Bottle();
    }

    @Override
    public abstract float price();
}

// Concrete classes
public class VegBurger extends Burger {
    @Override
    public float price() {
        return 25.0f;
    }

    @Override
    public String name() {
        return "Veg Burger";
    }
}

public class ChickenBurger extends Burger {
    @Override
    public float price() {
        return 50.5f;
    }

    @Override
    public String name() {
        return "Chicken Burger";
    }
}

public class Coke extends Drink {
    @Override
    public float price() {
        return 30.0f;
    }

    @Override
    public String name() {
        return "Coke";
    }
}

public class Pepsi extends Drink {
    @Override
    public float price() {
        return 35.0f;
    }

    @Override
    public String name() {
        return "Pepsi";
    }
}

// Meal class containing a list of Items
import java.util.ArrayList;
import java.util.List;

public class Meal {
    private List<Item> items = new ArrayList<Item>();

    public void addItem(Item item) {
        items.add(item);
    }

    public float getCost() {
        float cost = 0.0f;
        for (Item item : items) {
            cost += item.price();
        }
        return cost;
    }

    public void showItems() {
        for (Item item : items) {
            System.out.print("Item : " + item.name());
            System.out.print(", Packing : " + item.packing().pack());
            System.out.println(", Price : " + item.price());
        }
    }
}

// MealBuilder class to construct a Meal
public class MealBuilder {
    public Meal prepareVegMeal() {
        Meal meal = new Meal();
        meal.addItem(new VegBurger());
        meal.addItem(new Coke());
        return meal;
    }

    public Meal prepareNonVegMeal() {
        Meal meal = new Meal();
        meal.addItem(new ChickenBurger());
        meal.addItem(new Pepsi());
        return meal;
    }
}

// Main class to demonstrate Builder Pattern
public class BuilderPatternDemo {
    public static void main(String[] args) {
        MealBuilder mealBuilder = new MealBuilder();

        Meal vegMeal = mealBuilder.prepareVegMeal();
        System.out.println("Veg Meal");
        vegMeal.showItems();
        System.out.println("Total Cost: " + vegMeal.getCost());

        Meal nonVegMeal = mealBuilder.prepareNonVegMeal();
        System.out.println("\nNon-Veg Meal");
        nonVegMeal.showItems();
        System.out.println("Total Cost: " + nonVegMeal.getCost());
    }
}
```
</details>

### 3. Factory Method
<details>
  <summary>Defines an interface for creating an object but allows subclasses to alter the type of objects that will be created.</summary>

```java
// Shape interface
public interface Shape {
    void draw();
}

// Concrete Shape classes
public class Circle implements Shape {

    @Override
    public void draw() {
        System.out.println("Inside Circle::draw() method.");
    }
}

public class Rectangle implements Shape {

    @Override
    public void draw() {
        System.out.println("Inside Rectangle::draw() method.");
    }
}

public class Square implements Shape {

    @Override
    public void draw() {
        System.out.println("Inside Square::draw() method.");
    }
}

// Factory class
public class ShapeFactory {

    // Use getShape method to get object of type shape
    public Shape getShape(String shapeType) {
        if (shapeType == null) {
            return null;
        }
        if (shapeType.equalsIgnoreCase("CIRCLE")) {
            return new Circle();
        } else if (shapeType.equalsIgnoreCase("RECTANGLE")) {
            return new Rectangle();
        } else if (shapeType.equalsIgnoreCase("SQUARE")) {
            return new Square();
        }
        return null;
    }
}

// Main class to demonstrate Factory Method Pattern
public class FactoryPatternDemo {
    public static void main(String[] args) {
        ShapeFactory shapeFactory = new ShapeFactory();

        // Get an object of Circle and call its draw method
        Shape shape1 = shapeFactory.getShape("CIRCLE");
        shape1.draw();

        // Get an object of Rectangle and call its draw method
        Shape shape2 = shapeFactory.getShape("RECTANGLE");
        shape2.draw();

        // Get an object of Square and call its draw method
        Shape shape3 = shapeFactory.getShape("SQUARE");
        shape3.draw();
    }
}
```
</details>

### 4. Prototype

<details>
  <summary>Specifies the kinds of objects to create using a prototypical instance and creates new objects by copying this prototype.</summary>

```java
// Abstract class
public abstract class Shape implements Cloneable {
    private String id;
    protected String type;

    abstract void draw();

    public String getType() {
        return type;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    @Override
    public Object clone() {
        Object clone = null;
        try {
            clone = super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return clone;
    }
}

// Concrete Shape classes
public class Rectangle extends Shape {

    public Rectangle() {
        type = "Rectangle";
    }

    @Override
    public void draw() {
        System.out.println("Inside Rectangle::draw() method.");
    }
}

public class Circle extends Shape {

    public Circle() {
        type = "Circle";
    }

    @Override
    public void draw() {
        System.out.println("Inside Circle::draw() method.");
    }
}

// ShapeCache class
public class ShapeCache {

    private static Hashtable<String, Shape> shapeMap = new Hashtable<String, Shape>();

    public static Shape getShape(String shapeId) {
        Shape cachedShape = shapeMap.get(shapeId);
        return (Shape) cachedShape.clone();
    }

    // For each shape run database query and create shape
    // shapeMap.put(shapeKey, shape);
    // For example, adding three shapes
    public static void loadCache() {
        Circle circle = new Circle();
        circle.setId("1");
        shapeMap.put(circle.getId(), circle);

        Rectangle rectangle = new Rectangle();
        rectangle.setId("2");
        shapeMap.put(rectangle.getId(), rectangle);
    }
}

// Main class to demonstrate Prototype Pattern
public class PrototypePatternDemo {
    public static void main(String[] args) {
        ShapeCache.loadCache();

        Shape clonedShape1 = (Shape) ShapeCache.getShape("1");
        System.out.println("Shape : " + clonedShape1.getType());
        clonedShape1.draw();

        Shape clonedShape2 = (Shape) ShapeCache.getShape("2");
        System.out.println("Shape : " + clonedShape2.getType());
        clonedShape2.draw();
    }
}
```
</details>

### 5. Singleton

<details>
  <summary>Ensures a class has only one instance and provides a global point of access to it.</summary>

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
</details>

## Structural Patterns

### 6. Adapter

<details>
  <summary>Converts the interface of a class into another interface clients expect. It allows classes to work together that couldn't otherwise because of incompatible interfaces.</summary>

```java

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
</details>

### 7. Bridge

<details>
  <summary>Decouples an abstraction from its implementation so that the two can vary independently.</summary>

```java
// Color interface
public interface Color {
    void applyColor();
}

// Concrete Color classes
public class Red implements Color {

    @Override
    public void applyColor() {
        System.out.println("Applying red color");
    }
}

public class Green implements Color {

    @Override
    public void applyColor() {
        System.out.println("Applying green color");
    }
}

// Shape abstract class
public abstract class Shape {
    protected Color color;

    protected Shape(Color color) {
        this.color = color;
    }

    abstract public void draw();
}

// Concrete Shape classes
public class Circle extends Shape {

    public Circle(Color color) {
        super(color);
    }

    @Override
    public void draw() {
        System.out.print("Drawing Circle with color ");
        color.applyColor();
    }
}

public class Rectangle extends Shape {

    public Rectangle(Color color) {
        super(color);
    }

    @Override
    public void draw() {
        System.out.print("Drawing Rectangle with color ");
        color.applyColor();
    }
}

// Main class to demonstrate Bridge Pattern
public class BridgePatternDemo {
    public static void main(String[] args) {
        Shape redCircle = new Circle(new Red());
        Shape greenRectangle = new Rectangle(new Green());

        redCircle.draw();
        greenRectangle.draw();
    }
}
```
</details>

### 8. Composite

<details>
  <summary>Composes objects into tree structures to represent part-whole hierarchies, allowing clients to treat individual objects and compositions of objects uniformly.</summary>

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
</details>

### 9. Decorator

<details>
  <summary>Adds additional responsibilities to an object dynamically. It provides a flexible alternative to subclassing for extending functionality.</summary>

```java
// Shape interface
public interface Shape {
    void draw();
}

// Concrete Shape classes
public class Circle implements Shape {

    @Override
    public void draw() {
        System.out.println("Shape: Circle");
    }
}

public class Rectangle implements Shape {

    @Override
    public void draw() {
        System.out.println("Shape: Rectangle");
    }
}

// Abstract Decorator class
public abstract class ShapeDecorator implements Shape {
    protected Shape decoratedShape;

    public ShapeDecorator(Shape decoratedShape) {
        this.decoratedShape = decoratedShape;
    }

    public void draw() {
        decoratedShape.draw();
    }
}

// Concrete Decorator class
public class RedShapeDecorator extends ShapeDecorator {

    public RedShapeDecorator(Shape decoratedShape) {
        super(decoratedShape);
    }

    @Override
    public void draw() {
        decoratedShape.draw();
        setRedBorder(decoratedShape);
    }

    private void setRedBorder(Shape decoratedShape) {
        System.out.println("Border Color: Red");
    }
}

// Main class to demonstrate Decorator Pattern
public class DecoratorPatternDemo {
    public static void main(String[] args) {
        Shape circle = new Circle();
        Shape redCircle = new RedShapeDecorator(new Circle());
        Shape redRectangle = new RedShapeDecorator(new Rectangle());

        System.out.println("Circle with normal border");
        circle.draw();

        System.out.println("\nCircle with red border");
        redCircle.draw();

        System.out.println("\nRectangle with red border");
        redRectangle.draw();
    }
}
```
</details>

### 10. Facade

<details>
  <summary>Provides a unified interface to a set of interfaces in a subsystem, making the subsystem easier to use.</summary>

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
</details>

### 11. Flyweight

<details>
  <summary>Uses sharing to support large numbers of fine-grained objects efficiently.</summary>

```java
// Shape interface
public interface Shape {
    void draw();
}

// Concrete Flyweight class
public class Circle implements Shape {
    private String color;
    private int x;
    private int y;
    private int radius;

    public Circle(String color) {
        this.color = color;
    }

    public void setX(int x) {
        this.x = x;
    }

    public void setY(int y) {
        this.y = y;
    }

    public void setRadius(int radius) {
        this.radius = radius;
    }

    @Override
    public void draw() {
        System.out.println("Circle: Draw() [Color : " + color + ", x : " + x + ", y :" + y + ", radius :" + radius);
    }
}

// Flyweight Factory class
import java.util.HashMap;

public class ShapeFactory {
    private static final HashMap<String, Shape> circleMap = new HashMap<>();

    public static Shape getCircle(String color) {
        Circle circle = (Circle) circleMap.get(color);

        if (circle == null) {
            circle = new Circle(color);
            circleMap.put(color, circle);
            System.out.println("Creating circle of color : " + color);
        }
        return circle;
    }
}

// Main class to demonstrate Flyweight Pattern
public class FlyweightPatternDemo {
    private static final String colors[] = {"Red", "Green", "Blue", "White", "Black"};

    public static void main(String[] args) {

        for (int i = 0; i < 20; ++i) {
            Circle circle = (Circle) ShapeFactory.getCircle(getRandomColor());
            circle.setX(getRandomX());
            circle.setY(getRandomY());
            circle.setRadius(100);
            circle.draw();
        }
    }

    private static String getRandomColor() {
        return colors[(int) (Math.random() * colors.length)];
    }

    private static int getRandomX() {
        return (int) (Math.random() * 100);
    }

    private static int getRandomY() {
        return (int) (Math.random() * 100);
    }
}
```
</details>

### 12. Proxy

<details>
  <summary>Provides a surrogate or placeholder for another object to control access to it.</summary>

```java
// Image interface
public interface Image {
    void display();
}

// RealImage class
public class RealImage implements Image {
    private String fileName;

    public RealImage(String fileName) {
        this.fileName = fileName;
        loadFromDisk(fileName);
    }

    private void loadFromDisk(String fileName) {
        System.out.println("Loading " + fileName);
    }

    @Override
    public void display() {
        System.out.println("Displaying " + fileName);
    }
}

// ProxyImage class
public class ProxyImage implements Image {
    private RealImage realImage;
    private String fileName;

    public ProxyImage(String fileName) {
        this.fileName = fileName;
    }

    @Override
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(fileName);
        }
        realImage.display();
    }
}

// Main class to demonstrate Proxy Pattern
public class ProxyPatternDemo {
    public static void main(String[] args) {
        Image image = new ProxyImage("test_image.jpg");

        // Image will be loaded from disk and displayed
        image.display();
        System.out.println("");

        // Image will not be loaded from disk and will be displayed directly
        image.display();
    }
}
```
</details>

## Behavioral Patterns

### 13. Chain of Responsibility

<details>
  <summary>Passes a request along a chain of handlers. Each handler either processes the request or passes it to the next handler in the chain.</summary>

```java
// Logger abstract class
public abstract class Logger {
    public static int INFO = 1;
    public static int DEBUG = 2;
    public static int ERROR = 3;

    protected int level;

    // next element in chain of responsibility
    protected Logger nextLogger;

    public void setNextLogger(Logger nextLogger) {
        this.nextLogger = nextLogger;
    }

    public void logMessage(int level, String message) {
        if (this.level <= level) {
            write(message);
        }
        if (nextLogger != null) {
            nextLogger.logMessage(level, message);
        }
    }

    abstract protected void write(String message);
}

// ConsoleLogger class
public class ConsoleLogger extends Logger {

    public ConsoleLogger(int level) {
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("Standard Console::Logger: " + message);
    }
}

// ErrorLogger class
public class ErrorLogger extends Logger {

    public ErrorLogger(int level) {
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("Error Console::Logger: " + message);
    }
}

// FileLogger class
public class FileLogger extends Logger {

    public FileLogger(int level) {
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("File::Logger: " + message);
    }
}

// ChainPatternDemo class to demonstrate Chain of Responsibility Pattern
public class ChainPatternDemo {

    private static Logger getChainOfLoggers() {

        Logger errorLogger = new ErrorLogger(Logger.ERROR);
        Logger fileLogger = new FileLogger(Logger.DEBUG);
        Logger consoleLogger = new ConsoleLogger(Logger.INFO);

        errorLogger.setNextLogger(fileLogger);
        fileLogger.setNextLogger(consoleLogger);

        return errorLogger;
    }

    public static void main(String[] args) {
        Logger loggerChain = getChainOfLoggers();

        loggerChain.logMessage(Logger.INFO,
                "This is an information.");

        loggerChain.logMessage(Logger.DEBUG,
                "This is a debug level information.");

        loggerChain.logMessage(Logger.ERROR,
                "This is an error information.");
    }
}
```
</details>

### 14. Command

<details>
  <summary>Encapsulates a request as an object, thereby allowing for parameterization of clients with queues, requests, and operations.</summary>

```java
// Command interface
public interface Command {
    void execute();
}

// Light class (Receiver)
public class Light {
    public void turnOn() {
        System.out.println("The light is on");
    }

    public void turnOff() {
        System.out.println("The light is off");
    }
}

// Concrete Command for turning on the light
public class LightOnCommand implements Command {
    private Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOn();
    }
}

// Concrete Command for turning off the light
public class LightOffCommand implements Command {
    private Light light;

    public LightOffCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOff();
    }
}

// Invoker class
public class RemoteControl {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        command.execute();
    }
}

// Main class to demonstrate Command Pattern
public class CommandPatternDemo {
    public static void main(String[] args) {
        Light livingRoomLight = new Light();

        // Creating commands
        Command lightOn = new LightOnCommand(livingRoomLight);
        Command lightOff = new LightOffCommand(livingRoomLight);

        // Invoker
        RemoteControl remote = new RemoteControl();

        // Turn on the light
        remote.setCommand(lightOn);
        remote.pressButton();

        // Turn off the light
        remote.setCommand(lightOff);
        remote.pressButton();
    }
}
```
</details>

### 15. Interpreter

<details>
  <summary>Defines a representation for a language's grammar along with an interpreter that uses the representation to interpret sentences in the language.</summary>

```java
// Expression interface
public interface Expression {
    boolean interpret(String context);
}

// TerminalExpression class
public class TerminalExpression implements Expression {
    private String data;

    public TerminalExpression(String data) {
        this.data = data;
    }

    @Override
    public boolean interpret(String context) {
        return context.contains(data);
    }
}

// OrExpression class
public class OrExpression implements Expression {
    private Expression expr1;
    private Expression expr2;

    public OrExpression(Expression expr1, Expression expr2) {
        this.expr1 = expr1;
        this.expr2 = expr2;
    }

    @Override
    public boolean interpret(String context) {
        return expr1.interpret(context) || expr2.interpret(context);
    }
}

// AndExpression class
public class AndExpression implements Expression {
    private Expression expr1;
    private Expression expr2;

    public AndExpression(Expression expr1, Expression expr2) {
        this.expr1 = expr1;
        this.expr2 = expr2;
    }

    @Override
    public boolean interpret(String context) {
        return expr1.interpret(context) && expr2.interpret(context);
    }
}

// Main class to demonstrate Interpreter Pattern
public class InterpreterPatternDemo {
    // Rule: Robert and John are male
    public static Expression getMaleExpression() {
        Expression robert = new TerminalExpression("Robert");
        Expression john = new TerminalExpression("John");
        return new OrExpression(robert, john);
    }

    // Rule: Julie is a married woman
    public static Expression getMarriedWomanExpression() {
        Expression julie = new TerminalExpression("Julie");
        Expression married = new TerminalExpression("Married");
        return new AndExpression(julie, married);
    }

    public static void main(String[] args) {
        Expression isMale = getMaleExpression();
        Expression isMarriedWoman = getMarriedWomanExpression();

        System.out.println("John is male? " + isMale.interpret("John"));
        System.out.println("Julie is a married woman? " + isMarriedWoman.interpret("Married Julie"));
    }
}
```
</details>

### 16. Iterator

<details>
  <summary>Provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.</summary>

```java
// Iterator interface
public interface Iterator {
    boolean hasNext();
    Object next();
}

// Container interface
public interface Container {
    Iterator getIterator();
}

// Concrete collection class
public class NameRepository implements Container {
    public String names[] = {"Robert", "John", "Julie", "Lora"};

    @Override
    public Iterator getIterator() {
        return new NameIterator();
    }

    // Concrete iterator class
    private class NameIterator implements Iterator {
        int index;

        @Override
        public boolean hasNext() {
            if (index < names.length) {
                return true;
            }
            return false;
        }

        @Override
        public Object next() {
            if (this.hasNext()) {
                return names[index++];
            }
            return null;
        }
    }
}

// Main class to demonstrate Iterator Pattern
public class IteratorPatternDemo {
    public static void main(String[] args) {
        NameRepository namesRepository = new NameRepository();

        for (Iterator iter = namesRepository.getIterator(); iter.hasNext(); ) {
            String name = (String) iter.next();
            System.out.println("Name : " + name);
        }
    }
}
```
</details>

### 17. Mediator


<details>
  <summary>Defines an object that encapsulates how a set of objects interact, promoting loose coupling by keeping objects from referring to each other explicitly.</summary>

```java
// Mediator interface
public interface ChatRoomMediator {
    void showMessage(User user, String message);
}

// Concrete Mediator class
public class ChatRoom implements ChatRoomMediator {

    @Override
    public void showMessage(User user, String message) {
        System.out.println(user.getName() + ": " + message);
    }
}

// User class
public abstract class User {
    protected ChatRoomMediator mediator;
    protected String name;

    public User(ChatRoomMediator med, String name) {
        this.mediator = med;
        this.name = name;
    }

    public abstract void send(String message);
    public abstract void receive(String message);

    public String getName() {
        return name;
    }
}

// Concrete User class
public class UserImpl extends User {

    public UserImpl(ChatRoomMediator med, String name) {
        super(med, name);
    }

    @Override
    public void send(String message) {
        System.out.println(this.name + " sends: " + message);
        mediator.showMessage(this, message);
    }

    @Override
    public void receive(String message) {
        System.out.println(this.name + " receives: " + message);
    }
}

// Main class to demonstrate Mediator Pattern
public class MediatorPatternDemo {
    public static void main(String[] args) {
        ChatRoom chatRoom = new ChatRoom();

        User robert = new UserImpl(chatRoom, "Robert");
        User john = new UserImpl(chatRoom, "John");

        robert.send("Hi John!");
        john.send("Hello Robert!");
    }
}
```
</details>

### 18. Memento

<details>
  <summary>Captures and externalizes an object's internal state so that the object can be restored to this state later without violating encapsulation.</summary>

```java
// Memento class
public class Memento {
    private String state;

    public Memento(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }
}

// Originator class
public class Originator {
    private String state;

    public void setState(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }

    public Memento saveStateToMemento() {
        return new Memento(state);
    }

    public void getStateFromMemento(Memento memento) {
        state = memento.getState();
    }
}

// Caretaker class
import java.util.ArrayList;
import java.util.List;

public class Caretaker {
    private List<Memento> mementoList = new ArrayList<Memento>();

    public void add(Memento state) {
        mementoList.add(state);
    }

    public Memento get(int index) {
        return mementoList.get(index);
    }
}

// Main class to demonstrate Memento Pattern
public class MementoPatternDemo {
    public static void main(String[] args) {
        Originator originator = new Originator();
        Caretaker caretaker = new Caretaker();

        originator.setState("State #1");
        originator.setState("State #2");
        caretaker.add(originator.saveStateToMemento());

        originator.setState("State #3");
        caretaker.add(originator.saveStateToMemento());

        originator.setState("State #4");
        System.out.println("Current State: " + originator.getState());

        originator.getStateFromMemento(caretaker.get(0));
        System.out.println("First saved State: " + originator.getState());
        originator.getStateFromMemento(caretaker.get(1));
        System.out.println("Second saved State: " + originator.getState());
    }
}
```
</details>

### 19. Observer

<details>
  <summary>Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.</summary>

```java
// Observer interface
public interface Observer {
    void update(float temperature, float humidity, float pressure);
}

// Subject interface
public interface Subject {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers();
}

// Concrete Subject class
import java.util.ArrayList;
import java.util.List;

public class WeatherStation implements Subject {
    private List<Observer> observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherStation() {
        observers = new ArrayList<>();
    }

    @Override
    public void registerObserver(Observer o) {
        observers.add(o);
    }

    @Override
    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(temperature, humidity, pressure);
        }
    }

    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        measurementsChanged();
    }

    private void measurementsChanged() {
        notifyObservers();
    }
}

// Concrete Observer class
public class CurrentConditionsDisplay implements Observer {
    private float temperature;
    private float humidity;

    @Override
    public void update(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        display();
    }

    public void display() {
        System.out.println("Current conditions: " + temperature + "F degrees and " + humidity + "% humidity");
    }
}

// Concrete Observer class
public class StatisticsDisplay implements Observer {
    private float maxTemp = 0.0f;
    private float minTemp = 200;
    private float tempSum = 0.0f;
    private int numReadings;

    @Override
    public void update(float temperature, float humidity, float pressure) {
        tempSum += temperature;
        numReadings++;

        if (temperature > maxTemp) {
            maxTemp = temperature;
        }

        if (temperature < minTemp) {
            minTemp = temperature;
        }

        display();
    }

    public void display() {
        System.out.println("Avg/Max/Min temperature = " + (tempSum / numReadings) + "/" + maxTemp + "/" + minTemp);
    }
}

// Main class to demonstrate Observer Pattern
public class ObserverPatternDemo {
    public static void main(String[] args) {
        WeatherStation weatherStation = new WeatherStation();

        CurrentConditionsDisplay currentDisplay = new CurrentConditionsDisplay();
        StatisticsDisplay statisticsDisplay = new StatisticsDisplay();

        weatherStation.registerObserver(currentDisplay);
        weatherStation.registerObserver(statisticsDisplay);

        weatherStation.setMeasurements(80, 65, 30.4f);
        weatherStation.setMeasurements(82, 70, 29.2f);
        weatherStation.setMeasurements(78, 90, 29.2f);
    }
}
```
</details>

### 20. State

<details>
  <summary>Allows an object to alter its behavior when its internal state changes. The object will appear to change its class.</summary>

```java
// State interface
public interface State {
    void doAction(TrafficLightContext context);
}

// Context class
public class TrafficLightContext {
    private State currentState;

    public TrafficLightContext() {
        currentState = null;
    }

    public void setState(State state) {
        this.currentState = state;
    }

    public State getState() {
        return currentState;
    }

    public void request() {
        currentState.doAction(this);
    }
}

// Concrete State classes
public class RedState implements State {
    @Override
    public void doAction(TrafficLightContext context) {
        System.out.println("Traffic Light is Red. Stop!");
        context.setState(new GreenState());
    }
}

public class GreenState implements State {
    @Override
    public void doAction(TrafficLightContext context) {
        System.out.println("Traffic Light is Green. Go!");
        context.setState(new YellowState());
    }
}

public class YellowState implements State {
    @Override
    public void doAction(TrafficLightContext context) {
        System.out.println("Traffic Light is Yellow. Slow down!");
        context.setState(new RedState());
    }
}

// Main class to demonstrate State Pattern
public class StatePatternDemo {
    public static void main(String[] args) {
        TrafficLightContext context = new TrafficLightContext();

        RedState red = new RedState();
        red.doAction(context);

        System.out.println("Current State: " + context.getState().getClass().getSimpleName());

        context.request();
        System.out.println("Current State: " + context.getState().getClass().getSimpleName());

        context.request();
        System.out.println("Current State: " + context.getState().getClass().getSimpleName());
    }
}
```
</details>

### 21. Strategy

<details>
  <summary>Defines a family of algorithms, encapsulates each one, and makes them interchangeable. It lets the algorithm vary independently from the clients that use it.</summary>

```java
// Strategy interface
public interface PaymentStrategy {
    void pay(int amount);
}

// Concrete Strategy class for Credit Card payment
public class CreditCardPayment implements PaymentStrategy {
    private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpiry;

    public CreditCardPayment(String name, String cardNumber, String cvv, String dateOfExpiry) {
        this.name = name;
        this.cardNumber = cardNumber;
        this.cvv = cvv;
        this.dateOfExpiry = dateOfExpiry;
    }

    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid with credit card.");
    }
}

// Concrete Strategy class for PayPal payment
public class PaypalPayment implements PaymentStrategy {
    private String emailId;
    private String password;

    public PaypalPayment(String emailId, String password) {
        this.emailId = emailId;
        this.password = password;
    }

    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using PayPal.");
    }
}

// Context class
import java.util.ArrayList;
import java.util.List;

public class ShoppingCart {
    List<Item> items;

    public ShoppingCart() {
        this.items = new ArrayList<>();
    }

    public void addItem(Item item) {
        this.items.add(item);
    }

    public void removeItem(Item item) {
        this.items.remove(item);
    }

    public int calculateTotal() {
        int sum = 0;
        for (Item item : items) {
            sum += item.getPrice();
        }
        return sum;
    }

    public void pay(PaymentStrategy paymentMethod) {
        int amount = calculateTotal();
        paymentMethod.pay(amount);
    }
}

// Item class
public class Item {
    private String name;
    private int price;

    public Item(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }
}

// Main class to demonstrate Strategy Pattern
public class StrategyPatternDemo {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        Item item1 = new Item("Book", 10);
        Item item2 = new Item("Pen", 5);

        cart.addItem(item1);
        cart.addItem(item2);

        // Pay using Credit Card
        cart.pay(new CreditCardPayment("John Doe", "1234567890123456", "786", "12/23"));

        // Pay using PayPal
        cart.pay(new PaypalPayment("johndoe@example.com", "password"));
    }
}
```
</details>

### 22. Template Method

<details>
  <summary>Defines the skeleton of an algorithm in an operation, deferring some steps to subclasses. It lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.</summary>

```java
// Abstract class
public abstract class DataProcessor {
    // Template method
    public final void process() {
        readData();
        processData();
        saveData();
    }

    abstract void readData();
    abstract void processData();

    // Concrete method
    public void saveData() {
        System.out.println("Saving data to database");
    }
}

// Concrete class for processing CSV data
public class CSVDataProcessor extends DataProcessor {
    @Override
    void readData() {
        System.out.println("Reading data from CSV file");
    }

    @Override
    void processData() {
        System.out.println("Processing CSV data");
    }
}

// Concrete class for processing XML data
public class XMLDataProcessor extends DataProcessor {
    @Override
    void readData() {
        System.out.println("Reading data from XML file");
    }

    @Override
    void processData() {
        System.out.println("Processing XML data");
    }
}

// Main class to demonstrate Template Method Pattern
public class TemplateMethodPatternDemo {
    public static void main(String[] args) {
        DataProcessor csvProcessor = new CSVDataProcessor();
        csvProcessor.process();

        System.out.println();

        DataProcessor xmlProcessor = new XMLDataProcessor();
        xmlProcessor.process();
    }
}
```
</details>

### 23. Visitor

<details>
  <summary>Represents an operation to be performed on the elements of an object structure. It lets you define a new operation without changing the classes of the elements on which it operates.</summary>

```java
// Visitor interface
public interface ComputerPartVisitor {
    void visit(Computer computer);
    void visit(Mouse mouse);
    void visit(Keyboard keyboard);
    void visit(Monitor monitor);
}

// ComputerPart interface
public interface ComputerPart {
    void accept(ComputerPartVisitor computerPartVisitor);
}

// Concrete ComputerPart classes
public class Keyboard implements ComputerPart {

    @Override
    public void accept(ComputerPartVisitor computerPartVisitor) {
        computerPartVisitor.visit(this);
    }
}

public class Monitor implements ComputerPart {

    @Override
    public void accept(ComputerPartVisitor computerPartVisitor) {
        computerPartVisitor.visit(this);
    }
}

public class Mouse implements ComputerPart {

    @Override
    public void accept(ComputerPartVisitor computerPartVisitor) {
        computerPartVisitor.visit(this);
    }
}

public class Computer implements ComputerPart {

    ComputerPart[] parts;

    public Computer() {
        parts = new ComputerPart[] {new Mouse(), new Keyboard(), new Monitor()};
    }

    @Override
    public void accept(ComputerPartVisitor computerPartVisitor) {
        for (int i = 0; i < parts.length; i++) {
            parts[i].accept(computerPartVisitor);
        }
        computerPartVisitor.visit(this);
    }
}

// Concrete Visitor class
public class ComputerPartDisplayVisitor implements ComputerPartVisitor {

    @Override
    public void visit(Computer computer) {
        System.out.println("Displaying Computer.");
    }

    @Override
    public void visit(Mouse mouse) {
        System.out.println("Displaying Mouse.");
    }

    @Override
    public void visit(Keyboard keyboard) {
        System.out.println("Displaying Keyboard.");
    }

    @Override
    public void visit(Monitor monitor) {
        System.out.println("Displaying Monitor.");
    }
}

// Main class to demonstrate Visitor Pattern
public class VisitorPatternDemo {
    public static void main(String[] args) {
        ComputerPart computer = new Computer();
        computer.accept(new ComputerPartDisplayVisitor());
    }
}
```
</details>