# Crypt Class

The `Crypt` class is a part of the Chase CommonLib library developed by LFInteractive LLC. This class is designed for encrypting and decrypting strings and files using the AES encryption algorithm. It provides methods to encrypt and decrypt data and allows you to specify a custom salt for added security.

### Class Overview

#### Namespace

```csharp
namespace Chase.CommonLib.Math;
```

#### License

The `Crypt` class is licensed under the [GPL-3.0 license](https://www.gnu.org/licenses/gpl-3.0.en.html#license-text).

#### Summary

A class for encrypting and decrypting strings and files using the AES encryption algorithm.

### Constructor

#### `Crypt()`

Creates a new instance of the `Crypt` class with a random salt based on the current machine.

```csharp
public Crypt();
```

#### `Crypt(string salt)`

Creates a new instance of the `Crypt` class with the specified salt.

* `salt`: A custom salt value used for encryption and decryption.

```csharp
public Crypt(string salt);
```

### Properties

#### `Salt`

Gets or sets the salt used for encryption and decryption.

```csharp
public string Salt { get; set; }
```

### Methods

#### `Encrypt(string text)`

Encrypts the specified text using AES encryption.

* `text`: The text to be encrypted.

Returns the encrypted text as a Base64-encoded string.

```csharp
public string Encrypt(string text);
```

#### `Encrypt(string path, string output)`

Encrypts the content of a file and writes the encrypted data to an output file.

* `path`: The path to the input file.
* `output`: The path to the output file where the encrypted data will be written.

```csharp
public void Encrypt(string path, string output);
```

#### `Decrypt(string path, string output)`

Decrypts the content of a file and writes the decrypted data to an output file.

* `path`: The path to the input file containing encrypted data.
* `output`: The path to the output file where the decrypted data will be written.

```csharp
public void Decrypt(string path, string output);
```

#### `Decrypt(string text)`

Decrypts the specified Base64-encoded text using AES decryption.

* `text`: The Base64-encoded text to be decrypted.

Returns the decrypted text.

```csharp
public string Decrypt(string text);
```

### Private Methods

#### `GetSaltBytes()`

Gets the salt bytes derived from the `Salt` property.

```csharp
private byte[] GetSaltBytes();
```

### Examples

#### Example 1: Encrypting and Decrypting Text

```csharp
// Create a Crypt instance with a custom salt
Crypt crypt = new Crypt("MyCustomSalt");

// Encrypt a text
string encryptedText = crypt.Encrypt("SensitiveData123");

// Decrypt the encrypted text
string decryptedText = crypt.Decrypt(encryptedText);

Console.WriteLine("Original Text: SensitiveData123");
Console.WriteLine("Encrypted Text: " + encryptedText);
Console.WriteLine("Decrypted Text: " + decryptedText);
```

#### Example 2: Encrypting and Decrypting Files

```csharp
// Create a Crypt instance with a custom salt
Crypt crypt = new Crypt("MyCustomSalt");

// Encrypt a file
crypt.Encrypt("input.txt", "encrypted_output.dat");

// Decrypt the encrypted file
crypt.Decrypt("encrypted_output.dat", "decrypted_output.txt");
```

This documentation provides an overview of the `Crypt` class, its constructor, properties, methods, and includes example code demonstrating how to use it for text and file encryption and decryption.
