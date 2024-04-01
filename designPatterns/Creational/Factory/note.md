
## Factory Design Pattern

The Factory Design Pattern is used in software development to abstract and centralize the creation of objects, solving the problem of dynamic object instantiation.

This pattern is ideal for scenarios where different objects must be created based on runtime conditions

Suppose we want to create instances of different vehicle types (Car, Bike, and Truck) based on a given string, such as "Car," "Bike," or "Truck."

Without the Factory Design Pattern, you might have to create these objects directly in the client code using constructors. This approach would also require modifying client code whenever new vehicle types are introduced and we would not be able to reuse and encapsulate creation logic

We can move this modification to the factory instead (it will still violate OCP here but still better than doing this on the client side )

The Factory Design Pattern provides an elegant solution to this problem. The VehicleFactory class acts as a factory for creating instances of Vehicle subclasses. By calling the createVehicle method with a string representing the desired vehicle type, such as "Car," "Bike," or "Truck," the factory returns a concrete instance of the appropriate vehicle class.

This abstraction allows for flexible and scalable object creation without exposing the client code to the intricacies of constructing objects.

If new vehicle types are added in the future, you can extend the factory without modifying the existing client code.

This pattern promotes code reusability, maintainability, and separation of concerns, making it a valuable tool for managing object creation in software development.