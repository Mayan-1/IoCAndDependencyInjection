# Understanding IoC and DI

## What is Inversion Of Control?

It is a design and software principle in which the control of and object’s behavior is inverted or moved outside of the object.

Imagine that you have an console application that asks for the name and age of your client and then show a message, you would probably write code like this:

```csharp
string name = Console.ReadLine();
string age = Console.ReadLine();

Console.WriteLine($"Welcome, {name}! Enjoy our system.");
```

This code don’t use IoC because:

- **Direct control over the flow:** The logic to ask for and display information is plugged directly into the main function of the code.
- **Tight dependencies:** Here, `Console.WriteLine` and `Console.ReadLine` are used directly, which creates a dependency on the console to ask for and display information.

## How do we apply IoC in this code?

1.  We can extract that logic into an interface to ask for the name and age of your client, like `IUserInput`, and another interface to display a message, like `IUserOutput`. Let’s see.
2.  Create implementations of these interfaces and inject them into a constructor of a `ConsoleApplication` class that handles the main logic.

```csharp

public interface IUserInput
{
	string GetNome();
	string GetAge();
}

public interface IUserOutput
{
	void ShowMessage(string name);
}

public class UserInput : IUserInput
{
	 public string GetName()
    {
        Console.WriteLine("Enter your name:");
        return Console.ReadLine();
    }

    public string GetAge()
    {
        Console.WriteLine("Enter your age:");
        return Console.ReadLine();
    }
}

public class UserOutput : IUserOutput
{
    public void ShowMessage(string name)
    {
        Console.WriteLine($"Welcome, {name}! Enjoy using our system.");
    }
}

public class ConsoleApplication
{
		private readonly IUserInput _userInput;
    private readonly IUserOutput _userOutput;

    public ConsoleApplication(IUserInput userInput, IUserOutput userOutput)
    {
        _userInput = userInput;
        _userOutput = userOutput;
    }

    public void Run()
    {
        string name = _userInput.GetName();
        string age = _userInput.GetAge();

        _userOutput.ShowMessage(name);
    }
}
```

In the code above, we have the design pattern DI (Dependency Injection). Our `ConsoleApplication` doesn't have to know the logic of `UserInput` or `UserOutput` it only needs to know their abstractions, `IUserInput` and `IUserOutput`. This way, we make our code more flexible and maintainable. Then we can make other changes to `ShowMessage`, for example, and the `ConsoleApplication` wouldn't be affected.

Probably at this point, you have a question: what exactly is Dependency Injection?

## DI (Dependency Injection)

Dependency Injection is a design pattern aimed at decoupling the classes within a system. In other words, it is a way to structure code so that classes depend on abstractions instead of directly relying on other concrete classes. We use Dependency Injection as one of the ways to achieve Inversion of Control.

As we saw in the code above, instead of `ConsoleApplication` knowing the business logic or having a `UserInput` object, we use an abstraction (interface). And with that interface, ConsoleApplication can call all the methods implemented by UserInput or UserOutput.
