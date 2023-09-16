# Database File

### Overview

The `DatabaseFile` class is a part of the Chase CommonLib library, designed for use with .NET 6.0 and above. It provides functionality to work with compressed database files containing a collection of entries. This class allows you to efficiently store and retrieve large amounts of data using a key-value approach. Each entry is identified by a unique `Guid` key and can store any serializable object.

### Licensing

This class is licensed under the [GNU General Public License (GPL-3.0)](https://www.gnu.org/licenses/gpl-3.0.en.html#license-text).

### Namespace

```csharp
using Chase.CommonLib.FileSystem.Configuration;
```

### Constructors

#### `DatabaseFile(string filePath)`

* **Description**: Creates a new instance of the `DatabaseFile` class and opens or creates a database file at the specified `filePath`.
* **Parameters**:
  * `filePath` (string): The path to the database file.
* **Example**:

```csharp
string dbFilePath = "myDatabase.db";
DatabaseFile database = new DatabaseFile(dbFilePath);
```

#### `static DatabaseFile Open(string filePath)`

* **Description**: Creates or opens an instance of the `DatabaseFile` class from an existing database file at the specified `filePath`.
* **Parameters**:
  * `filePath` (string): The path to the database file.
* **Returns**: An instance of the `DatabaseFile` class.
* **Example**:

```csharp
string dbFilePath = "myDatabase.db";
DatabaseFile database = DatabaseFile.Open(dbFilePath);
```

### Methods

#### `void WriteEntry(Guid key, object value)`

* **Description**: Writes an entry to the database file with the specified `Guid` key and associated object value. If an entry with the same key already exists, it will be overwritten.
* **Parameters**:
  * `key` (Guid): The unique identifier for the entry.
  * `value` (object): The value to be stored in the entry. This value must be serializable.
* **Example**:

```csharp
Guid entryKey = Guid.NewGuid();
string entryValue = "This is my data.";
database.WriteEntry(entryKey, entryValue);
```

#### `T? ReadEntry<T>(Guid key)`

* **Description**: Reads an entry from the database file with the specified `Guid` key and deserializes it into the specified type `T`.
* **Parameters**:
  * `key` (Guid): The unique identifier for the entry.
* **Returns**: The deserialized value of the entry, or `default(T)` if the entry does not exist.
* **Example**:

```csharp
Guid entryKey = Guid.Parse("c7f106bfa0e84c66b9f6d2c4f9e3c3b7");
string entryValue = database.ReadEntry<string>(entryKey);
```

#### `bool Exists(Guid key)`

* **Description**: Checks if an entry with the specified `Guid` key exists in the database file.
* **Parameters**:
  * `key` (Guid): The unique identifier for the entry.
* **Returns**: `true` if the entry exists; otherwise, `false`.
* **Example**:

```csharp
Guid entryKey = Guid.Parse("c7f106bfa0e84c66b9f6d2c4f9e3c3b7");
bool entryExists = database.Exists(entryKey);
```

#### `void Dispose()`

* **Description**: Releases all resources used by the `DatabaseFile` object and writes any pending changes to the file. This method should be called when you are done using the `DatabaseFile`.
* **Example**:

```csharp
database.Dispose();
```

### Example Usage

Here is an example of how to use the `DatabaseFile` class:

```csharp
string dbFilePath = "myDatabase.db";
DatabaseFile database = new DatabaseFile(dbFilePath);

Guid entryKey = Guid.NewGuid();
string entryValue = "This is my data.";

// Write an entry
database.WriteEntry(entryKey, entryValue);

// Check if an entry exists
bool entryExists = database.Exists(entryKey);

if (entryExists)
{
    // Read the entry
    string retrievedValue = database.ReadEntry<string>(entryKey);
    Console.WriteLine($"Retrieved Value: {retrievedValue}");
}

// Dispose of the DatabaseFile to release resources
database.Dispose();
```

This example demonstrates creating a `DatabaseFile`, writing an entry, checking its existence, reading it, and finally disposing of the `DatabaseFile` object when done.

Please ensure that you have appropriate error handling and data validation in your actual implementation.
