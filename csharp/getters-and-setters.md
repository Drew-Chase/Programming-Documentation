# Getters and Setters

In C#, getters and setters are methods used in properties to control the access to private data members (fields) of a class. Getters are used to retrieve the value of a property, while setters are used to assign a new value to the property. Getters and setters provide a way to encapsulate the access to data and enable additional logic or validation to be executed.

Here's an explanation of getters and setters with code examples:

Getters:

* A getter is a method that retrieves the value of a property.
* It is defined using the `get` keyword in a property declaration.
* Getters do not have parameters and return the value of the property.

Example:

```csharp
public class Person
{
    private string name; // Private field

    public string Name // Property
    {
        get { return name; } // Getter method
    }
}

Person person = new Person();
string personName = person.Name; // Accessing the property using the getter
```

In the above example, the `Name` property has a getter that returns the value of the private `name` field. The getter allows external code to access the value of the property without directly accessing the underlying field.

Setters:

* A setter is a method used to assign a new value to a property.
* It is defined using the `set` keyword in a property declaration.
* Setters have a single parameter representing the new value to be assigned to the property.

Example:

```csharp
public class Person
{
    private string name; // Private field

    public string Name // Property
    {
        get { return name; } // Getter method
        set { name = value; } // Setter method
    }
}

Person person = new Person();
person.Name = "John"; // Setting the property value using the setter
```

In the above example, the `Name` property has both a getter and a setter. The setter allows external code to assign a new value to the property, which internally updates the private `name` field.

Getters and setters allow controlled access to properties and enable additional logic or validation to be executed during property access or assignment. They provide a way to encapsulate data and maintain the integrity and consistency of the underlying values.
