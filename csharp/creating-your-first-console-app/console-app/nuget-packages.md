# Nuget Packages

This project uses `Newtonsoft.JSON` and `Serilog` and `CLMath`

## Install Command

```bash
Install-Package CLMath
Install-Package Newtonsoft.JSON
Install-Package Serilog
Install-Package Serilog.Sinks.Console
```

## CSProj File

```xml
  <ItemGroup>
    <PackageReference Include="CLMath" Version="0.1.3" />
    <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
    <PackageReference Include="Serilog" Version="2.12.0" />
    <PackageReference Include="Serilog.Sinks.Console" Version="4.1.0" />
  </ItemGroup>
```
