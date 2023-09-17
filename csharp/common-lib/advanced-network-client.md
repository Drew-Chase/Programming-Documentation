# Advanced Network Client

The `AdvancedNetworkClient` class is a modified version of the `HttpClient` class that provides additional features for network operations in .NET 6.0+. This class extends the functionality of `HttpClient` to simplify common networking tasks such as downloading files, uploading files, and making HTTP requests that return JSON data.

### Class Overview

#### Namespace

```csharp
namespace Chase.CommonLib.Networking;
```

#### Inheritance

* Inherits from: `HttpClient`

#### License

* This class is licensed under the GNU General Public License (GPL-3.0). For more details, refer to [GPL-3.0 License](https://www.gnu.org/licenses/gpl-3.0.en.html#license-text).

### Methods

#### DownloadFileAsync

```csharp
public async Task<string> DownloadFileAsync(Uri address, string path, DownloadProgressEvent? progress = null)
```

* **Description**: Downloads a file from the specified URI and saves it to the given file path.
* **Parameters**:
  * `address` (Uri): The direct download URI of the file.
  * `path` (string): The directory or absolute file path where the downloaded file will be saved.
  * `progress` (DownloadProgressEvent?): An optional event handler for tracking the download progress.
* **Returns**:
  * A task that represents the asynchronous operation and returns the path of the downloaded file.

#### UploadFileAsync

```csharp
public async Task<HttpResponseMessage> UploadFileAsync(string address, string file, DownloadProgressEvent? progress = null)
```

* **Description**: Uploads a file to the specified URI using an HTTP POST request.
* **Parameters**:
  * `address` (string): The destination URI where the file will be uploaded.
  * `file` (string): The path of the file to be uploaded.
  * `progress` (DownloadProgressEvent?): An optional event handler for tracking the upload progress.
* **Returns**:
  * A task that represents the asynchronous operation and returns the HTTP response message.

#### GetAsJson

```csharp
public async Task<JObject?> GetAsJson(HttpRequestMessage requestMessage)
```

* **Description**: Sends an HTTP request using the provided `HttpRequestMessage` and parses the response content as a JSON object (JObject).
* **Parameters**:
  * `requestMessage` (HttpRequestMessage): The HTTP request message to send.
* **Returns**:
  * A task that represents the asynchronous operation and returns a JObject representing the parsed JSON response, or null if the response is null.

#### GetAsJsonArray

```csharp
public async Task<JArray?> GetAsJsonArray(HttpRequestMessage requestMessage)
```

* **Description**: Sends an HTTP request using the provided `HttpRequestMessage` and parses the response content as a JSON array (JArray).
* **Parameters**:
  * `requestMessage` (HttpRequestMessage): The HTTP request message to send.
* **Returns**:
  * A task that represents the asynchronous operation and returns a JArray representing the parsed JSON response, or null if the response is null.

### Example Usage

Here are some example usages of the `AdvancedNetworkClient` class:

```csharp
// Create an instance of AdvancedNetworkClient
var client = new AdvancedNetworkClient();

// Example 1: Download a file and track progress
string downloadUri = "https://example.com/somefile.zip";
string downloadPath = "C:\\Downloads\\somefile.zip";
await client.DownloadFileAsync(new Uri(downloadUri), downloadPath, (sender, e) =>
{
    Console.WriteLine($"Downloading: {e.FileName}, Progress: {e.Progress * 100}%");
});

// Example 2: Upload a file and track progress
string uploadUri = "https://example.com/upload";
string uploadFilePath = "C:\\Documents\\document.pdf";
await client.UploadFileAsync(uploadUri, uploadFilePath, (sender, e) =>
{
    Console.WriteLine($"Uploading: {e.FileName}, Progress: {e.Progress * 100}%");
});

// Example 3: Send an HTTP GET request and parse the response as JSON
string apiUri = "https://api.example.com/data";
HttpRequestMessage getRequest = new HttpRequestMessage(HttpMethod.Get, apiUri);
JObject? jsonResponse = await client.GetAsJson(getRequest);
if (jsonResponse != null)
{
    Console.WriteLine($"Received JSON Response: {jsonResponse.ToString()}");
}

// Example 4: Send an HTTP GET request and parse the response as JSON array
string arrayUri = "https://api.example.com/array";
HttpRequestMessage arrayRequest = new HttpRequestMessage(HttpMethod.Get, arrayUri);
JArray? jsonArrayResponse = await client.GetAsJsonArray(arrayRequest);
if (jsonArrayResponse != null)
{
    Console.WriteLine($"Received JSON Array Response: {jsonArrayResponse.ToString()}");
}
```

These examples demonstrate how to use the `AdvancedNetworkClient` class to perform various networking tasks, including file downloads, uploads, and handling JSON responses. The optional progress event handlers allow you to track the progress of file transfers.
