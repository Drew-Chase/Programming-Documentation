---
description: A singleton for your application configuration.
---

# Application Config

The `AppConfigBase` class is a base class for configuration files in the Chase CommonLib library, designed for use in .NET 6.0 and later. It provides a foundation for managing configuration settings by extending the `ConfigurationFile<T>` class, where `T` is the derived configuration class. This base class implements a singleton pattern to ensure that only one instance of the configuration is created and managed during the application's lifetime.

### Properties

#### Instance

```csharp
[JsonIgnore]
public static T Instance { get; }
```

* **Description**: Gets the singleton instance of the configuration file.
* **Usage**: This property allows you to access the configuration settings throughout your application using the singleton pattern. It ensures that there is only one instance of the configuration, promoting consistency in configuration values.

#### Path

```csharp
public string? Path { get; protected set; }
```

* **Description**: Gets or sets the path to the configuration file on the disk.
* **Usage**: This property is used to specify the location of the configuration file. It is set during the initialization process.

### Methods

#### Initialize

```csharp
public virtual void Initialize(string path)
```

* **Description**: Initializes and loads the configuration file from the specified path.
* **Parameters**:
  * `path` (string): The path to the configuration file.
* **Usage**: Call this method to initialize the configuration file. It sets the `Path` property and loads the configuration file from the provided path. This method should be called once at the beginning of your application.

#### Load

```csharp
public override T? Load()
```

* **Description**: Loads the configuration file from disk.
* **Returns**: The loaded instance of the derived configuration class.
* **Exceptions**:
  * `IOException`: Thrown if the configuration file path is not set.
* **Usage**: This method loads the configuration settings from the specified file into the derived configuration class. It also handles copying properties from the loaded instance to the singleton instance to ensure that the singleton instance reflects the latest configuration values.

### Example Code

Here is an example of how to use the `AppConfigBase` class:

```csharp
using Chase.CommonLib.FileSystem.Configuration;

// Define your derived configuration class
public class MyAppConfig : AppConfigBase<MyAppConfig>
{
    // Define your configuration properties here
    public string ApiKey { get; set; }
    public int MaxConnections { get; set; }
}

// In your application code:
public class Program
{
    public static void Main()
    {
        // Initialize the configuration
        MyAppConfig.Instance.Initialize("config.json");

        // Access configuration values
        string apiKey = MyAppConfig.Instance.ApiKey;
        int maxConnections = MyAppConfig.Instance.MaxConnections;

        // Modify configuration values (if needed)
        MyAppConfig.Instance.MaxConnections = 10;

        // Save the updated configuration (if needed)
        MyAppConfig.Instance.Save();
    }
}
```

In the above example, we first define a derived configuration class `MyAppConfig` based on `AppConfigBase`. We define configuration properties within this class.

In the `Main` method, we initialize the configuration using `Initialize`, access configuration values, and optionally modify and save them. The `Instance` property provides access to the singleton instance of the configuration.

By using this class, you can easily manage your application's configuration settings while ensuring consistency and singleton behavior.

#### Ignoring Properties

To ignore specific properties during serialization, you can use the `[JsonIgnore]` attribute. For example, if you want to ignore a property named `IgnoreMe`, you can do the following:

```csharp
public class MyAppConfig : AppConfigBase<MyAppConfig>
{
    [JsonIgnore]
    public string IgnoreMe { get; set; }
}
```

#### Changing Property Names in JSON Output

To change the property name in the JSON output, you can use the `[JsonProperty]` attribute. For example, if you want to change the property name `ApiKey` to `api_key` in the JSON output, you can do the following:

```csharp
public class MyAppConfig : AppConfigBase<MyAppConfig>
{
    [JsonProperty("api_key")]
    public string ApiKey { get; set; }
}
```

This allows you to control how your configuration properties are represented in the serialized JSON configuration file.
