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
