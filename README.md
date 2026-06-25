# MetaEngine OpenAPI C# HttpClient

[![NuGet version](https://img.shields.io/nuget/v/MetaEngine.CSharp.OpenApi.HttpClient.Tool.svg)](https://www.nuget.org/packages/MetaEngine.CSharp.OpenApi.HttpClient.Tool/)
[![NuGet downloads](https://img.shields.io/nuget/dt/MetaEngine.CSharp.OpenApi.HttpClient.Tool.svg)](https://www.nuget.org/packages/MetaEngine.CSharp.OpenApi.HttpClient.Tool/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Generate idiomatic C# clients and models from OpenAPI/Swagger specifications.**

`HttpClient`-based services and `System.Text.Json` models — typed, with optional XML docs, DataAnnotations validation, retries, auth, and DI-ready middleware.

> Distributed as a **.NET tool** — `dotnet tool install` and run it from the command line. No project reference, no MSBuild wiring; it uses the .NET SDK you already have.

---

## Quick Links

- **NuGet Package**: [MetaEngine.CSharp.OpenApi.HttpClient.Tool](https://www.nuget.org/packages/MetaEngine.CSharp.OpenApi.HttpClient.Tool)
- **Website**: [metaengine.eu](https://www.metaengine.eu)

---

## Features

- ✅ **Native dotnet tool** - `dotnet tool install` and run; no project reference, no build-time wiring
- ✅ **`HttpClient` services** - One typed service class per OpenAPI tag
- ✅ **`System.Text.Json` models** - Records/classes with typed required vs optional members and standalone enums
- ✅ **XML docs** - Optional `/// <summary>` doc comments from OpenAPI descriptions and examples
- ✅ **DataAnnotations** - Optional `[Required]`, `[StringLength]`, `[Range]`, `[EmailAddress]`, `[RegularExpression]` from spec constraints
- ✅ **DI-ready middleware** - Optional `ApiDelegatingHandler` + `ServiceCollectionExtensions`
- ✅ **Auth & resilience** - Bearer/basic auth, custom headers, retries with exponential backoff
- ✅ **Tag Filtering** - Generate only the operations you need
- ✅ **Options Object** - Collapse long parameter lists into an options object past a threshold

---

## Installation

```bash
# Global (available everywhere)
dotnet tool install -g MetaEngine.CSharp.OpenApi.HttpClient.Tool

# Or pin it to a repo (local tool manifest)
dotnet new tool-manifest        # once per repo
dotnet tool install MetaEngine.CSharp.OpenApi.HttpClient.Tool
```

---

## Requirements

- **.NET SDK 8.0** or later — the same toolchain you use to build C#. The tool runs on any platform .NET supports (Linux, macOS, Windows).

---

## Quick Start

```bash
metaengine-openapi-csharp-httpclient <input> <output> <namespace> [options]
```

### Recommended Setup

```bash
metaengine-openapi-csharp-httpclient api.yaml ./Generated PetStore.Client \
  --documentation \
  --validation-annotations
```

### More Examples

```bash
# From a URL
metaengine-openapi-csharp-httpclient https://api.example.com/openapi.json ./Generated PetStore.Client

# Filter by OpenAPI tags
metaengine-openapi-csharp-httpclient api.yaml ./Generated PetStore.Client --include-tags orders,users

# Resilient client with auth + retries + DI middleware
metaengine-openapi-csharp-httpclient api.yaml ./Generated PetStore.Client \
  --bearer-auth API_TOKEN --retries 3 --middleware
```

---

## CLI Options

| Argument / Option | Description | Default |
|---|---|---|
| `input` *(required)* | OpenAPI specification file path or URL | - |
| `output` *(required)* | Output directory for generated C# files | - |
| `namespace` *(required)* | Root namespace for the generated client and models | - |
| `--include-tags <tags>` | Only generate operations with these tags (comma-separated) | - |
| `--service-suffix <name>` | Service class naming suffix (e.g. `Client`, `Api`) | - |
| `--options-threshold <n>` | Parameter count at which a method switches to an options object | `4` |
| `--documentation` | Generate XML doc comments from schema descriptions and examples | `false` |
| `--validation-annotations` | Emit `System.ComponentModel.DataAnnotations` attributes on models | `false` |
| `--middleware` | Generate middleware infrastructure (`ApiDelegatingHandler` + `ServiceCollectionExtensions`) | `false` |
| `--error-handling` | Enable smart error handling based on HTTP status semantics | `false` |
| `--bearer-auth [env-var]` | Bearer token from an env var (default `API_TOKEN`); adds an `Authorization` header | - |
| `--basic-auth` | Basic auth from env vars (`API_USERNAME` / `API_PASSWORD`) | `false` |
| `--retries [max-attempts]` | Enable retries with exponential backoff; optional value sets max attempts | - |
| `--custom-header <name=env-var>` | Static header from an env var. Repeatable | - |
| `--strict-validation` | Enable strict OpenAPI validation | `false` |
| `--clean` | Clean the destination directory before generating | `false` |
| `--verbose` | Enable verbose logging | `false` |

---

## Generated Code Structure

```
output/
  ├── Models/                    # One file per schema (System.Text.Json)
  │   ├── Order.cs               # public class Order { ... }
  │   ├── OrderStatus.cs         # standalone enum type
  │   └── ...
  └── Client/                    # One HttpClient service per tag
      ├── OrdersClient.cs        # all order operations
      └── ...
```

---

## Support

- **Issues**: [GitHub Issues](https://github.com/meta-engine/openapi-csharp-httpclient/issues)
- **Email**: info@metaengine.eu
- **Website**: [metaengine.eu](https://www.metaengine.eu)

---

## License

MIT License - see [LICENSE](./LICENSE) file for details.

---

## About This Repository

This is the **documentation and issue tracking repository** for MetaEngine OpenAPI C# HttpClient. The package is published to [NuGet.org](https://www.nuget.org/packages/MetaEngine.CSharp.OpenApi.HttpClient.Tool).

Source code is proprietary, but the package is free to use under the MIT license.
