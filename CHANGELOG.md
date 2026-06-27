# Changelog

See the [NuGet versions page](https://www.nuget.org/packages/MetaEngine.CSharp.OpenApi.HttpClient.Tool#versions-body-tab) for the full version history.

## 1.0.1

- **`--options-threshold` now changes the generated code.** Once an operation reaches the
  threshold parameter count (default `4`), its query and header parameters are grouped into a
  single options object instead of separate method arguments. In `1.0.0` the flag was accepted
  but had no effect on the generated output.

## 1.0.0

- Initial release — C# code generation from OpenAPI specifications, distributed as a dotnet tool
- **`HttpClient` services** — one typed service class per OpenAPI tag
- **`System.Text.Json` models** — typed required/optional members, standalone enum types
- `--documentation` — XML doc comments from OpenAPI descriptions and examples
- `--validation-annotations` — `System.ComponentModel.DataAnnotations` attributes from spec constraints
- `--middleware` — `ApiDelegatingHandler` + `ServiceCollectionExtensions` for DI
- `--bearer-auth`, `--basic-auth`, `--custom-header`, `--retries`, `--error-handling`
- `--include-tags`, `--service-suffix`, `--options-threshold`, `--strict-validation`, `--clean`, `--verbose`
