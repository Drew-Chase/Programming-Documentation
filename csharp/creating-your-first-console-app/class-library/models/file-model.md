---
description: This will hold information on each file type
---

# File Model

Lets create a `struct` in our `Models` namespace and name it `FileModel.cs`\
See: [struct-vs-class.md](../../../understanding-c/struct-vs-class.md "mention")

## Fields

Now we will create five properties\
See: [variables-vs-properties.md](../../../understanding-c/variables/variables-vs-properties.md "mention")

<table><thead><tr><th>Name</th><th>Type</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>Extension</td><td>string</td><td>The file extension</td><td></td></tr><tr><td>Count</td><td>int</td><td>The number of files with the extension</td><td></td></tr><tr><td>Bytes</td><td>long</td><td>The total number of bytes</td><td></td></tr><tr><td>Characters</td><td>int</td><td>The total number of characters in each file</td><td></td></tr><tr><td>Encoding</td><td>System.Text.Encoding</td><td>The encoding type</td><td></td></tr></tbody></table>

See: [primitive-types.md](../../../understanding-c/variables/primitive-types.md "mention")

## Example:

```csharp
using System.Text;

namespace Library.Model;

public struct FileModel
{
    // The file extension
    public string Extension { get; set; }
    // The number of files with the extension
    public int Count { get; set; }
    // The total number of bytes
    public long Bytes { get; set; }
    // The total number of characters in each file
    public int Characters { get; set; }
    // The encoding type
    public Encoding Encoding { get; set; }
}
```
