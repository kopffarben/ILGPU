# Agent Context for Src/ILGPU.Analyzers

Contains Roslyn analyzers and source generators used by ILGPU projects.

## Main Components

- `InterleaveFieldsGenerator.cs` – Incremental source generator rewriting struct layouts when marked with `InterleaveFieldsAttribute`.
- `SymbolExtensions.cs` – Helper methods for Roslyn symbol analysis.
- `Resources/` – Diagnostic descriptors and localized strings.
- `Tools/` – MSBuild tasks packaging the analyzer for NuGet.
- `AnalyzerReleases.*.md` – Logs of analyzer versions shipped.
