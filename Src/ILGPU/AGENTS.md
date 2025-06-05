# Agent Context for Src/ILGPU

This project implements the core compiler and runtime of ILGPU. Below is a brief description of the most important folders and files.

## Top-Level Files

- `Context.cs` and `Context.Builder.cs` – central entry points providing device enumeration, compilation pipelines and code generation phases. Extensions derive from `ContextExtension`.
- `ArrayView.cs`, `BasicValueType.cs` – foundational types for working with accelerator memory.
- `Atomic.cs` and `AtomicFunctions.tt` – atomic operations and template for generating backend-specific implementations.
- `CapabilityNotSupportedException.cs` – custom exception for unsupported features.

## Subdirectories

### Backends
Contains code generators for each backend:
- `IL` – Generates MSIL for CPU execution.
- `PTX` – NVIDIA PTX backend with register allocation and debug info support.
- `OpenCL` – OpenCL kernel generator.
- `Velocity` – SIMD backend for Velocity vectors.
- `EntryPoints` – shared logic to specialize kernels and map arguments.

### CodeGeneration
Common infrastructure such as instruction builders, basic block emitters and debug info helpers.

### Frontend
Parses .NET IL into the IR. Includes support for debug information and intrinsic mapping.

### IR
Defines intermediate representation nodes (`Value`, `BasicBlock`, `TypeNode`) and transformation passes located under `Analyses`, `Construction`, `Rewriting` and `Transforms`.

### Runtime
Abstract accelerator and memory implementations. Backend-specific accelerators live in `CPU/`, `Cuda/`, `OpenCL/` and `Velocity/`.
Key classes include `Accelerator`, `AcceleratorStream`, `MemoryBuffer` and `Kernel`.

### Util
Utility helpers like array extension methods and math functions. Several `.tt` templates generate specialized code.

## Resources
The `Resources` folder contains string resources for errors and diagnostics.
