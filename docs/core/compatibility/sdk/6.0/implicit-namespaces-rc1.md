---
title: "Preview-to-preview breaking change: Implicit `global using` directives disabled for existing projects"
description: Learn about the preview-to-preview breaking change in .NET 6 where the implicit `global using` directives feature is enabled by default only for new C# projects.
ms.date: 09/02/2021
---
# Implicit `global using` directives for new C# projects only

C# 10 introduced [`global using`](../../../../csharp/language-reference/keywords/using-directive.md#global-modifier) directive support for C# projects. In .NET 6 Preview 7, the .NET SDK implicitly added `global using` directives to new and existing .NET projects, by default. In .NET 6 RC 1 and later versions, implicit `global using` directives are only added for new C# projects. They are disabled for existing projects, and some of the associated MSBuild property and item names have changed from Preview 7.

You may need to re-enable the feature or explicitly include namespaces that your project depends on.

## Version introduced

.NET 6 RC 1

## Old behavior

In .NET 6 Preview 7, the .NET SDK implicitly included a set of default namespaces for *new and existing* C# projects that target .NET 6 or later and that use one of the following SDKs:

- Microsoft.NET.Sdk
- Microsoft.NET.Sdk.Web
- Microsoft.NET.Sdk.Worker
- Microsoft.NET.Sdk.WindowsDesktop

In addition, you could:

- Disable the feature globally using the `DisableImplicitNamespaceImports` MSBuild property, or on an SDK-specific basis using the `DisableImplicitNamespaceImports_DotNet`, `DisableImplicitNamespaceImports_Web`, `DisableImplicitNamespaceImports_Worker`, `DisableImplicitNamespaceImports_WindowsForms`, and `DisableImplicitNamespaceImports_WPF` properties.
- Import specific namespaces using the `Import` item type.

## New behavior

Starting in .NET 6 RC 1, no implicit `global using` directives are added when you retarget an *existing* project to .NET 6 or later. (They are still added for new C# projects that target .NET 6.) However, you can enable the feature in your existing C# project by setting the [`ImplicitUsings` MSBuild property](../../../project-sdk/msbuild-props.md#implicitusings) to `true` or `enable`.

In addition, for C# projects:

- The MSBuild property used to enable or disable the feature is renamed to `ImplicitUsings`, and the SDK-specific properties, such as `DisableImplicitNamespaceImports_DotNet`, are no longer valid. The feature is either completely enabled or completely disabled.
- The item type to include a specific namespaces is renamed to `Using`. For Visual Basic projects, you can continue to use the `Import` item type.

If you enable implicit `global using` directives, the .NET SDK adds `global using` directives for a set of default namespaces to a generated file in the project's *obj* directory. The default namespaces are as follows:

| SDK | Default namespaces |
| - | - |
| Microsoft.NET.Sdk | <xref:System><br/><xref:System.Collections.Generic?displayProperty=fullName><br/><xref:System.IO?displayProperty=fullName><br/><xref:System.Linq?displayProperty=fullName><br/><xref:System.Net.Http?displayProperty=fullName><br/><xref:System.Threading?displayProperty=fullName><br/><xref:System.Threading.Tasks?displayProperty=fullName> |
| Microsoft.NET.Sdk.Web | <xref:System.Net.Http.Json?displayProperty=fullName><br/><xref:Microsoft.AspNetCore.Builder?displayProperty=fullName><br/><xref:Microsoft.AspNetCore.Hosting?displayProperty=fullName><br/><xref:Microsoft.AspNetCore.Http?displayProperty=fullName><br/><xref:Microsoft.AspNetCore.Routing?displayProperty=fullName><br/><xref:Microsoft.Extensions.Configuration?displayProperty=fullName><br/><xref:Microsoft.Extensions.DependencyInjection?displayProperty=fullName><br/><xref:Microsoft.Extensions.Hosting?displayProperty=fullName><br/><xref:Microsoft.Extensions.Logging?displayProperty=fullName> |
| Microsoft.NET.Sdk.Worker | <xref:Microsoft.Extensions.Configuration?displayProperty=fullName><br/><xref:Microsoft.Extensions.DependencyInjection?displayProperty=fullName><br/><xref:Microsoft.Extensions.Hosting?displayProperty=fullName><br/><xref:Microsoft.Extensions.Logging?displayProperty=fullName> |
| Microsoft.NET.Sdk.WindowsDesktop (Windows Forms) | Microsoft.NET.Sdk namespaces<br/><xref:System.Drawing?displayProperty=fullName><br/><xref:System.Windows.Forms?displayProperty=fullName> |
| Microsoft.NET.Sdk.WindowsDesktop (WPF) | Microsoft.NET.Sdk namespaces<br/>Removed <xref:System.IO?displayProperty=fullName><br/>Removed <xref:System.Net.Http?displayProperty=fullName> |

## Change category

This change may affect [*source compatibility*](../../categories.md#source-compatibility).

## Reason for change

The Preview 7 breaking change caused a few issues in some important scenarios. We are changing the feature to mitigate the issues and generally improve its usability.

## Recommended action

If you relied on namespaces being implicitly included in your C# project, you can:

- Re-enable implicit `global using` directives by setting the [`ImplicitUsings` MSBuild property](../../../project-sdk/msbuild-props.md#implicitusings) to `true` or `enable`.

- Include the namespaces globally by adding `Using` items to your project file (or renaming your existing `Import` items). For example:

  ```xml
  <ItemGroup>
    <Using Include="System.IO.Pipes" />
  </ItemGroup>
  ```

- Add `using` directives to your source files or `global using` directives to a single source file.

## Affected APIs

N/A

## See also

- [Implicit using directives](../../../project-sdk/overview.md#implicit-using-directives)
