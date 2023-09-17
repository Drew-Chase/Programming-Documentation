---
description: To handle multiple configuration files.
---

# Configuration File

The `ConfigurationFile<T>` class is a part of the Chase CommonLib library, developed by LFInteractive LLC. It serves as a template for creating configuration files in JSON format for .NET 6.0+ applications. This class is designed to be used as a base class for specific configuration file implementations, allowing developers to easily create and manage configuration files with common functionality. The class includes features for saving and loading configuration data from disk, as well as events that notify when the configuration file is saved or loaded.

### Class Overview

```csharp
namespace Chase.CommonLib.FileSystem.Configuration
{
    public class ConfigurationFile<T> where T : ConfigurationFile<T>, new()
    {
        // Class members and methods
    }
}
```

#### Class Constraints

The class is generic and takes a type parameter `T`, which must derive from `ConfigurationFile<T>` and have a parameterless constructor. This constraint ensures that any derived class adheres to the common functionality defined by this base class.

### Properties

#### `Path` Property

```csharp
[JsonIgnore]
public string Path { get; set; } = "";
```

* This property represents the path to the configuration file on disk.
* The `[JsonIgnore]` attribute is used to indicate that this property should be excluded when serializing the object to JSON. This allows developers to store additional information in the configuration file class without it being persisted to the file.

### Events

#### `ConfigurationSaved` Event

```csharp
public event ConfigurationEventHandler? ConfigurationSaved;
```

* This event is triggered when the configuration file is saved using the `Save()` method.

#### `ConfigurationLoaded` Event

```csharp
public event ConfigurationEventHandler? ConfigurationLoaded;
```

* This event is triggered when the configuration file is loaded using the `Load()` method.

### Methods

#### `Save()` Method

```csharp
public virtual void Save()
```

* This method saves the current configuration object to the specified file path.
* It checks if the `Path` property is set and raises an exception if it is not.
* The configuration object is serialized to JSON format with indented formatting and saved to the specified file path.
* After saving, the `ConfigurationSaved` event is invoked to notify listeners.

#### `Load()` Method

```csharp
public virtual T? Load()
```

* This method loads a configuration object from the specified file path.
* It checks if the `Path` property is set and raises an exception if it is not.
* If the file does not exist, it creates a new configuration file by calling the `Save()` method.
* If the file exists, it reads the JSON content, deserializes it into the specified type `T`, and sets the `Path` property of the loaded object.
* After loading, the `ConfigurationLoaded` event is invoked to notify listeners.
* The method returns the loaded configuration object or `null` if loading fails.

### Example Usage

Here's an example of how to use the `ConfigurationFile<T>` class and how to apply the `[JsonIgnore]` and `[JsonProperty]` attributes:

```csharp
using Chase.CommonLib.FileSystem.Configuration;
using Newtonsoft.Json;

public class MyAppConfiguration : ConfigurationFile<MyAppConfiguration>
{
    // Add your application-specific configuration properties here
    public string AppName { get; set; } = "MyApp";
    
    [JsonIgnore] // Exclude this property from serialization
    public string SecretKey { get; set; } = "supersecret";
    
    [JsonProperty("LoggingLevel")] // Rename the property in the output JSON
    public string LogLevel { get; set; } = "Info";
}

// Usage
MyAppConfiguration config = new MyAppConfiguration();
config.Path = "appconfig.json";

// Save the configuration to disk
config.Save();

// Load the configuration from disk
MyAppConfiguration loadedConfig = config.Load();

// Access configuration properties
string appName = loadedConfig.AppName;
string logLevel = loadedConfig.LogLevel;
```

In this example, we create a derived configuration class `MyAppConfiguration` with additional properties. We use `[JsonIgnore]` to exclude the `SecretKey` property from serialization and `[JsonProperty]` to rename the `LogLevel` property in the output JSON.
