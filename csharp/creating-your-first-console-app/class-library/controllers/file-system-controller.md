# File System Controller

## Create Class

Create a static class called `FileSystemController` in the `Controller` namespace

See: [classes.md](../../../undestanding-csharp/classes.md "mention")

```csharp
namespace Library.Controller;
public static class FileSystemController
{}
```

### Methods

* Directory Iterator
* Scan
* Print
* Save Result
* Save
* Open File

## Directory Iterator

create a `public` `static` `async` method named `DirectoryIterator` that takes a `String` as a parameter and returns `Task<(ConcurrentDictionary<string, FileModel> Model, int FileCount)>`

```csharp
private static async Task<(ConcurrentDictionary<string, FileModel> Model, int FileCount)> DirectoryIterator(string path)
```

create a Concurrent Dictionary with a String and a FileModel, Concurrent Dictionaries are Dictionaries that are thread safe.

```csharp
ConcurrentDictionary<string, FileModel> fileData = new();
```

create an array of all files.

```csharp
string[] entries = Directory.GetFiles(path, "*.*", SearchOption.AllDirectories);
```

Create a Parallel foreach loop through all file entries, a Parallel foreach is a foreach loop that splits the given array into many smaller arrays and creates a Thread around them, then process's the array. \
See: [parallel-tasks.md](../../../undestanding-csharp/tasks-and-async/parallel-tasks.md "mention")

```csharp
Parallel.ForEach(entries, entry =>
{
});
```

Create a Nullable string named `extension`\
See: [nullable-variables.md](../../../undestanding-csharp/variables/nullable-variables.md "mention")

```csharp
string? extension = Path.GetExtension(entry);
if (extension != null)
{
    // Do Stuff
}
```

