# Visual Studio Hints, Tips and Snipits

## Blank Solution

1. File => New Project => Other Project Types => Visual Studio Solution
2. Name: `MyProject`
3. Location: `{Root Directory of Repository}`

## Multi-platform Assembly

1. Solution => Add => New Project => .NET Core => Class Library (.NET Core)
2. Name: `MyLibrary`
3. Location: `{Soution Directory}\Source`
4. Edit *MyLibrary.csproj* => Replace all with [MultiPlatform.csproj](msbuild.md)
5. Add [stylecop.json](msbuild.md), Build Action: *C# analyzer additional file**
6. Project => Properties => Signing => Sign the assembly
