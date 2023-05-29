# File Controller

## Create Class

Create a static class called `FileController` in the `Controller` namespace

See: [classes.md](../../../undestanding-csharp/classes.md "mention")

```csharp
namespace Library.Controller;
public static class FileController
{}
```

### Methods

* [GetFile](file-controller.md#get-encoding)
* [GetEncoding](file-controller.md#get-encoding)
* [IsText](file-controller.md#is-text)

## Get Encoding

create a `public` `static` method named `GetEncoding` that takes a `System.IO.FileStream` as a parameter and returns `System.Text.Encoding`

```csharp
public static Encoding GetEncoding(FileStream fs){}
```

create a byte buffer

```csharp
byte[] buffer = new byte[4];
```

read the first 4 bytes from the file and populate the buffer with them

```csharp
fs.Read(buffer, 0, 4);
```

Check if the first 4 bytes contain Unicode characters

```csharp
if (buffer.Length >= 2 && buffer[0] == 0xFF && buffer[1] == 0xFE)
{
    return Encoding.Unicode;
}
```

Check if it contains UTF-16

```csharp
if (buffer.Length >= 2 && buffer[0] == 0xFE && buffer[1] == 0xFF)
{
    return Encoding.BigEndianUnicode;
}
```

Check if it contains UTF-8

```csharp
if (buffer.Length >= 3 && buffer[0] == 0xEF && buffer[1] == 0xBB && buffer[2] == 0xBF)
{
    return Encoding.UTF8;
}
```

Otherwise return ASCII

```csharp
return Encoding.ASCII;
```

### Overview

```csharp
public static Encoding GetEncoding(FileStream fs)
{
    // Read the first few bytes from the file
    byte[] buffer = new byte[4];
    fs.Read(buffer, 0, 4);

    // Check for Unicode byte order marks (BOM)
    if (buffer.Length >= 2 && buffer[0] == 0xFF && buffer[1] == 0xFE)
    {
        return Encoding.Unicode; // UTF-16 (little-endian)
    }

    if (buffer.Length >= 2 && buffer[0] == 0xFE && buffer[1] == 0xFF)
    {
        return Encoding.BigEndianUnicode; // UTF-16 (big-endian)
    }

    if (buffer.Length >= 3 && buffer[0] == 0xEF && buffer[1] == 0xBB && buffer[2] == 0xBF)
    {
        return Encoding.UTF8; // UTF-8
    }

    // No BOM found, default to ASCII
    return Encoding.ASCII;
}
```

## Is Text

This checks if the encoding type is text-based.

```csharp
public static bool IsText(Encoding encoding) =>
    encoding == Encoding.UTF8 ||
    encoding == Encoding.ASCII ||
    encoding == Encoding.Unicode ||
    encoding == Encoding.UTF32;
```

## Get File

This method takes in a string for a path and returns [file-model.md](../models/file-model.md "mention")

First create a `FileInfo` variable named `info`

```csharp
FileInfo info = new(path);
```

Next initialize the default values

```csharp
int lines = 0;
int characters = 0;
Encoding encoding = Encoding.ASCII;
```

Next create a file stream opening the file and reading it

```csharp
using (FileStream fs = new(path, FileMode.Open, FileAccess.Read, FileShare.Read))
{}
```

Now get the files encoding type

See: [#get-encoding](file-controller.md#get-encoding "mention")

```csharp
encoding = GetEncoding(fs);
```

Check if the encoding type is text

```csharp
if (IsText(encoding))
{}
```

Create a stream reader and read the contents of the file

```csharp
using StreamReader reader = new(fs);
string content = reader.ReadToEnd();
```

Now get the number of lines and number of characters

```csharp
lines = content.Split("\n").Length;
characters = content.Length;
```

Now close the File Stream

Finally return the resulting [File Model](../models/file-model.md)

```csharp
FileModel model = new()
{
    Extension = info.Extension,
    NumberOfFiles = 1,
    Bytes = info.Length,
    Encoding = encoding,
    Lines = lines,
    Characters = characters
};

return model;
```

### Overview

```csharp
public static FileModel GetFile(string path)
{
    FileInfo info = new(path);

    // Initializes default values
    int lines = 0;
    int characters = 0;
    Encoding encoding = Encoding.ASCII;

    // Opens the file stream and locks the file from being written to.
    using (FileStream fs = new(path, FileMode.Open, FileAccess.Read, FileShare.Read))
    {
        encoding = GetEncoding(fs);

        if (IsText(encoding))
        {
            // Creates a read stream for the file
            using StreamReader reader = new(fs);

            // Reads the file's content
            string content = reader.ReadToEnd();

            // Splits the content by the new line character (\n)
            lines = content.Split("\n").Length;

            // Splits the content by each individual character.
            characters = content.Length;
        }
    }

    FileModel model = new()
    {
        Extension = info.Extension,
        NumberOfFiles = 1,
        Bytes = info.Length,
        Encoding = encoding,
        Lines = lines,
        Characters = characters
    };

    return model;
}
```

## Overview

```csharp
using Library.Model;
using System.Text;

namespace Library.Controller;

/// <summary>
/// Provides methods for working with files.
/// </summary>
public static class FileController
{
    /// <summary>
    /// Retrieves information about a file.
    /// </summary>
    /// <param name="path">The absolute path to the file.</param>
    /// <returns>A FileModel object containing file information.</returns>
    public static FileModel GetFile(string path)
    {
        FileInfo info = new(path);

        // Initializes default values
        int lines = 0;
        int characters = 0;
        Encoding encoding = Encoding.ASCII;

        // Opens the file stream and locks the file from being written to.
        using (FileStream fs = new(path, FileMode.Open, FileAccess.Read, FileShare.Read))
        {
            encoding = GetEncoding(fs);

            if (IsText(encoding))
            {
                // Creates a read stream for the file
                using StreamReader reader = new(fs);

                // Reads the file's content
                string content = reader.ReadToEnd();

                // Splits the content by the new line character (\n)
                lines = content.Split("\n").Length;

                // Splits the content by each individual character.
                characters = content.Length;
            }
        }

        FileModel model = new()
        {
            Extension = info.Extension,
            NumberOfFiles = 1,
            Bytes = info.Length,
            Encoding = encoding,
            Lines = lines,
            Characters = characters
        };

        return model;
    }

    /// <summary>
    /// Gets the encoding type of a file.
    /// </summary>
    /// <param name="fs">The FileStream representing the file.</param>
    /// <returns>The encoding type of the file.</returns>
    public static Encoding GetEncoding(FileStream fs)
    {
        // Read the first few bytes from the file
        byte[] buffer = new byte[4];
        fs.Read(buffer, 0, 4);

        // Check for Unicode byte order marks (BOM)
        if (buffer.Length >= 2 && buffer[0] == 0xFF && buffer[1] == 0xFE)
        {
            return Encoding.Unicode; // UTF-16 (little-endian)
        }

        if (buffer.Length >= 2 && buffer[0] == 0xFE && buffer[1] == 0xFF)
        {
            return Encoding.BigEndianUnicode; // UTF-16 (big-endian)
        }

        if (buffer.Length >= 3 && buffer[0] == 0xEF && buffer[1] == 0xBB && buffer[2] == 0xBF)
        {
            return Encoding.UTF8; // UTF-8
        }

        // No BOM found, default to ASCII
        return Encoding.ASCII;
    }

    /// <summary>
    /// Checks if the encoding type is text-based.
    /// </summary>
    /// <param name="encoding">The encoding type to check.</param>
    /// <returns>True if the encoding is text-based; otherwise, false.</returns>
    public static bool IsText(Encoding encoding) =>
        encoding == Encoding.UTF8 ||
        encoding == Encoding.ASCII ||
        encoding == Encoding.Unicode ||
        encoding == Encoding.UTF32;
}
```
