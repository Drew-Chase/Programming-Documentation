# Strings

The `Strings` class is a part of the Chase CommonLib library developed by LFInteractive LLC. It provides a set of static methods for performing various string-related operations in .NET 6.0 and higher. This documentation will describe the methods available in the `Strings` class and provide example code snippets for each method.

***

### Methods

#### GetValidFileName

```csharp
/// <summary>
/// Gets a valid file name from the original string.
/// </summary>
/// <param name="original">The original string to process.</param>
/// <returns>A valid file name derived from the original string.</returns>
public static string GetValidFileName(string original)
```

This method takes an original string and removes any characters that are not valid in a file name. It iterates through the characters of the original string and replaces any invalid characters with an empty string. The resulting string is a valid file name that can be used to create files.

**Example:**

```csharp
string originalFileName = "my*file?.txt";
string validFileName = Strings.GetValidFileName(originalFileName);
Console.WriteLine(validFileName); // Output: "myfile.txt"
```

#### IsAlphaNumeric

```csharp
/// <summary>
/// Checks if the string is alphanumeric.
/// </summary>
/// <param name="str">The string to check.</param>
/// <returns>True if the string is alphanumeric, false otherwise.</returns>
public static bool IsAlphaNumeric(string str)
```

This method checks whether a given string consists of only alphanumeric characters (letters and digits). It iterates through the characters of the input string and returns `true` if all characters are alphanumeric, and `false` if any non-alphanumeric character is found.

**Example:**

```csharp
string alphanumericStr = "abc123";
string nonAlphanumericStr = "abc123!";
bool isAlphanumeric1 = Strings.IsAlphaNumeric(alphanumericStr); // true
bool isAlphanumeric2 = Strings.IsAlphaNumeric(nonAlphanumericStr); // false
```

#### IsAlphabetical

```csharp
/// <summary>
/// Checks if the string is alphabetical.
/// </summary>
/// <param name="str">The string to check.</param>
/// <returns>True if the string is alphabetical, false otherwise.</returns>
public static bool IsAlphabetical(string str)
```

This method checks whether a given string consists of only alphabetical characters (letters). It iterates through the characters of the input string and returns `true` if all characters are letters, and `false` if any non-letter character is found.

**Example:**

```csharp
string alphabeticalStr = "abc";
string mixedStr = "abc123";
bool isAlphabetical1 = Strings.IsAlphabetical(alphabeticalStr); // true
bool isAlphabetical2 = Strings.IsAlphabetical(mixedStr); // false
```

***

This concludes the documentation for the `Strings` class in the Chase CommonLib library. You can use the provided methods to perform common string-related operations in your .NET applications.
