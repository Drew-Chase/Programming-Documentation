# VCPKG
Vcpkg is a cross-platform package manager for C and C++ developers. It allows developers to quickly acquire and build open source libraries and share collections of private components. To get started with Vcpkg, follow these steps:

- Clone the Vcpkg repository and run the bootstrapping script to produce the Vcpkg binary.
- Install Vcpkg globally for MSBuild projects or clone it as a submodule for CMake projects.
- Use `find_package()` to reference the libraries in your CMakeLists.txt files.
- Use `vcpkg` commands to search, install, remove, list, update, and upgrade packages.
- Verify that the installed libraries are discoverable by IntelliSense and usable in code without additional configuration.

For a more comprehensive guide on using Vcpkg, refer to the [official documentation](https://vcpkg.io/en/getting-started.html). If you encounter any issues, check the [known issues](https://devblogs.microsoft.com/cppblog/vcpkg-is-now-included-with-visual-studio/) and [open an issue](https://github.com/microsoft/vcpkg) on the Vcpkg GitHub repository.

## Installation

To install vcpkg, follow these steps:

- First, clone the vcpkg repository from GitHub using the following command:
```powershell
git clone https://github.com/Microsoft/vcpkg.git
```
- Navigate to the vcpkg directory and run the bootstrap script to generate the vcpkg binary:
```powershell
cd vcpkg
./bootstrap-vcpkg.sh
```
- After the bootstrap script completes, you can install packages using the `vcpkg install` command. For example, to install the Boost library, run:
```powershell
./vcpkg install boost
```
- Optionally, you can add the vcpkg directory to your system PATH to be able to use the `vcpkg` command from any directory. On Windows, you can use the following command to add the vcpkg directory to your user PATH:
```powershell
setx PATH "%PATH%;C:\path\to\vcpkg"
```
- Alternatively, you can install vcpkg globally for MSBuild projects or as a submodule for CMake projects. For more information on installation and usage, refer to the official [vcpkg documentation](https://vcpkg.io/en/getting-started.html).

Note that vcpkg can also be installed using package managers such as [Homebrew](https://formulae.brew.sh/formula/vcpkg) on macOS and Linux.

## Integration
To link vcpkg with Visual Studio, you can follow these steps:

- If you have installed Visual Studio 2015 Update 3 or later with the vcpkg component checked, you can use the copy of vcpkg that is included with Visual Studio. This copy of vcpkg supports manifests but does not support classic mode. To initialize it with the IDE, run `vcpkg integrate install` with administrator privileges from the Developer Command Prompt for Visual Studio or Developer PowerShell for Visual Studio. This will enable MSBuild and CMake IDE integration with this copy of vcpkg.
- If you have installed vcpkg manually, you can integrate it with Visual Studio using the `vcpkg integrate install` command. This command sets the user-wide vcpkg instance and displays CMake integration help. On Windows with Visual Studio 2015, this subcommand will add redirecting logic into the MSBuild installation which will automatically pick up each user's user-wide vcpkg instance. Visual Studio 2017 and newer have this logic in the box. This command also creates a few short files containing the absolute path to the vcpkg instance inside the user's user-wide configuration location.
- In Visual Studio, you can then include the vcpkg libraries in your project by adding the appropriate `#include` statements and linking your project to the vcpkg-generated libraries. This can be done through the Visual Studio project properties or by manually editing the project file. For example, to link to the Boost library, you can add the following to your project file:
```xml
<ItemDefinitionGroup>
  <ClCompile>
    <AdditionalIncludeDirectories>$(VCPKG_ROOT)/installed/x64-windows/include/boost</AdditionalIncludeDirectories>
  </ClCompile>
  <Link>
    <AdditionalLibraryDirectories>$(VCPKG_ROOT)/installed/x64-windows/lib</AdditionalLibraryDirectories>
    <AdditionalDependencies>libboost_system-vc142-mt-x64-1_74.lib</AdditionalDependencies>
  </Link>
</ItemDefinitionGroup>
```
- If you want to statically link the vcpkg libraries to your project, you can configure Visual Studio to use the static libraries downloaded by vcpkg. To do this, you need to change the Runtime Library property of your project to `Multi-threaded (/MT)` for Release mode and `Multi-threaded Debug (/MTd)` for Debug mode. You can also set the `VcpkgTriplet` property in the project file to `x64-windows-static` to use the static libraries. For more information on static linking with vcpkg and Visual Studio, refer to [this tutorial](https://levelup.gitconnected.com/how-to-statically-link-c-libraries-with-vcpkg-visual-studio-2019-435c2d4ace03).

### Notes
I've found that you might need to restart your computer before Visual Studio fully connects to vcpkg.