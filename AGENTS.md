# Agent Context for ILGPU Repository

This file provides a detailed overview of the repository structure and the most important components. It is meant to give future automated agents a compact summary of how the library is organized.

## Repository Layout

- `Src/` – all source projects
  - `ILGPU/` – core runtime and compiler
  - `ILGPU.Algorithms/` – high level algorithm library
  - `ILGPU.Analyzers/` – Roslyn analyzers and source generators
  - `ILGPU.Tests*` – unit tests for different backends
- `Docs/` – tutorials and documentation
- `Samples/` – example projects demonstrating usage
- `Tools/` – build helpers and style checkers

## Main Library (`Src/ILGPU`)

The `ILGPU` project implements a JIT compiler for GPU kernels and a runtime capable of executing them on multiple accelerator types.

### Context and Device Management

- `Context.cs` – central entry point. Manages all registered devices, compilation phases and transformer passes. Provides enumerators to discover devices and create `Accelerator` instances. Extensions can be attached via `ContextExtension`.
- `Runtime/Device.cs` – base class of all device descriptors. Derived types include `CPUDevice`, `CudaDevice`, `CLDevice` and `VelocityDevice`.
- `Runtime/Accelerator.cs` – abstract accelerator implementation that manages memory buffers, kernel caches and stream scheduling. Concrete accelerators reside in subfolders `CPU`, `Cuda`, `OpenCL` and `Velocity`.

### Frontend

- `Frontend/ILFrontend.cs` – parses .NET IL into the library's intermediate representation (IR). The frontend builds a control flow graph using `Block` and `Block.CFGBuilder`. Debug information helpers live under `Frontend/DebugInformation`.
- `Frontend/Intrinsic` – defines attributes and matching logic for intrinsic methods.

### Intermediate Representation

- `IR/` – defines node types such as `Value`, `BasicBlock` and `Node`. Transformations and analyses live in subfolders `Construction`, `Rewriting`, `Analyses` and `Transforms`. `IRContext` holds type and node caches.

### Backends

- `Backends/IL` – generates MSIL code for CPU execution (`ILBackend`, `DefaultILBackend`). Uses `ILEmitter` to create dynamic methods at runtime.
- `Backends/PTX` – PTX backend for NVIDIA GPUs. Code generation is handled by `PTXCodeGenerator` and related helpers. Register allocation uses `PTXRegisterAllocator`.
- `Backends/OpenCL` – OpenCL kernel generator using similar abstractions (`CLCodeGenerator`).
- `Backends/Velocity` – backend for the SIMD/Velocity accelerator.
- `Backends/EntryPoints` – common logic for launching kernels: `EntryPoint`, `ArgumentMapper` and `KernelSpecialization`.

### Runtime Layer

- `Runtime/AcceleratorStream.cs` – asynchronous command stream used to launch kernels and perform memory operations.
- `Runtime/MemoryBuffer.cs` and `ArrayView.cs` – typed and raw views over accelerator memory. `MemoryBuffer` implementations exist per backend (`CPUMemoryBuffer`, `CudaMemoryBuffer`, etc.).
- `Runtime/Kernels` – dynamic kernel loading and specialization. `Kernel` objects represent compiled entry points.
- `RuntimeAPI.cs` and `RuntimeSystem.cs` – load native driver libraries on demand.

### Utilities

- `Util/` – generic helpers such as `ArrayViewExtensions`, math utilities and collection classes. Many files are generated from `*.tt` templates.
- `Resources/` – string resources and error descriptions.

## Algorithms Library (`Src/ILGPU.Algorithms`)

Provides reusable algorithms that run on all accelerator types.

- `AlgorithmContext.cs` – registers algorithm-specific intrinsics for each backend.
- `ArrayExtensions.cs`, `GridExtensions.cs`, `GroupExtensions.cs`, `WarpExtensions.cs` – extension methods for common operations (copying arrays, thread group communication, warp reductions and scans).
- `HistogramExtensions.cs` – histogram and sorting utilities. Many implementations are generated from `HistogramLaunchers.tt` and other templates.
- `FixedPrecision/` – fixed point integer math types generated from `FixedInts.tt`.

## Analyzers (`Src/ILGPU.Analyzers`)

- `InterleaveFieldsGenerator.cs` – incremental source generator that rewrites struct layouts using the `InterleaveFieldsAttribute` from the `CodeGeneration` folder.
- The `Resources` subdirectory defines diagnostic messages.

## Tests

Unit tests live under `Src/ILGPU.Tests` and backend specific projects (`ILGPU.Tests.CPU`, `ILGPU.Tests.Cuda`, `ILGPU.Tests.OpenCL`). They exercise kernels, algorithms and runtime functionality.

## Samples and Documentation

- `Samples/` contains small applications demonstrating how to use ILGPU, such as vector addition or reduction samples.
- `Docs/` provides detailed tutorials ranging from GPU primers to advanced usage and upgrade guides.

## Tools

`Tools/` hosts build utilities. Examples:
- `CheckStyles` – MSBuild target enforcing code style rules.
- `CudaVersionUpdateTool` – updates bundled CUDA library versions.
- `GenerateCompatibilitySuppressionFiles` – script to generate suppression lists.

## Build Information

The repo targets .NET 8 (see `global.json`). Solution files are located in `Src/ILGPU.sln` and `Tools/Tools.sln`. MSBuild `Directory.Build.props` files configure common settings. Some code is generated via T4 templates and Roslyn generators during build.

## Notes for Agents

ILGPU uses advanced C# features like generics, value-type pointers and dynamic code generation. Most APIs are thread-safe, although runtime objects like `Accelerator` instances are not. When looking for specific logic:

1. Core compiler and runtime live under `Src/ILGPU`.
2. High-level algorithms are implemented in `Src/ILGPU.Algorithms`.
3. Backend-specific runtime code resides in `Src/ILGPU/Runtime` and code generators under `Src/ILGPU/Backends`.
4. Examples and tests show typical usage patterns.
