# SOLID Principles in Low-Level Design (LLD)

SOLID principles are a set of fundamental guidelines for designing software systems that are maintainable, extensible, and flexible. These principles were introduced by Robert C. Martin (Uncle Bob) and have become widely adopted in the object-oriented programming community. In this Markdown file, we will explore each principle, provide real-life examples, and discuss how they contribute to better low-level design.

## 1. Single Responsibility Principle (SRP)

The Single Responsibility Principle states that a class should have only one reason to change. In other words, a class should have a single, well-defined responsibility or concern.

**Real-life Example**: Consider a `UserManager` class that is responsible for managing user data, authenticating users, and sending notifications. This class violates the SRP because it has multiple responsibilities: user management, authentication, and notification sending. A better design would be to separate these concerns into three different classes: `UserRepository` for user data management, `AuthenticationService` for user authentication, and `NotificationService` for sending notifications.

```java
// Violates SRP
class UserManager {
    public void registerUser(UserData data) { /* ... */ }
    public boolean authenticateUser(String username, String password) { /* ... */ }
    public void sendWelcomeEmail(User user) { /* ... */ }
}

// Follows SRP
class UserRepository {
    public void registerUser(UserData data) { /* ... */ }
    // Other user data management methods
}

class AuthenticationService {
    public boolean authenticateUser(String username, String password) { /* ... */ }
    // Other authentication methods
}

class NotificationService {
    public void sendWelcomeEmail(User user) { /* ... */ }
    // Other notification methods
}
```

By adhering to the SRP, your classes become more focused, easier to understand, and less prone to unintended side effects when making changes.

## 2. Open/Closed Principle (OCP)

The Open/Closed Principle states that software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.

**Real-life Example**: Consider a `ShapeCalculator` class that calculates the area of different shapes like rectangles and circles. If we want to add support for calculating the area of a new shape, like a triangle, we would need to modify the existing `ShapeCalculator` class, violating the OCP.

A better approach would be to use an abstract base class or interface for shapes and implement specific shape classes that inherit or implement the base class/interface. This way, we can add new shapes without modifying the existing `ShapeCalculator` class.

```java
// Violates OCP
class ShapeCalculator {
    public double calculateArea(Shape shape) {
        if (shape instanceof Rectangle) {
            // Calculate area for rectangle
        } else if (shape instanceof Circle) {
            // Calculate area for circle
        }
        // If we want to add a new shape, we need to modify this class
    }
}

// Follows OCP
interface Shape {
    double calculateArea();
}

class Rectangle implements Shape {
    private double length, width;
    
    public double calculateArea() {
        return length * width;
    }
}

class Circle implements Shape {
    private double radius;
    
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

class Triangle implements Shape {
    private double base, height;
    
    public double calculateArea() {
        return 0.5 * base * height;
    }
}

class ShapeCalculator {
    public double calculateArea(Shape shape) {
        return shape.calculateArea();
    }
}
```

By following the OCP, we can extend the system's functionality without modifying the existing codebase, making it more maintainable and less prone to introducing bugs when adding new features.

## 3. Liskov Substitution Principle (LSP)

The Liskov Substitution Principle states that objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program.

If Class B is a subtype of Class A, then we should be able to replace object of A with B without breaking the behaviour of program

Subclass should extent the capablity of parent class not narrow it down.

**Real-life Example**: Consider a `Vehicle` base class and two derived classes: `Car` and `Bicycle`. If we have a method that takes a `Vehicle` object as input and performs operations based on the assumption that all vehicles have an engine, this method would violate the LSP because a `Bicycle` does not have an engine.

```java
// Violates LSP
class Vehicle {
    // ...
}

class Car extends Vehicle {
    private Engine engine;
    // ...
}

class Bicycle extends Vehicle {
    // No engine
    // ...
}

void startVehicle(Vehicle vehicle) {
    vehicle.engine.start(); // This will fail for Bicycle objects
}

// This also violated LSP
interface Bike {
    void turnOnEngine();
    void accelerate();
}

class MotorCycle implements Bike {
    boolean isEngineOn;
    int speed;

    public void turnOnEngine() {
        // turn on the engine!
        isEngineOn = true;
    }

    public void accelerate() {
        // increase the speed
        speed = speed + 10;
    }
}

class Bicycle implements Bike {
    public void turnOnEngine() {
        throw new AssertionError("There is no engine");
    }

    public void accelerate() {
        // do something
    }
}

// This implementation violates the LSP because a Bicycle object cannot be 
// substituted for a Bike object without affecting the correctness of the program. 
// If we pass a Bicycle instance to a method that expects a Bike and calls the 
// turnOnEngine() method, it will throw an AssertionError.

```