Now get the FileModel instance\
See: [#get-file](file-controller.md#get-file "mention")

```csharp
FileModel instance = FileController.GetFile(entry);
```

Check if the file data already contains the instance extension and if it does append the instance to it

```csharp
if (fileData.ContainsKey(extension))
{
    // Appends the result of "model" to "instance"
    lock (fileData)
    {
        fileData[extension] = fileData[extension] with
        {
            Lines = fileData[extension].Lines + instance.Lines,
            Bytes = fileData[extension].Bytes + instance.Bytes,
            Characters = fileData[extension].Characters + instance.Characters,
            NumberOfFiles = fileData[extension].NumberOfFiles + instance.NumberOfFiles,
        };
    }
}
```

otherwise create a new entry

```csharp
else
{
    fileData[extension] = instance;
}
```

Finally iterate the file count

```csharp
fileCount++;
```

Extra Note: You may want to wrap everything in a try catch

### Overview

```csharp
private static async Task<(ConcurrentDictionary<string, FileModel> Model, int FileCount)> DirectoryIterator(string path)
{
    ConcurrentDictionary<string, FileModel> fileData = new();
    string[] entries = Directory.GetFiles(path, "*.*", SearchOption.AllDirectories);
    int fileCount = 0;
    List<Task> tasks = new();
    Parallel.ForEach(entries, entry =>
    {
        string extension = Path.GetExtension(entry);
        if (extension != null)
        {
            Log.Debug("Processing file: {FILE}", entry);
            try
            {
                // Path is a file
                FileModel instance = FileController.GetFile(entry);
                fileCount++;
                if (fileData.ContainsKey(extension))
                {
                    // Appends the result of "model" to "instance"
                    lock (fileData)
                    {
                        fileData[extension] = fileData[extension] with
                        {
                            Lines = fileData[extension].Lines + instance.Lines,
                            Bytes = fileData[extension].Bytes + instance.Bytes,
                            Characters = fileData[extension].Characters + instance.Characters,
                            NumberOfFiles = fileData[extension].NumberOfFiles + instance.NumberOfFiles,
                        };
                    }
                }
                else
                {
                    //fileData.TryAdd(extension, instance);
                    fileData[extension] = instance;
                }
            }
            catch (Exception e)
            {
                // Throws an error if the file could NOT be accessed.
                Log.Error("Unable to read file: {PATH}\n{EXCEPTION}", entry, e);
            }
        }
    });

    await Task.WhenAll(tasks);

    Log.Debug("Returning batch: {SIZE} - {FILES}", fileData.Count, fileCount);
    return (fileData, fileCount);
}
```

## Scan

create a `public` `static` `async` method named `Scan` that takes a `String` as a parameter and returns `Task<ResultModel>`

```csharp
public static async Task<ResultModel> Scan(string path)
```

Start a stopwatch

```csharp
Stopwatch stopwatch = Stopwatch.StartNew();
```

Create a variable for model and file count

```csharp
(ConcurrentDictionary<string, FileModel> models, int fileCount) = await DirectoryIterator(path);
```

Stop the stopwatch

```csharp
stopwatch.Stop();
```

Then create and return a `ResultModel`\
See: [result-model.md](../models/result-model.md "mention")

```csharp
ResultModel model = new()
{
    Items = models.Values.OrderByDescending(i => i.Lines).ToArray(),
    NumberOfExtensions = models.Count,
    Duration = stopwatch.Elapsed,
    NumberOfFiles = fileCount
};
return model;
```

### Overview

```csharp
public static async Task<ResultModel> Scan(string path)
{
    Log.Information("Starting scan of: '{PATH}'", path);
    Stopwatch stopwatch = Stopwatch.StartNew();
    (ConcurrentDictionary<string, FileModel> models, int fileCount) = await DirectoryIterator(path);

    stopwatch.Stop();
    ResultModel model = new()
    {
        Items = models.Values.OrderByDescending(i => i.Lines).ToArray(),
        NumberOfExtensions = models.Count,
        Duration = stopwatch.Elapsed,
        NumberOfFiles = fileCount
    };
    Log.Information("Scan Completed after {TIME}", model.Duration);

    return model;
}
```

## Print

create a `public` `static` method named `Print` that takes a `ResultModel` as a parameter and returns `void`

```csharp
public static void Print(ResultModel scanResult)
```

Loop through each item in `scanResult`

```csharp
foreach (FileModel model in scanResult.Items)
```

### Overview

```csharp
public static void Print(ResultModel scanResult)
{
    Console.ForegroundColor = ConsoleColor.Green;
    foreach (FileModel model in scanResult.Items)
    {
        Console.WriteLine($"{model.Extension}:");
        Console.ForegroundColor = ConsoleColor.Cyan;
        Console.WriteLine($"\t- {model.Lines:N0} Line(s)");
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine($"\t- {model.Characters:N0} Character(s)");
        Console.ForegroundColor = ConsoleColor.Magenta;
        Console.WriteLine($"\t- {model.NumberOfFiles:N0} File(s)");
        Console.ForegroundColor = ConsoleColor.DarkYellow;
        Console.WriteLine($"\t- {CLFileMath.AdjustedFileSize(model.Bytes)}");
        Console.ForegroundColor = ConsoleColor.Blue;
        Console.WriteLine($"\t- {model.Encoding.EncodingName} Encoding(s)");
    }
    Console.ResetColor();
}
```

## Save

Create a file stream and a stream writer to write the scanResult

See: [result-model.md](../models/result-model.md "mention")

```csharp
using (FileStream fs = new(outputFile, FileMode.Create, FileAccess.Write, FileShare.None))
{
    using StreamWriter writer = new(fs);
    writer.Write(scanResult);
}
```

### Overview

```csharp
public static void SaveResult(ResultModel scanResult, bool openWhenDone = true)
{
    Save(scanResult, Path.Combine(Path.GetTempPath(), Path.GetTempFileName() + ".json"), openWhenDone);
}

public static void Save(ResultModel scanResult, string outputFile, bool openWhenDone = true)
{
    using (FileStream fs = new(outputFile, FileMode.Create, FileAccess.Write, FileShare.None))
    {
        using StreamWriter writer = new(fs);
        writer.Write(scanResult);
    }
    Log.Information("Output to File: {FILE}", outputFile);
    if (openWhenDone)
    {
        OpenFile(outputFile);
    }
}
```

## Open File

Create a method named `OpenFile` that takes in a `String` named `file`

```csharp
public static void OpenFile(string file)
```

Next call `Process.Start` method with `ProcessStartInfo` with the file name of file and `UseShellExecute` set to `true`

```csharp
Process.Start(
    new ProcessStartInfo()
    {
        FileName = file,
        UseShellExecute = true,
    });
```

### Overview

```csharp
public static void OpenFile(string file)
{
    Process.Start(
        new ProcessStartInfo()
        {
            FileName = file,
            UseShellExecute = true,
        });
}
```

## Overview

```csharp
using CLMath;
using Library.Model;
using Serilog;
using System.Collections.Concurrent;
using System.Diagnostics;

namespace Library.Controller;

/// <summary>
/// Provides methods for working with the file system.
/// </summary>
public static class FileSystemController
{  
    /// <summary>
   /// Prints the scan result to the console.
   /// </summary>
   /// <param name="scanResult">The scan result to print.</param>
    public static void Print(ResultModel scanResult)
    {
        Console.ForegroundColor = ConsoleColor.Green;
        foreach (FileModel model in scanResult.Items)
        {
            Console.WriteLine($"{model.Extension}:");
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine($"\t- {model.Lines:N0} Line(s)");
            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine($"\t- {model.Characters:N0} Character(s)");
            Console.ForegroundColor = ConsoleColor.Magenta;
            Console.WriteLine($"\t- {model.NumberOfFiles:N0} File(s)");
            Console.ForegroundColor = ConsoleColor.DarkYellow;
            Console.WriteLine($"\t- {CLFileMath.AdjustedFileSize(model.Bytes)}");
            Console.ForegroundColor = ConsoleColor.Blue;
            Console.WriteLine($"\t- {model.Encoding.EncodingName} Encoding(s)");
        }
        Console.ResetColor();
    }

    /// <summary>
    /// Saves the scan result to a temporary file and opens it.
    /// </summary>
    /// <param name="scanResult">The scan result to save.</param>
    /// <param name="openWhenDone">
    /// Indicates whether to open the file when done saving (default is true).
    /// </param>

    public static void SaveResult(ResultModel scanResult, bool openWhenDone = true)
    {
        Save(scanResult, Path.Combine(Path.GetTempPath(), Path.GetTempFileName() + ".json"), openWhenDone);
    }

    /// <summary>
    /// Saves the scan result to the specified file and opens it.
    /// </summary>
    /// <param name="scanResult">The scan result to save.</param>
    /// <param name="outputFile">The path of the output file.</param>
    /// <param name="openWhenDone">
    /// Indicates whether to open the file when done saving (default is true).
    /// </param>

    public static void Save(ResultModel scanResult, string outputFile, bool openWhenDone = true)
    {
        using (FileStream fs = new(outputFile, FileMode.Create, FileAccess.Write, FileShare.None))
        {
            using StreamWriter writer = new(fs);
            writer.Write(scanResult);
        }
        Log.Information("Output to File: {FILE}", outputFile);
        if (openWhenDone)
        {
            OpenFile(outputFile);
        }
    }

    /// <summary>
    /// Opens the specified file using the default associated application.
    /// </summary>
    /// <param name="file">The path of the file to open.</param>
    public static void OpenFile(string file)
    {
        Process.Start(
            new ProcessStartInfo()
            {
                FileName = file,
                UseShellExecute = true,
            });
    }

    /// <summary>
    /// Scans the specified directory and returns the scan result.
    /// </summary>
    /// <param name="path">The path of the directory to scan.</param>
    /// <returns>The scan result.</returns>
    public static async Task<ResultModel> Scan(string path)
    {
        Log.Information("Starting scan of: '{PATH}'", path);
        Stopwatch stopwatch = Stopwatch.StartNew();
        (ConcurrentDictionary<string, FileModel> models, int fileCount) = await DirectoryIterator(path);

        stopwatch.Stop();
        ResultModel model = new()
        {
            Items = models.Values.OrderByDescending(i => i.Lines).ToArray(),
            NumberOfExtensions = models.Count,
            Duration = stopwatch.Elapsed,
            NumberOfFiles = fileCount
        };
        Log.Information("Scan Completed after {TIME}", model.Duration);

        return model;
    }

    /// <summary>
    /// Iterates through the specified directory and collects file data.
    /// </summary>
    /// <param name="path">The path of the directory to iterate.</param>
    /// <returns>A tuple containing a dictionary of file models and the total number of files.</returns>
    private static async Task<(ConcurrentDictionary<string, FileModel> Model, int FileCount)> DirectoryIterator(string path)
    {
        ConcurrentDictionary<string, FileModel> fileData = new();
        string[] entries = Directory.GetFiles(path, "*.*", SearchOption.AllDirectories);
        int fileCount = 0;
        List<Task> tasks = new();
        Parallel.ForEach(entries, entry =>
        {
            string? extension = Path.GetExtension(entry);
            if (extension != null)
            {
                Log.Debug("Processing file: {FILE}", entry);
                try
                {
                    // Path is a file
                    FileModel instance = FileController.GetFile(entry);
                    if (fileData.ContainsKey(extension))
                    {
                        // Appends the result of "model" to "instance"
                        lock (fileData)
                        {
                            fileData[extension] = fileData[extension] with
                            {
                                Lines = fileData[extension].Lines + instance.Lines,
                                Bytes = fileData[extension].Bytes + instance.Bytes,
                                Characters = fileData[extension].Characters + instance.Characters,
                                NumberOfFiles = fileData[extension].NumberOfFiles + instance.NumberOfFiles,
                            };
                        }
                    }
                    else
                    {
                        fileData[extension] = instance;
                    }
                    // Iterate the file count
                    fileCount++;
                }
                catch (Exception e)
                {
                    // Throws an error if the file could NOT be accessed.
                    Log.Error("Unable to read file: {PATH}\n{EXCEPTION}", entry, e);
                }
            }
        });

        await Task.WhenAll(tasks);

        Log.Debug("Returning batch: {SIZE} - {FILES}", fileData.Count, fileCount);
        return (fileData, fileCount);
    }
}
```
