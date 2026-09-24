# Changelog

See the [NuGet versions page](https://www.nuget.org/packages/MetaEngine.CSharp.OpenApi.HttpClient.Tool#versions-body-tab) for the full version history.

## 1.0.3

### Bug Fixes

- **Primitive schema references preserve JSON member names.** Properties that reference primitive schemas now carry `JsonPropertyName`, including required properties and names containing underscores or hyphens. Valid JSON payloads deserialize and round-trip with the default `System.Text.Json` settings.
- **`--middleware` rejects missing base-address configuration.** Generated client registrations now use the configured `HttpClient.BaseAddress` and report a missing address when the client is constructed.

## 1.0.2

### Features

- **Spec defaults become C# default arguments.** A required parameter that carries a schema `default` now surfaces it as a method default argument — e.g. `FindPetsByStatusAsync(PetStatus status = PetStatus.Available, ...)` — so callers can omit it. Optional parameters keep their `Type? x = null` omit-from-wire semantics.

### Bug Fixes

- **`oneOf`/`anyOf` unions now deserialize at runtime.** Undiscriminated unions — the Stripe-style expandable pattern (`anyOf [string, $ref, ...]`), object-vs-object `oneOf` with no discriminator, and `oneOf` over string enums — previously generated a bare marker interface with no converter, so any response carrying such a field threw at deserialization. Each now generates a `System.Text.Json` converter (a synthesized string variant for the string arm, try-each object dispatch for the object arms).
- **Overlapping union variants resolve to the right type.** When two object members of a union shared required-property sets, the converter returned the first match and could silently deserialize to the wrong variant. It now discriminates on the distinguishing property set.
- **`--error-handling` keeps the success check for uncategorized status codes.** With error handling enabled, status codes that weren't explicitly categorized (400, 409, 422, 429 after retries, ...) fell through and the error body was deserialized as if it were a success payload. Those codes now surface as errors.
- **`--documentation` comments bind to their member.** A property carrying both a doc comment and an attribute (e.g. with `--validation-annotations`) emitted the `/// <summary>` between the attribute and the property, leaving it orphaned. The doc comment now renders above the attributes.
- **Consistent indentation in generated client methods.** Some service method bodies and brace-less `if` continuations were dedented; all generated client code is now uniformly indented.

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