To follow the LSP, we should ensure that derived classes maintain the behavior and invariants defined by their base classes. One solution is to create an abstract base class `MotorizedVehicle` and derive `Car` from it, keeping `Bicycle` as a separate class that does not inherit from `MotorizedVehicle`.

```java
// Follows LSP
abstract class MotorizedVehicle {
    protected Engine engine;
    
    void startEngine() {
        engine.start();
    }
}

class Car extends MotorizedVehicle {
    // ...
}

class Bicycle {
    // ...
}
```

By adhering to the LSP, we can ensure that objects of derived classes can be safely substituted for objects of their base classes, leading to more robust and maintainable code.

## 4. Interface Segregation Principle (ISP)

The Interface Segregation Principle states that clients should not be forced to depend on interfaces they do not use.

**Real-life Example**: Consider an interface `MultiFunctionDevice` that defines methods for printing, scanning, and faxing. If we have a `Printer` class that only needs to implement the printing functionality, it would still need to provide empty or stub implementations for the scanning and faxing methods, even though they are not needed.

```java
// Violates ISP
interface MultiFunctionDevice {
    void print(Document doc);
    void scan(Document doc);
    void fax(Document doc);
}

class Printer implements MultiFunctionDevice {
    public void print(Document doc) {
        // Printing implementation
    }
    
    public void scan(Document doc) {
        // Printer cannot scan, so we have an empty or stub implementation
    }
    
    public void fax(Document doc) {
        // Printer cannot fax, so we have an empty or stub implementation
    }
}
```

To follow the ISP, we should segregate the interface into smaller, more specific interfaces, each representing a distinct responsibility or behavior.

```java
// Follows ISP
interface Printer {
    void print(Document doc);
}

interface Scanner {
    void scan(Document doc);
}

interface Fax {
    void fax(Document doc);
}

class MultiFunctionDevice implements Printer, Scanner, Fax {
    // Implement all three interfaces
}

class SimplePrinter implements Printer {
    public void print(Document doc) {
        // Printing implementation
    }
}
```

By adhering to the ISP, we create smaller and more cohesive interfaces, reducing the risk of bloated and unnecessarily complex implementations. Clients can depend only on the interfaces they actually need, leading to better code organization and maintainability.

## 5. Dependency Inversion Principle (DIP)
The Dependency Inversion Principle states that high-level modules should not depend on low-level modules; both should depend on abstractions. Additionally, abstractions should not depend on details, but details should depend on abstractions.

Classes should depend on Interfaces rather than concrete classes

**Real-life Example**:
Imagine you own a car and need to take it to a repair shop for servicing. In this scenario, the car (high-level module) depends on the repair shop (low-level module) to perform the necessary repairs and maintenance.

The traditional approach would be to tightly couple the car with a specific repair shop. For example, your car might have a built-in computer system that only recognizes and communicates with a specific repair shop's diagnostic tools and software. This tight coupling violates the Dependency Inversion Principle.

To follow the DIP, we need to introduce an abstraction (interface or abstract class) that decouples the car from the specific repair shop implementation.

For instance, we could define an interface called `RepairService` that specifies the methods required for servicing a car:

```java
interface RepairService {
    void performDiagnostics();
    void replaceParts();
    void runTests();
    // ...
}
```

Now, the car's computer system can depend on the `RepairService` interface instead of a specific repair shop implementation. This way, any repair shop that implements the `RepairService` interface can service your car.

```java
class Car {
    private RepairService repairService;

    public Car(RepairService repairService) {
        this.repairService = repairService;
    }

    public void getServiced() {
        repairService.performDiagnostics();
        repairService.replaceParts();
        repairService.runTests();
        // ...
    }
}
```

In this example, the high-level module (the car) depends on the abstraction (`RepairService` interface), and the low-level modules (different repair shops) depend on the abstraction by implementing the `RepairService` interface.

```java
class ShopA implements RepairService {
    // ...
}

class ShopB implements RepairService {
    // ...
}
```

By following the Dependency Inversion Principle, we've achieved a more flexible and extensible design. You can take your car to any repair shop that implements the `RepairService` interface without modifying the car's code. Additionally, new repair shops can be added to the system without affecting the existing code, as long as they implement the required interface.

This principle promotes loosely coupled, modular, and reusable code, making it easier to maintain and extend the system over time.
# Conclusion

The SOLID principles are fundamental guidelines for designing object-oriented software systems that are maintainable, extensible, and flexible. By adhering to these principles, developers can create code that is easier to understand, modify, and test, ultimately leading to more robust and scalable applications.

It's important to note that while SOLID principles are widely accepted and beneficial, they should not be treated as dogma. Developers should use their best judgment and consider the specific requirements and constraints of their projects when applying these principles. However, understanding and applying SOLID principles can significantly improve the overall quality and longevity of software systems.
```
