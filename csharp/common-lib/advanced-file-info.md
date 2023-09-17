# Advanced File Info

The `AdvancedFileInfo` class is part of the Chase CommonLib library, designed to provide additional information about a file, including its size representation in various units. This class extends the functionality of the standard `System.IO.FileInfo` class by adding methods to convert file sizes to human-readable strings in different units.

### Namespace

```csharp
namespace Chase.CommonLib.Math;
```

### Constructors

#### `AdvancedFileInfo(string file)`

Creates a new instance of the `AdvancedFileInfo` class with the specified file path.

* **Parameters:**
  * `file` (string): The path of the file.

### Properties

#### `BaseInfo` (FileInfo)

The `BaseInfo` property holds the base `System.IO.FileInfo` object for the specified file path.

#### `IsDirectory` (bool)

The `IsDirectory` property indicates whether the specified path represents a directory (`true`) or not (`false`).

### Methods

#### `SizeToString(long bytes, int places = 2, FileSizeUnit unit = FileSizeUnit.Bytes)`

Converts a file size in bytes to a human-readable string representation with the specified number of decimal places and unit.

* **Parameters:**
  * `bytes` (long): The size of the file in bytes.
  * `places` (int, optional): The number of decimal places to round the size. Default is 2.
  * `unit` (FileSizeUnit, optional): The unit of file size to use for representation (Bytes, Bits, or IbiBytes). Default is Bytes.
* **Returns:** (string) A human-readable string representation of the file size.
* **Exceptions:**
  * `ArgumentException`: Thrown if an invalid `unit` value is provided.

#### `SizeToString(int places = 2, FileSizeUnit unit = FileSizeUnit.Bytes)`

Converts the size of the file represented by this `AdvancedFileInfo` object to a human-readable string representation with the specified number of decimal places and unit.

* **Parameters:**
  * `places` (int, optional): The number of decimal places to round the size. Default is 2.
  * `unit` (FileSizeUnit, optional): The unit of file size to use for representation (Bytes, Bits, or IbiBytes). Default is Bytes.
* **Returns:** (string) A human-readable string representation of the file size.

### Enum

#### `FileSizeUnit`

An enum representing different file size units:

* `Bytes`: Represents file size in bytes (e.g., 1.2 MB).
* `Bits`: Represents file size in bits (e.g., 1.2 Mb).
* `IbiBytes`: Represents file size in binary bytes (e.g., 1.2 MiB).

### Example Usage

```csharp
using Chase.CommonLib.Math;

class Program
{
    static void Main()
    {
        // Create an AdvancedFileInfo object for a file
        AdvancedFileInfo fileInfo = new AdvancedFileInfo("example.txt");

        // Check if the path is a directory
        if (fileInfo.IsDirectory)
        {
            Console.WriteLine("The path is a directory.");
        }
        else
        {
            Console.WriteLine("The path is not a directory.");
        }

        // Get and print the file size in different units
        Console.WriteLine("File Size (Bytes): " + fileInfo.SizeToString(unit: FileSizeUnit.Bytes));
        Console.WriteLine("File Size (Bits): " + fileInfo.SizeToString(unit: FileSizeUnit.Bits));
        Console.WriteLine("File Size (IbiBytes): " + fileInfo.SizeToString(unit: FileSizeUnit.IbiBytes));
    }
}
```

This example demonstrates how to use the `AdvancedFileInfo` class to check if a path is a directory and convert the file size to different units for a specified file.
