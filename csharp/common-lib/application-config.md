---
description: A singleton for your application configuration.
---

# Application Config

The `AppConfigBase` class is a base class for configuration files in the Chase CommonLib library. It provides a framework for managing and handling configuration data. This class is designed to be extended by specific configuration classes that define their own configuration properties and behavior.

### Namespace

```csharp
namespace Chase.CommonLib.FileSystem.Configuration
```

### Inheritance

* `AppConfigBase` is the base class for all configuration classes in the library.

### Constructor

#### `public AppConfigBase()`

* Initializes a new instance of the `AppConfigBase` class. This constructor is protected to enforce the Singleton pattern.

### Properties

#### `public static AppConfigBase Instance { get; protected set; }`

* Gets the singleton instance of the configuration file.

#### `public string Path { get; set; }`

* Gets or sets the configuration file path.

### Events

#### `public event ConfigurationEventHandler? ConfigurationSaved`

* Event that is fired when the configuration file is saved. Subscribers can handle this event to perform actions after the configuration is saved.

#### `public event ConfigurationEventHandler? ConfigurationLoaded`

* Event that is fired when the configuration file is loaded. Subscribers can handle this event to perform actions after the configuration is loaded.

### Methods

#### `public virtual void Initialize(string path)`

* Initializes and loads the configuration file.
  * `path`: A string representing the file path of the configuration file to load.

#### `public virtual void Save()`

* Saves the configuration file to disk.
  * Throws `IOException` if the configuration file path is not set.

#### `public virtual void Load()`

* Loads the configuration file from disk.
  * Throws `IOException` if the configuration file path is not set.

### Example Usage

```csharp
using Chase.CommonLib.FileSystem.Configuration;
using System;

namespace YourNamespace
{
    // Define a custom configuration class that extends AppConfigBase
    public class CustomConfig : AppConfigBase
    {
        public string SomeSetting { get; set; }

        [JsonProperty("another_setting")]
        public int AnotherSetting { get; set; }

        [JsonIgnore]
        public int IgnoredSetting { get; set; }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // Create an instance of your custom configuration class
            var customConfig = new CustomConfig();

            // Set the path to the configuration file
            customConfig.Initialize("config.json");

            // Modify configuration settings
            customConfig.SomeSetting = "Hello, World!";
            customConfig.AnotherSetting = 42;

            // Save the configuration to disk
            customConfig.Save();

            // Load the configuration from disk
            customConfig.Load();

            // Access configuration settings
            Console.WriteLine($"SomeSetting: {customConfig.SomeSetting}");
            Console.WriteLine($"AnotherSetting: {customConfig.AnotherSetting}");
        }
    }
}
```

In this example, we create a custom configuration class `CustomConfig` that extends `AppConfigBase`. We initialize, modify, save, and load configuration settings using this custom class.
