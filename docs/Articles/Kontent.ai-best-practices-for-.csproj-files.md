# Version management

### DOs:
- Use exclusively the `.csproj`'s `Version` attribute to manage package, assembly, or any other versions.
- Use `<GenerateAssemblyInfo>true</GenerateAssemblyInfo>`

### DON'Ts:
- Don't use `PackageVersion`, `AssemblyVersion`, `AssemblyFileVersion`,`AssemblyInformationalVersion`, `VersionSuffix`, `VersionPrefix` (neither in the `.csproj` nor in the `AssemblyInfo.cs`)
- Don't use `GenerateAssemblyXYZ` attributes in `.csproj` (e.g. `GenerateAssemblyTitleAttribute`)
- Don't put any version or package information into `AssemblyInfo.cs`

To learn more about the purpose of the various attributes, read:
- https://andrewlock.net/version-vs-versionsuffix-vs-packageversion-what-do-they-all-mean/#fileversion

The `Version` attribute gets propagated to all other version numbers (unless overridden). In case you think you need more granular settings for your project, always contact devrel@kontent.ai.

## `Version`
In Kontent.ai .NET projects (such as [kontent-ai-delivery-sdk-net](https://github.com/kontent-ai/delivery-sdk-net) or [kontent-ai-management-sdk-net](https://github.com/kontent-ai/kontent-ai-management-sdk-net) we **don't store a version number anywhere in the source files**.

The version of a release is determined by a [tag version](https://help.github.com/en/articles/creating-releases) and promoted to the NuGet package through the combination of [`dotnet build -p:Version=1.2.3`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-build) and [`dotnet pack -p:PackageVersion=1.2.3`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-pack).

## Directory.build.props
This file lets you set shared properties for multiple `.csproj` files. It's automatically being picked up by MSBuild during the build. It's a good idea to use this file for attributes such as `<RepositoryUrl>`, `<IncludeSymbols>`, `<SymbolPackageFormat>`, or `<GenerateDocumentationFile>` that you typically want to manage in one place for the whole repository.

## AssemblyInfo.cs

In case you want to get rid of the `AssemblyInfo.cs` completely and the only thing stopping you are the `InternalsVisibleToAttribute`s for tests, then you can [move them to `.csproj` or `.props` file](https://stackoverflow.com/a/49978185/1332034):

```xml
<ItemGroup>
    <AssemblyAttribute Include="System.Runtime.CompilerServices.InternalsVisibleTo">
      <_Parameter1>$(MSBuildProjectName).Test</_Parameter1>
    </AssemblyAttribute>
</ItemGroup>
```

# SourceLink & Symbol publishing
It's mandatory for Kontent.ai .NET projects to have [SourceLink](https://github.com/dotnet/sourcelink/) enabled. This is the recommended configuration:

```xml
<Project Sdk="Microsoft.NET.Sdk">
 <PropertyGroup>
    <PublishRepositoryUrl>true</PublishRepositoryUrl>
    <EmbedUntrackedSources>true</EmbedUntrackedSources>
    <IncludeSymbols>true</IncludeSymbols>
    <SymbolPackageFormat>snupkg</SymbolPackageFormat>
  </PropertyGroup>
  <ItemGroup>
      <PackageReference Include="Microsoft.SourceLink.GitHub" Version="1.0.0-*" PrivateAssets="All"/>
  </ItemGroup>
</Project>
```

# Documentation generation
Always use `<GenerateDocumentationFile>true</GenerateDocumentationFile>`. Don't use the `<DocumentationFile>` attribute if not necessary.

# Repository & package metadata
The following attributes are required:
```xml
<PropertyGroup>
  <Authors>Kontent.ai</Authors>
  <Product>Kontent.ai</Product>
  <Copyright>© 2022 Kontent.ai. All rights reserved.</Copyright>
  <Description>✏️</Description>
  <PackageLicenseExpression>MIT</PackageLicenseExpression>
  <PackageProjectUrl>https://github.com/kontent-ai/✏️</PackageProjectUrl>
  <PackageIconUrl>https://github.com/kontent-ai/.github/blob/master/images/logo_nuget.png?raw=true</PackageIconUrl>
  <RepositoryUrl>https://github.com/kontent-ai/✏️.git</RepositoryUrl>
</PropertyGroup>
```

# Target framework

For all [class libraries](https://docs.microsoft.com/en-us/dotnet/standard/class-libraries) target NET Standard 2.0 and .NET 6.

```xml
<TargetFrameworks>netstandard2.0;net6.0</TargetFrameworks>
```

For other types of projects (i.e. test projects, console applications) target .NET 5 and .NET 6.

```xml
<TargetFrameworks>net5.0;net6.0</TargetFrameworks>
```

:warning: [kontent-ai-aspnetcore](https://github.com/kontent-ai/kontent-ai-aspnetcore) targets .NET 5 and .NET 6, despite being a class library. Subject to change after .NET 7 adoption.
