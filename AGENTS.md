ILGPU Coding Conventions and Internal Architecture

## Coding conventions
- The project uses `.editorconfig` files. Global settings use spaces, indent size 4.
- Line endings are CRLF, with trailing whitespace trimmed.
- Files under `Src` enforce a maximum line length of 90 characters.
- C# code uses expression-bodied members, prefers `var` except for built-in types, and requires braces for multiline blocks.
- Interfaces must begin with `I`.
- Analyzer warnings for unused parameters and other style rules are mostly disabled or set to low severity.


## Internals of ILGPU
ILGPU is a JIT compiler for .NET languages that targets GPUs. It builds kernels using a parallel transformation and compilation model.
Optimization passes are controlled via `OptimizationLevel` in `Context.Builder`; `O2` performs extra transformations for best performance.

Internal caches like `KernelCache`, `MemoryBufferCache`, and accelerator-specific caches speed up compilation and reuse resources. Use `Context.ClearCache` and `Accelerator.ClearCache` to release them when needed.

Target-specific `Backend` instances generate code for each accelerator. The `IRContext` holds intermediate representations and can be shared across backends. You normally interact with higher-level runtime APIs, which compile and load kernels for an accelerator.

Kernels can also be compiled manually via a backend to get a `CompiledKernel`. Accelerators load these kernels using functions such as `LoadAutoGroupedKernel`. Launching a kernel via `Launch` boxes arguments; performance-critical code should use typed kernel launcher delegates created with `CreateLauncherDelegate` or high-level methods like `LoadAutoGroupedStreamKernel`.
