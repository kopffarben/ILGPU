# Agent Context for Samples

This folder contains a large collection of example projects demonstrating ILGPU features. Each subdirectory is a standalone .NET project with a `Program.cs` entry point.

Notable samples:
- `AdjustableSharedMemory/` – Shows how to allocate dynamic shared memory.
- `AdvancedAtomics/` – Demonstrates advanced atomic operations.
- `Algorithms*` – Examples using the algorithm library for groups, histograms and math routines.
- `SimpleMath/`, `VectorAddition/` – Basic kernels for beginners.

Most samples build against the `ILGPU` and `ILGPU.Algorithms` packages and can be run on any backend if supported.
