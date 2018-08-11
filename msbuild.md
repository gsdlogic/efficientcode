# MSBuild Hints, Tips and Snipits

## Quick links

- [Common Macros for Build Commands and Properties](https://msdn.microsoft.com/en-us/library/c02as0cs.aspx)
- [MSBuild Reserved and Well-Known Properties](https://msdn.microsoft.com/en-us/library/ms164309.aspx)

## Execute NuGet.exe to build a package from the project file:

```
<Target Name="AfterBuild">
  <Exec Command="$(NuGetExePath) pack $(MSBuildProjectFile) -OutputDirectory $(OutDir) -Symbols -Prop Configuration=$(Configuration)" />
</Target>
```

## Copy files from the output directory and execute NuGet.exe to build a package from a .nuspec file:

```
<Target Name="AfterBuild">
  <Copy SourceFiles="@(ReferenceCopyLocalPaths->'$(OutDir)%(DestinationSubDirectory)%(Filename)%(Extension)')" DestinationFolder="$(MSBuildProjectDirectory)\package\lib\net40" />
  <Exec Command="$(MSBuildProjectDirectory)\nuget.exe pack package\$(AssemblyName).nuspec" />
  <Delete Files="@(ReferenceCopyLocalPaths->'$(OutDir)%(DestinationSubDirectory)%(Filename)%(Extension)')" />
</Target>
```

## *MultiPlatform.csproj*

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFrameworks>netstandard1.0;netstandard2.0;net40</TargetFrameworks>
    <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    <Authors>GSD Logic</Authors>
    <Company>GSD Logic</Company>
    <Copyright>Copyright © 2018 GSD Logic. All Rights Reserved.</Copyright>
    <PackageLicenseUrl>http://www.gsdlogic.com</PackageLicenseUrl>
    <PackageProjectUrl>http://www.gsdlogic.com</PackageProjectUrl>
    <NeutralLanguage>en-US</NeutralLanguage>
    <Version>1.0.0-beta0</Version>
  </PropertyGroup>

  <PropertyGroup Condition="'$(Configuration)|$(TargetFramework)|$(Platform)'=='Debug|netstandard1.0|AnyCPU'">
    <DocumentationFile>bin\Debug\netstandard1.0\MyWebApiLibrary.xml</DocumentationFile>
  </PropertyGroup>

  <PropertyGroup Condition="'$(Configuration)|$(TargetFramework)|$(Platform)'=='Release|netstandard1.0|AnyCPU'">
    <DocumentationFile>bin\Release\netstandard1.0\MyWebApiLibrary.xml</DocumentationFile>
  </PropertyGroup>

  <PropertyGroup Condition="'$(Configuration)|$(TargetFramework)|$(Platform)'=='Debug|netstandard2.0|AnyCPU'">
    <DocumentationFile>bin\Debug\netstandard2.0\MyWebApiLibrary.xml</DocumentationFile>
  </PropertyGroup>

  <PropertyGroup Condition="'$(Configuration)|$(TargetFramework)|$(Platform)'=='Release|netstandard2.0|AnyCPU'">
    <DocumentationFile>bin\Release\netstandard2.0\MyWebApiLibrary.xml</DocumentationFile>
  </PropertyGroup>

  <PropertyGroup Condition="'$(Configuration)|$(TargetFramework)|$(Platform)'=='Debug|net40|AnyCPU'">
    <DocumentationFile>bin\Debug\net40\MyWebApiLibrary.xml</DocumentationFile>
  </PropertyGroup>

  <PropertyGroup Condition="'$(Configuration)|$(TargetFramework)|$(Platform)'=='Release|net40|AnyCPU'">
    <DocumentationFile>bin\Release\net40\MyWebApiLibrary.xml</DocumentationFile>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.CodeAnalysis.FxCopAnalyzers" Version="2.6.1" PrivateAssets="all" />
    <PackageReference Include="StyleCop.Analyzers" Version="1.0.2" PrivateAssets="all" />
  </ItemGroup>

</Project>
```

## *stylecop.json*

```
{
  "$schema": "https://raw.githubusercontent.com/DotNetAnalyzers/StyleCopAnalyzers/master/StyleCop.Analyzers/StyleCop.Analyzers/Settings/stylecop.schema.json",
  "settings": {
    "documentationRules": {
      "companyName": "GSD Logic",
      "copyrightText": "Copyright © 2018 {companyName}. All Rights Reserved.",
      "documentInterfaces": true,
      "documentExposedElements": true,
      "documentInternalElements": true,
      "documentPrivateElements": true,
      "documentPrivateFields": true,
      "xmlHeader": true
    }
  }
}
```
