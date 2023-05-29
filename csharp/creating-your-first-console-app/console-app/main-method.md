# Main Method

## Using Statements

```csharp
using Library.Controller;
using Library.Model;
using Serilog;
using Serilog.Events;
using Serilog.Sinks.SystemConsole.Themes;
```

Create a static async task Main with string\[] args

```csharp
private static async Task Main(string[] args)
```

create a logger template

### Serilog Templates

Serilog provides different options for message templates, which allow you to format log messages in a flexible way. Some of the template options that can be used in message templates include:

* `{Timestamp}`: The timestamp of the logging event.
* `{Level}`: The logging level of the event (e.g. Information, Warning, Error).
* `{Message}`: The message of the logging event.
* `{Exception}`: The exception associated with the logging event.
* `{SourceContext}`: The name of the logger that generated the event.
* `{Properties}`: A collection of all the properties associated with the logging event.

In addition to these built-in properties, you can also include custom properties in your message templates. You can include formatting options in your message templates, such as:

* `{Timestamp:yyyy-MM-dd HH:mm:ss.fff}`: Formats the timestamp as a string in the specified format.
* `{Level:u3}`: Formats the logging level as a string in uppercase with a minimum width of 3 characters.

```csharp
string template = "[Line Scanner] [{Level}] {Message:lj}{NewLine}{Exception}";
```

Create custom serilog styles

```csharp
Dictionary<ConsoleThemeStyle, SystemConsoleThemeStyle> customThemeStyles = new()
{
    {
        ConsoleThemeStyle.Text, new SystemConsoleThemeStyle
        {
            Foreground = ConsoleColor.White,
        }
    },
    {
        ConsoleThemeStyle.String, new SystemConsoleThemeStyle
        {
            Foreground = ConsoleColor.Yellow,
        }
    },
    {
        ConsoleThemeStyle.Scalar, new SystemConsoleThemeStyle
        {
            Foreground = ConsoleColor.Green,
        }
    },
    {
        ConsoleThemeStyle.Number, new SystemConsoleThemeStyle
        {
            Foreground = ConsoleColor.Blue,
        }
    },
    {
        ConsoleThemeStyle.Boolean, new SystemConsoleThemeStyle
        {
            Foreground = ConsoleColor.Cyan,
        }
    },
    {
        ConsoleThemeStyle.Null, new SystemConsoleThemeStyle
        {
            Foreground = ConsoleColor.Red,
        }
    },
    {
        ConsoleThemeStyle.LevelDebug, new SystemConsoleThemeStyle
        {
            Foreground = ConsoleColor.Blue,
        }
    },
    {
        ConsoleThemeStyle.LevelInformation, new SystemConsoleThemeStyle
        {
            Foreground = ConsoleColor.Green,
        }
    },
    {
        ConsoleThemeStyle.LevelWarning, new SystemConsoleThemeStyle
        {
            Foreground = ConsoleColor.DarkYellow,
        }
    },
    {
        ConsoleThemeStyle.LevelError, new SystemConsoleThemeStyle
        {
            Foreground = ConsoleColor.Red,
        }
    },
    {
        ConsoleThemeStyle.LevelFatal, new SystemConsoleThemeStyle
        {
            Foreground = ConsoleColor.DarkRed,
        }
    },
};
```

Create Logger

This creates a logger with the specified template and theme, sets the minimum log level based on applications configured release mode.\
See: [preprocessor-statements.md](../../undestanding-csharp/preprocessor-statements.md "mention")

```csharp
Log.Logger = new LoggerConfiguration()
.WriteTo.Console(LogEventLevel.Debug, outputTemplate: template, theme: new SystemConsoleTheme(customThemeStyles))
#if DEBUG
.MinimumLevel.Verbose()
#else
.MinimumLevel.Information()
#endif
.CreateLogger();
```

Now create a variable for path and set it to the zeroth argument and if the application is in debug mode it adds console input, you can change this if you would like.

```csharp
string path = Environment.CurrentDirectory;
if (args.Any())
{
    path = args[0];
}
#if DEBUG
else
{
    Console.ForegroundColor = ConsoleColor.Blue;
    Console.Write("Path: ");
    Console.ResetColor();
    path = Path.GetFullPath(Console.ReadLine() ?? path);
}
#endif
```

## Calls

Finally make the calls to [FileSystemController](../class-library/controllers/file-system-controller.md)

```csharp
ResultModel result = await FileSystemController.Scan(path);
FileSystemController.SaveResult(result);
FileSystemController.Print(result);
```

and reset the console's color

```csharp
Console.ResetColor();
```

## Overview

```csharp
using Library.Controller;
using Library.Model;
using Serilog;
using Serilog.Events;
using Serilog.Sinks.SystemConsole.Themes;

namespace Application;

internal class Program
{
    private static async Task Main(string[] args)
    {
        string template = "[Line Scanner] [{Level}] {Message:lj}{NewLine}{Exception}";
        Dictionary<ConsoleThemeStyle, SystemConsoleThemeStyle> customThemeStyles = new()
        {
            {
                ConsoleThemeStyle.Text, new SystemConsoleThemeStyle
                {
                    Foreground = ConsoleColor.White,
                }
            },
            {
                ConsoleThemeStyle.String, new SystemConsoleThemeStyle
                {
                    Foreground = ConsoleColor.Yellow,
                }
            },
            {
                ConsoleThemeStyle.Scalar, new SystemConsoleThemeStyle
                {
                    Foreground = ConsoleColor.Green,
                }
            },
            {
                ConsoleThemeStyle.Number, new SystemConsoleThemeStyle
                {
                    Foreground = ConsoleColor.Blue,
                }
            },
            {
                ConsoleThemeStyle.Boolean, new SystemConsoleThemeStyle
                {
                    Foreground = ConsoleColor.Cyan,
                }
            },
            {
                ConsoleThemeStyle.Null, new SystemConsoleThemeStyle
                {
                    Foreground = ConsoleColor.Red,
                }
            },
            {
                ConsoleThemeStyle.LevelDebug, new SystemConsoleThemeStyle
                {
                    Foreground = ConsoleColor.Blue,
                }
            },
            {
                ConsoleThemeStyle.LevelInformation, new SystemConsoleThemeStyle
                {
                    Foreground = ConsoleColor.Green,
                }
            },
            {
                ConsoleThemeStyle.LevelWarning, new SystemConsoleThemeStyle
                {
                    Foreground = ConsoleColor.DarkYellow,
                }
            },
            {
                ConsoleThemeStyle.LevelError, new SystemConsoleThemeStyle
                {
                    Foreground = ConsoleColor.Red,
                }
            },
            {
                ConsoleThemeStyle.LevelFatal, new SystemConsoleThemeStyle
                {
                    Foreground = ConsoleColor.DarkRed,
                }
            },
        };
        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console(LogEventLevel.Debug, outputTemplate: template, theme: new SystemConsoleTheme(customThemeStyles))
#if DEBUG
            .MinimumLevel.Verbose()
#else
            .MinimumLevel.Information()
#endif
            .CreateLogger();

        string path = Environment.CurrentDirectory;
        if (args.Any())
        {
            path = args[0];
        }
#if DEBUG
        else
        {
            Console.ForegroundColor = ConsoleColor.Blue;
            Console.Write("Path: ");
            Console.ResetColor();
            path = Path.GetFullPath(Console.ReadLine() ?? path);
        }
#endif

        ResultModel result = await FileSystemController.Scan(path);
        FileSystemController.SaveResult(result);
        FileSystemController.Print(result);
        Console.ResetColor();
    }
}
```
