# Design Patterns: Real-Life Examples and Pseudocode

Design patterns are reusable solutions to common software design problems. They provide a proven approach to structuring code, promoting code reuse, maintainability, and extensibility. In this Markdown file, we'll explore the top five design patterns with real-life examples and pseudocode for better understanding.

## 1. Strategy Pattern

The Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. It allows the algorithm to vary independently from clients that use it. This pattern is useful when you have multiple algorithms that perform a similar task, and you want to make them interchangeable based on certain conditions or configurations.

**Real-life Example**: Consider a payment processing system that supports multiple payment methods, such as credit card, PayPal, and bank transfer. Instead of having a single payment method with conditional statements for each payment type, you can encapsulate each payment method in a separate strategy class.

```
// PaymentStrategy interface
interface PaymentStrategy {
    function processPayment(amount: number): boolean
}

// Concrete strategies for different payment methods
class CreditCardStrategy implements PaymentStrategy {
    function processPayment(amount: number): boolean {
        // Implement credit card payment logic
        return true
    }
}

class PayPalStrategy implements PaymentStrategy {
    function processPayment(amount: number): boolean {
        // Implement PayPal payment logic
        return true
    }
}

class BankTransferStrategy implements PaymentStrategy {
    function processPayment(amount: number): boolean {
        // Implement bank transfer payment logic
        return true
    }
}

// Context class (e.g., PaymentProcessor)
class PaymentProcessor {
    private strategy: PaymentStrategy

    function setPaymentStrategy(strategy: PaymentStrategy): void {
        this.strategy = strategy
    }

    function processPayment(amount: number): boolean {
        return this.strategy.processPayment(amount)
    }
}

// Usage
let paymentProcessor = new PaymentProcessor()
paymentProcessor.setPaymentStrategy(new CreditCardStrategy())
paymentProcessor.processPayment(100) // Uses credit card payment

paymentProcessor.setPaymentStrategy(new PayPalStrategy())
paymentProcessor.processPayment(200) // Uses PayPal payment

```

## 2. Decorator Pattern

The Decorator Pattern attaches additional responsibilities to an object dynamically, providing a flexible alternative to subclassing for extending functionality. It involves creating decorator objects that wrap the original object and provide additional features.

**Real-life Example**: Consider a scenario where you have a pizza ordering system, and you want to allow customers to add toppings to their pizzas. Instead of creating a separate class for every possible combination of toppings, you can use the Decorator Pattern to dynamically add toppings to a base pizza.

```
// Component interface
interface Pizza {
    function getDescription(): string
    function getCost(): number
}

// Concrete component
class PlainPizza implements Pizza {
    function getDescription(): string {
        return "Plain Pizza"
    }

    function getCost(): number {
        return 5
    }
}

// Decorator interface
interface ToppingDecorator extends Pizza {
    function getPizza(): Pizza
}

// Concrete decorators
class CheeseTopping implements ToppingDecorator {
    private pizza: Pizza

    constructor(pizza: Pizza) {
        this.pizza = pizza
    }

    function getDescription(): string {
        return this.pizza.getDescription() + ", Cheese"
    }

    function getCost(): number {
        return this.pizza.getCost() + 1
    }

    function getPizza(): Pizza {
        return this.pizza
    }
}

class PepperoniTopping implements ToppingDecorator {
    private pizza: Pizza

    constructor(pizza: Pizza) {
        this.pizza = pizza
    }

    function getDescription(): string {
        return this.pizza.getDescription() + ", Pepperoni"
    }

    function getCost(): number {
        return this.pizza.getCost() + 1.5
    }

    function getPizza(): Pizza {
        return this.pizza
    }
}

// Usage
let plainPizza = new PlainPizza()
console.log(plainPizza.getDescription()) // Output: Plain Pizza
console.log(plainPizza.getCost()) // Output: 5

let cheesePizza = new CheeseTopping(plainPizza)
console.log(cheesePizza.getDescription()) // Output: Plain Pizza, Cheese
console.log(cheesePizza.getCost()) // Output: 6

let pepperoniPizza = new PepperoniTopping(cheesePizza)
console.log(pepperoniPizza.getDescription()) // Output: Plain Pizza, Cheese, Pepperoni
console.log(pepperoniPizza.getCost()) // Output: 7.5

```

## 3. Observer Pattern

The Observer Pattern defines a one-to-many dependency between objects, so that when one object changes state, all its dependents are notified and updated automatically. This pattern is commonly used in implementing distributed event handling systems, where objects subscribe to events and receive notifications when those events occur.

**Real-life Example**: Consider a news subscription system where users can subscribe to receive news updates from various sources. When a news article is published, all subscribed users should be notified.

