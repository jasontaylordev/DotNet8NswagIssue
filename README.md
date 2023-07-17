# NSwag .NET 8 Issue

The upcoming release of .NET 8 introduces [simplified output paths](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-8#simplified-output-paths), 
an option to simplify the output path and folder structure for build outputs (e.g. bin and obj). 
**NSwag.MSBuild** fails when this option is enabled. This repository was created to demonstrate the issue so that it can be resolved.

# Steps to Reproduce

1. Download .NET 8 SDK (Preview 6)
1. Clone this repo and run `dotnet build`

You will see this error:

```bash
MSBuild version 17.7.0+5785ed5c2 for .NET
  Determining projects to restore...
  All projects are up-to-date for restore.
C:\Program Files\dotnet\sdk\8.0.100-preview.6.23330.14\Sdks\Microsoft.NET.Sdk\targets\Microsoft.NET.RuntimeIdentifierIn
ference.targets(314,5): message NETSDK1057: You are using a preview version of .NET. See: https://aka.ms/dotnet-support
-policy [D:\Code\JasonTaylorDev\DotNet8NswagIssue\WebApi\WebApi.csproj]
  WebApi -> D:\Code\JasonTaylorDev\DotNet8NswagIssue\artifacts\bin\WebApi\debug\WebApi.dll
  NSwag command line tool for .NET Core Net70, toolchain v13.19.0.0 (NJsonSchema v10.9.0.0 (Newtonsoft.Json v13.0.0.0))
  Visit http://NSwag.org for more information.
  NSwag bin directory: C:\Users\jason\.nuget\packages\nswag.msbuild\13.19.0\tools\Net70

  Executing file 'D:\Code\JasonTaylorDev\DotNet8NswagIssue\WebApi\config.nswag' with variables 'Configuration=Debug'...
D:\Code\JasonTaylorDev\DotNet8NswagIssue\WebApi\WebApi.csproj : error MSB4057: The target "__GetNSwagProjectMetadata" d
oes not exist in the project.
  System.InvalidOperationException: Unable to retrieve project metadata. Ensure it's an MSBuild-based .NET Core project
  .If you're using custom BaseIntermediateOutputPath or MSBuildProjectExtensionsPath values, Use the --msbuildprojectex
  tensionspath option.
     at NSwag.Commands.Generation.AspNetCore.ProjectMetadata.GetProjectMetadata(String file, String buildExtensionsDir,
   String framework, String configuration, String runtime, Boolean noBuild, String outputPath, IConsoleHost console) in
   /_/src/NSwag.Commands/Commands/Generation/AspNetCore/ProjectMetadata.cs:line 152
     at NSwag.Commands.Generation.AspNetCore.AspNetCoreToSwaggerCommand.RunAsync(CommandLineProcessor processor, IConso
  leHost host) in /_/src/NSwag.Commands/Commands/Generation/AspNetCore/AspNetCoreToOpenApiCommand.cs:line 106
     at NSwag.Commands.NSwagDocumentBase.GenerateSwaggerDocumentAsync() in /_/src/NSwag.Commands/NSwagDocumentBase.cs:l
  ine 275
     at NSwag.Commands.NSwagDocument.ExecuteAsync() in /_/src/NSwag.Commands/NSwagDocument.cs:line 81
     at NSwag.Commands.Document.ExecuteDocumentCommand.ExecuteDocumentAsync(IConsoleHost host, String filePath) in /_/s
  rc/NSwag.Commands/Commands/Document/ExecuteDocumentCommand.cs:line 85
     at NSwag.Commands.Document.ExecuteDocumentCommand.RunAsync(CommandLineProcessor processor, IConsoleHost host) in /
  _/src/NSwag.Commands/Commands/Document/ExecuteDocumentCommand.cs:line 48
     at NConsole.CommandLineProcessor.ProcessSingleAsync(String[] args, Object input)
     at NConsole.CommandLineProcessor.ProcessAsync(String[] args, Object input)
     at NSwag.Commands.NSwagCommandProcessor.ProcessAsync(String[] args) in /_/src/NSwag.Commands/NSwagCommandProcessor
  .cs:line 61
D:\Code\JasonTaylorDev\DotNet8NswagIssue\WebApi\WebApi.csproj(24,3): error MSB3073: The command "dotnet "C:\Users\jason
\.nuget\packages\nswag.msbuild\13.19.0\buildTransitive\../tools/Net70/dotnet-nswag.dll" run /variables:Configuration=De
bug" exited with code -1.

Build FAILED.

D:\Code\JasonTaylorDev\DotNet8NswagIssue\WebApi\WebApi.csproj : error MSB4057: The target "__GetNSwagProjectMetadata" d
oes not exist in the project.
D:\Code\JasonTaylorDev\DotNet8NswagIssue\WebApi\WebApi.csproj(24,3): error MSB3073: The command "dotnet "C:\Users\jason
\.nuget\packages\nswag.msbuild\13.19.0\buildTransitive\../tools/Net70/dotnet-nswag.dll" run /variables:Configuration=De
bug" exited with code -1.
    0 Warning(s)
    2 Error(s)
```

You can workaround this issue by disabling simplified output paths. To do so:

1. Update **Directory.Build.props** and comment out or remove the following line:

   ```xml
   <ArtifactsPath>$(MSBuildThisFileDirectory)artifacts</ArtifactsPath>
   ```

