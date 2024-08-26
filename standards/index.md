[Home](../index.md) | [Coding Standards](../standards/index.md) | [WPF](../wpf/index.md)

# Coding Standards

1. **Naming Conventions**: Code must adhere to the [Microsoft Naming Guidelines](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/naming-guidelines).

2. **Code Analysis**: Projects must enable and follow all rules provided by [Code Analysis](https://learn.microsoft.com/en-us/visualstudio/code-quality/?view=vs-2022).

3. **StyleCop**: Projects must enable and follow all rules provided by [StyleCop](https://github.com/DotNetAnalyzers/StyleCopAnalyzers).

4. **Compilation**: Code must compile without any warnings or errors. Enable warnings as errors in all release configurations.

5. **Indentation**:
   - Use spaces instead of tabs.
   - Use 2 spaces for indentation in JSON and SGML-based languages (e.g., XML, HTML).
   - Use 4 spaces for indentation in all other languages.

6. **Web API Design**: Web API resources should follow a [RESTful web API](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design) design, adhering to a level 2 or above [maturity model](https://martinfowler.com/articles/richardsonMaturityModel.html).