```
// Subject interface
interface NewsPublisher {
    function subscribe(observer: NewsSubscriber): void
    function unsubscribe(observer: NewsSubscriber): void
    function notifySubscribers(article: NewsArticle): void
}

// Concrete subject
class NewsAgency implements NewsPublisher {
    private subscribers: NewsSubscriber[] = []

    function subscribe(observer: NewsSubscriber): void {
        this.subscribers.push(observer)
    }

    function unsubscribe(observer: NewsSubscriber): void {
        const index = this.subscribers.indexOf(observer)
        if (index !== -1) {
            this.subscribers.splice(index, 1)
        }
    }

    function notifySubscribers(article: NewsArticle): void {
        for (const subscriber of this.subscribers) {
            subscriber.update(article)
        }
    }

    function publishArticle(article: NewsArticle): void {
        this.notifySubscribers(article)
    }
}

// Observer interface
interface NewsSubscriber {
    function update(article: NewsArticle): void
}

// Concrete observers
class EmailSubscriber implements NewsSubscriber {
    private email: string

    constructor(email: string) {
        this.email = email
    }

    function update(article: NewsArticle): void {
        console.log(`Sending article "${article.title}" to ${this.email}`)
    }
}

class MobileAppSubscriber implements NewsSubscriber {
    private appId: string

    constructor(appId: string) {
        this.appId = appId
    }

    function update(article: NewsArticle): void {
        console.log(`Pushing notification for article "${article.title}" to app ${this.appId}`)
    }
}

// Usage
let newsAgency = new NewsAgency()

let emailSubscriber = new EmailSubscriber("user@example.com")
let mobileAppSubscriber = new MobileAppSubscriber("com.example.myapp")

newsAgency.subscribe(emailSubscriber)
newsAgency.subscribe(mobileAppSubscriber)

let article = { title: "Breaking News!", content: "..." }
newsAgency.publishArticle(article)

```

## 4. Singleton Pattern

The Singleton Pattern ensures that a class has only one instance and provides a global point of access to it. This pattern is useful when you need to have a single instance of a class that manages a resource or when you need to enforce a strict control over the creation and access of an object.

**Real-life Example**: Consider a scenario where you have a configuration manager class that reads configuration settings from a file or a database. You want to ensure that only one instance of this class exists throughout the application to avoid redundant resource usage and potential conflicts.

```
class ConfigurationManager {
    private static instance: ConfigurationManager
    private config: any

    private constructor() {
        // Read configuration from file or database
        this.config = { /* configuration data */ }
    }

    public static getInstance(): ConfigurationManager {
        if (!ConfigurationManager.instance) {
            ConfigurationManager.instance = new ConfigurationManager()
        }
        return ConfigurationManager.instance
    }

    public getConfigValue(key: string): any {
        return this.config[key]
    }
}

// Usage
const configManager1 = ConfigurationManager.getInstance()
const configValue1 = configManager1.getConfigValue("appName")

const configManager2 = ConfigurationManager.getInstance()
const configValue2 = configManager2.getConfigValue("databaseUrl")

// configManager1 and configManager2 are the same instance

```

## 5. Facade Pattern

The Facade Pattern provides a simplified interface to a complex system of classes, a library, or a framework, making it easier to understand and use. It acts as a high-level abstraction that hides the complexity of the underlying system and exposes only the necessary functionality.

**Real-life Example**: Consider a complex video conversion system that involves various classes and operations for reading video files, converting them to different formats, applying filters, and writing the output. Instead of exposing this complexity to the client code, you can create a Facade class that provides a simple interface for video conversion.

```
// Complex subsystem classes
class VideoReader {
    // ...
}

class VideoConverter {
    // ...
}

class VideoFilter {
    // ...
}

class VideoWriter {
    // ...
}

// Facade
class VideoConversionFacade {
    private reader: VideoReader
    private converter: VideoConverter
    private filter: VideoFilter
    private writer: VideoWriter

    constructor() {
        this.reader = new VideoReader()
        this.converter = new VideoConverter()
        this.filter = new VideoFilter()
        this.writer = new VideoWriter()
    }

    function convertVideo(inputFile: string, outputFile: string, format: string): void {
        const videoData = this.reader.readVideo(inputFile)
        const convertedData = this.converter.convert(videoData, format)
        const filteredData = this.filter.applyFilters(convertedData)
        this.writer.writeVideo(filteredData, outputFile)
    }
}

// Usage
const conversionFacade = new VideoConversionFacade()
conversionFacade.convertVideo("input.mp4", "output.avi", "avi")

```

In the above examples, we've used pseudocode to illustrate the concepts and structures of the design patterns. In practice, you would implement these patterns using the specific programming language and frameworks of your choice, following the language's syntax and conventions.

# Conclusion

Design patterns are powerful tools that help developers write maintainable, extensible, and flexible code. By understanding and applying these patterns, you can create more robust and efficient software systems. The examples provided in this Markdown file should give you a solid foundation for understanding and implementing the top five design patterns: Strategy, Decorator, Observer, Singleton, and Facade.

Remember, design patterns are not one-size-fits-all solutions; they should be applied judiciously based on the specific requirements and constraints of your project. Mastering these patterns takes time and practice, but the effort is well worth it in the long run, as it will help you write cleaner, more maintainable, and more scalable code.
