```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <LangVersion>latest</LangVersion>
    <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    <Product>{Product}</Product>
    <Authors>{Company}</Authors>
    <Company>{Company}</Company>
    <Copyright>Copyright © 2022 {Company}. All Rights Reserved.</Copyright>
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
