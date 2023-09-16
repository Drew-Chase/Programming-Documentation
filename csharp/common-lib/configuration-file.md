---
description: To handle multiple configuration files.
---

# Configuration File

### Overview

The `ConfigurationFile<T>` class is a part of the Chase CommonLib library, provided by LFInteractive LLC. This class is designed for managing configuration files in .NET 6.0+ applications. It allows you to easily read and write configuration data stored in JSON format to a file.

### Class Declaration

```csharp
namespace Chase.CommonLib.FileSystem.Configuration;

public class ConfigurationFile<T> : IDisposable
```

#### Type Parameters

* `T`: The type representing the content of the configuration file.

### Properties

#### `Content`

* Type: `T?`
* Description: Gets or sets the content of the configuration file.

#### `KeepOpen`

* Type: `bool`
* Description: Gets a value indicating whether the configuration file should be kept open for efficient read and write operations.

#### `FilePath`

* Type: `string`
* Description: Gets the path to the configuration file.

### Constructors

#### `ConfigurationFile(string filePath, bool keepOpen = false)`

* Parameters:
  * `filePath`: The path to the configuration file.
  * `keepOpen`: A boolean indicating whether to keep the file open for efficient read and write operations. Default is `false`.
* Description: Initializes a new instance of the `ConfigurationFile<T>` class. It creates or opens the specified configuration file based on the provided file path and optional keepOpen parameter.

### Methods

#### `Save()`

* Returns: `bool`
* Description: Saves the contents of the `Content` property to the configuration file. If `KeepOpen` is set to `true`, it will use an open file stream for writing; otherwise, it will write directly to the file.

#### `Load()`

* Returns: `T?`
* Description: Loads the contents of the configuration file into the `Content` property. If the file fails to load or doesn't exist, it returns the original content.

#### `Dispose()`

* Description: Releases all resources used by the `ConfigurationFile<T>` object. This method saves the configuration file (if necessary) and disposes of the underlying file stream.

### Example Usage

```csharp
using Chase.CommonLib.FileSystem.Configuration;

// Define a class to represent your configuration data
public class AppConfig
{
    public string ApiKey { get; set; }
    public int MaxRetryAttempts { get; set; }
}

// Create a ConfigurationFile instance
var configFilePath = "app-config.json";
var config = new ConfigurationFile<AppConfig>(configFilePath);

// Load the configuration from the file (or use default values if the file doesn't exist)
var appConfig = config.Load();

// Modify the configuration
appConfig.ApiKey = "your-api-key";
appConfig.MaxRetryAttempts = 3;

// Save the modified configuration back to the file
config.Save();

// Dispose of the ConfigurationFile when done
config.Dispose();
```

In this example, we create a `ConfigurationFile` instance for managing an `AppConfig` object. We load the configuration from the file, make changes, and then save it back to the file. Finally, we dispose of the `ConfigurationFile` object to release resources and ensure the changes are persisted.
