## Class Library (.csproj)

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <LangVersion>latest</LangVersion>
    <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    <Product>{Product}</Product>
    <Authors>{Company}</Authors>
    <Company>{Company}</Company>
    <Copyright>Copyright © {Year} {Company}. All Rights Reserved.</Copyright>
    <Version>0.0.0.0</Version>
    <InformationalVersion>0.0.0.0</InformationalVersion>
    <AssemblyVersion>0.0.0.0</AssemblyVersion>
    <FileVersion>0.0.0.0</FileVersion>
    <NeutralLanguage>en-US</NeutralLanguage>
    <EnforceCodeStyleInBuild>true</EnforceCodeStyleInBuild>
    <EnableNETAnalyzers>true</EnableNETAnalyzers>
    <AnalysisMode>AllEnabledByDefault</AnalysisMode>
    <SignAssembly>true</SignAssembly>
    <AssemblyOriginatorKeyFile>{Assembly}.snk</AssemblyOriginatorKeyFile>
    <DocumentationFile>bin\$(Configuration)\$(TargetFramework)\{Assembly}.xml</DocumentationFile>
  </PropertyGroup>

  <PropertyGroup Condition="'$(Configuration)|$(Platform)'=='Release|AnyCPU'">
    <TreatWarningsAsErrors>true</TreatWarningsAsErrors>
  </PropertyGroup>

  <ItemGroup>
    <None Remove="stylecop.json" />
  </ItemGroup>

  <ItemGroup>
    <AdditionalFiles Include="stylecop.json" />
  </ItemGroup>

  <ItemGroup>
    <PackageReference Include="StyleCop.Analyzers" Version="1.1.118" PrivateAssets="all" />
  </ItemGroup>

</Project>
```

## stylecop.json

```
{
  "$schema": "https://raw.githubusercontent.com/DotNetAnalyzers/StyleCopAnalyzers/master/StyleCop.Analyzers/StyleCop.Analyzers/Settings/stylecop.schema.json",
  "settings": {
    "documentationRules": {
      "companyName": "{Company}",
      "copyrightText": "Copyright © {Year} {Company}. All Rights Reserved.",
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
