# Understanding IoC and DI

## What is Inversion Of Control?

It is a design and software principle in which the control of and object’s behavior is inverted or moved outside of the object.
Imagine you have an application with a `Client` class and an instance of another class called `Order` inside `Client`, as shown in the following code:

```csharp
public class Client
{
	Order myOrder = new Order();

	public List<Order> GetOrders()
	{
		return myOrder.GetAllOrders();
	}
}
```

This code don’t use IoC because:

- The `Client` class has a strong coupling with the `Order` class.
- The `Client` class is responsible for knowing how to create an instance of `Order` and depends on that instance.
- The `Client` class depends on the `Order` class and its dependencies.
- Any changes made to the `Order` class affect the `Client` class.

## How do we apply IoC in this code?

- Let's invert the control of the `Client` class and remove its dependency.
- We'll transfer the responsibility of creating an instance of `Order` to another class.
- We should create an abstraction between the classes, and the classes should depend on this abstraction (interface).

```csharp

public interface IOrder
{
	List<Order> GetAllOrders();
}

public class Order: IOrder
{
	public int Id {get; set;}
	public int ClientId {get; set;}

	public List<Order> GetAllOrders()
	{
		var orders = new List<Order>();
		orders.Add(new Order {Id = 1, ClientId = 1});
		return orders;
	}
}

public class Client
{
	private readonly IOrder _order;

	public Client(IOrder order)
	{
		_order = order
	}

	public List<Order> GetOrders()
	{
		return order.GetAllOrders();
	}
}
```

Here, we apply Inversion of Control (IoC), meaning the responsibility for creating an instance of the `Order` class lies with the `IOrder` interface. Therefore, the `Client` class now depends on an abstraction rather than an implementation, which makes the code more decoupled and enhances maintainability and testability.

## Dependency Injection

Dependency injection is a technique used in object-oriented programming OOP to reduce the hardcoded dependencies between objects. A dependency in this context refers to a piece of code that relies on another resource to carry out its intended function. Often, that resource is a different object in the same application.

Dependencies within an OOP application enable objects to perform their assigned tasks by providing additional functionality. For example, an application might include two class definitions: Class A and Class B. As part of its definition, Class B creates an instance of Class A to carry out a specific task, which means that Class B is dependent on Class A to carry out its function. The dependency is hardcoded into the Class B definition, resulting in code that is tightly coupled. Such code is more difficult to test, modify or reuse than loosely coupled code.

## **The 4 roles of dependency injection**

Dependency injection is implemented in OOP development through an application's class definitions. The components that participate in the injection typically play one of these four roles:

1. **Service.** A class that carries out some type of functionality. Any object can be either a service or client. Which one it is depends on the role the object has in a particular injection.
2. **Client.** A class that requests something from a service. A client can be any class that uses a service.
3. **Interface.** A component implemented by a service for use by one or more clients. The component enables the client to access the service's functions, while abstracting the details of about how the service implements those functions, thus breaking dependencies between lower and higher classes.
4. **Injector.** A component that introduces a service to a client. The injector creates a service instance and then inserts the service into a client. The injector can be many objects working together.

## **Types of dependency injection**

OOP supports the following approaches to dependency injection:

- **Constructor injection.** An injector uses a class constructor to inject the dependency. The referenced object is passed in as a parameter to the constructor.
- **Setter (property) injection.** The client exposes a setter method that the injector uses to pass in the dependency.
- **Method injection.** A client class is used to implement an interface. A method then provides the dependency, and an injector uses the interface to supply the dependency to the class.
- **Interface injection.** An injector method, provided by a dependency, injects the dependency into another client. Clients then need to implement an interface that uses a setter method to accept the dependency.

## **Advantages of dependency injection**

Many development teams use dependency injection because it offers several important benefits:

- Code modules do not need to instantiate references to resources, and dependencies can be easily swapped out, even mock dependencies. By enabling the framework to do the resource creation, configuration data is centralized, and updates occur only in one place.
- Injected resources can be customized through. Extensible Markup Language files outside the source code. This enables changes to be applied without having to recompile the entire codebase.
- Programs are more testable, maintainable and reusable because the client classes do not need to know how dependencies are implemented.
- Developers working on the same application can build classes independently of each other because they only need to know how to use the interfaces to the referenced classes, not the workings of the classes themselves.
- Dependency injection helps in unit testing because configuration details can be saved to configuration files. This also enables the system to be reconfigured without recompiling.

## **Disadvantages of dependency injection**

Although dependency injection can be beneficial, it also comes with several challenges:

- Dependency injection makes troubleshooting difficult because much of the code is pushed into an unknown location that creates resources and distributes them as needed across the application.
- Debugging code when misbehaving objects are buried in a complicated third-party framework can be frustrating and time-consuming.
- Dependency injection can slow integrated development environment automation, as dependency injection frameworks use either reflection or dynamic programming.
