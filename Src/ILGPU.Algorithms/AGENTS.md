# Agent Context for Src/ILGPU.Algorithms

This project hosts reusable algorithms that run on any ILGPU accelerator. It extends the core runtime with high level operations and math helpers.

## Key Files

- `AlgorithmContext.cs` – Registers algorithm-specific intrinsics with each backend during startup.
- `ArrayExtensions.cs` – Methods for copying, initializing and transforming arrays across accelerators.
- `GridExtensions.cs`, `GroupExtensions.cs`, `WarpExtensions.cs` – Provide common patterns for thread groups and warps including reductions and broadcasts.
- `ConcurrentStreamProcessor.cs` – Utility to manage multiple streams for asynchronous work.
- `FixedPrecision/` – Contains generated fixed-point integer types from `FixedInts.tt`.
- `HistogramExtensions.cs` – Histogram implementations generated from template launchers.

## OpenCL Helpers

The `CL` folder contains wrappers for calling external OpenCL libraries (cuBLAS, cuFFT, etc.) when available.
