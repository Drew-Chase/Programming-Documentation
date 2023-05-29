# Methods

In programming, a method is a block of code that performs a specific task or operation. It allows you to organize your code into reusable chunks, making your program more modular and easier to understand.

Let's consider a real-world analogy to understand methods better. Imagine you have a recipe for making a sandwich. The recipe itself consists of a series of steps or instructions. Each step performs a specific action, such as spreading butter or adding a slice of cheese. These individual steps are similar to methods in programming.

Here's a simple example of a method in C#:

```csharp
public class Calculator
{
    // Method to add two numbers
    public int Add(int num1, int num2)
    {
        int sum = num1 + num2;
        return sum;
    }
}
```

In this example, we have a class called `Calculator`, and inside it, we have a method called `Add`. This method takes two integer parameters (`num1` and `num2`) and returns an integer value.

To use this method, we create an instance of the `Calculator` class and call the `Add` method, passing in the numbers we want to add:

```csharp
Calculator calculator = new Calculator();
int result = calculator.Add(5, 3);
Console.WriteLine("The sum is: " + result); // Output: The sum is: 8
```

In this case, we create an instance of the `Calculator` class called `calculator`. We then call the `Add` method on that instance, passing in the numbers `5` and `3`. The method performs the addition operation and returns the sum, which is stored in the `result` variable. Finally, we print the result to the console.

Methods can have different purposes and can return different types of values or perform actions without returning anything. They can also take different types and numbers of parameters based on the specific task they are designed to perform.

Remember, methods are like individual steps in a recipe that perform specific actions. They help break down complex problems into smaller, manageable pieces of code, making your programs more organized and easier to work with.
