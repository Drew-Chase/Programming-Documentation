# Result Model

Lets create a `struct` in our `Models` namespace and name it `ResultModel.cs`\
See: [struct-vs-class.md](../../../understanding-c/struct-vs-class.md "mention")

## Fields

Now we will create four properties\
See: [variables-vs-properties.md](../../../understanding-c/variables/variables-vs-properties.md "mention")

<table><thead><tr><th>Name</th><th>Type</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>Count</td><td>int</td><td>The number of results</td><td></td></tr><tr><td>NumberOfFiles</td><td>int</td><td>The total number of files processed</td><td></td></tr><tr><td>Duration</td><td>TimeSpan</td><td>The time it took to compile results</td><td></td></tr><tr><td>Items</td><td>FileModel[]</td><td>The file extensions processed</td><td></td></tr></tbody></table>

See: [primitive-types.md](../../../understanding-c/variables/primitive-types.md "mention")

## Example:

```csharp
namespace Library.Model;

public struct ResultModel
{
    // The number of results
    public int Count { get; set; }

    // The total number of files processed
    public int NumberOfFiles { get; set; }

    // The time it took to compile results
    public TimeSpan Duration { get; set; }

    // The file extensions processed
    public FileModel[] Items { get; set; }
}
```
