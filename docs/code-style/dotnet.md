---
layout: default
title: .NET
parent: Code style
---

# .NET code style

Conventions for Kontent.ai .NET projects - `.csproj` setup, versioning, and packaging.

## Version management

We keep a **single source of truth** for the version: the release's Git tag. It's supplied at build time through MSBuild's `Version` property and propagates to the assembly and package versions automatically - **no version number is stored in any source file**.

### DOs
- Drive every version from the `Version` MSBuild property, supplied at build time (e.g. from the release tag in CI) rather than written into a file.
- Use `<GenerateAssemblyInfo>true</GenerateAssemblyInfo>`.

### DON'Ts
- Don't hardcode versions in source files - no `<Version>`, `<PackageVersion>`, `<AssemblyVersion>`, `<AssemblyFileVersion>`, `<AssemblyInformationalVersion>`, `<VersionSuffix>`, or `<VersionPrefix>` in the `.csproj`, and nothing version-related in `AssemblyInfo.cs`.
- Don't use `GenerateAssemblyXYZ` attributes in the `.csproj` (e.g. `GenerateAssemblyTitleAttribute`).

To learn more about what the various attributes mean, read [Version vs. VersionSuffix vs. PackageVersion](https://andrewlock.net/version-vs-versionsuffix-vs-packageversion-what-do-they-all-mean/#fileversion). If you think you need more granular settings, contact devrel@kontent.ai.

### How a release gets its version
In Kontent.ai .NET projects (such as [delivery-sdk-net](https://github.com/kontent-ai/delivery-sdk-net) or [management-sdk-net](https://github.com/kontent-ai/management-sdk-net)) the version isn't kept in the repository at all - it comes from the release's [Git tag](https://help.github.com/en/articles/creating-releases) and is passed in at build time. `Version` is the only knob you set; it flows through to the NuGet package version (restated on `pack` only because the package is built without a rebuild):

```bash
dotnet build -p:Version=1.2.3
dotnet pack --no-build -p:PackageVersion=1.2.3
```

### Directory.build.props
This file lets you set shared properties for multiple `.csproj` files. It's automatically being picked up by MSBuild during the build. It's a good idea to use this file for attributes such as `<RepositoryUrl>`, `<IncludeSymbols>`, `<SymbolPackageFormat>`, or `<GenerateDocumentationFile>` that you typically want to manage in one place for the whole repository.

### AssemblyInfo.cs

In case you want to get rid of the `AssemblyInfo.cs` completely and the only thing stopping you are the `InternalsVisibleToAttribute`s for tests, then you can [move them to `.csproj` or `.props` file](https://stackoverflow.com/a/49978185/1332034):

```xml
<ItemGroup>
    <AssemblyAttribute Include="System.Runtime.CompilerServices.InternalsVisibleTo">
      <_Parameter1>$(MSBuildProjectName).Test</_Parameter1>
    </AssemblyAttribute>
</ItemGroup>
```

## SourceLink & Symbol publishing
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

## Documentation generation
Always use `<GenerateDocumentationFile>true</GenerateDocumentationFile>`. Don't use the `<DocumentationFile>` attribute if not necessary.

## Repository & package metadata
The following attributes are required:
```xml
<PropertyGroup>
  <Authors>Kontent.ai</Authors>
  <Product>Kontent.ai</Product>
  <Copyright>© 2026 Kontent s.r.o. All rights reserved.</Copyright>
  <Description>✏️</Description>
  <PackageLicenseExpression>MIT</PackageLicenseExpression>
  <PackageProjectUrl>https://github.com/kontent-ai/✏️</PackageProjectUrl>
  <PackageIcon>icon.png</PackageIcon>
  <PackageReadmeFile>README.md</PackageReadmeFile>
  <RepositoryUrl>https://github.com/kontent-ai/✏️.git</RepositoryUrl>
</PropertyGroup>
```

The icon and README referenced above are embedded in the package (the legacy `PackageIconUrl` is deprecated), so pack the files too:

```xml
<ItemGroup>
  <None Include="icon.png" Pack="true" PackagePath="" />
  <None Include="README.md" Pack="true" PackagePath="" />
</ItemGroup>
```

## Target framework

For all [class libraries](https://learn.microsoft.com/en-us/dotnet/standard/class-libraries) target .NET Standard 2.0 and the current .NET LTS release (.NET 8 at the time of writing).

```xml
<TargetFrameworks>netstandard2.0;net8.0</TargetFrameworks>
```

For other types of projects (i.e. test projects, console applications) target the current .NET LTS release (.NET 8).

```xml
<TargetFramework>net8.0</TargetFramework>
```

> **Note**
> [aspnetcore-extensions](https://github.com/kontent-ai/aspnetcore-extensions) targets only .NET 8 (not .NET Standard 2.0), because it builds on top of ASP.NET Core.
