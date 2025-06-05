# Agent Context for Src Directory

This folder contains all source projects for the ILGPU repository. Each subdirectory is a standalone project or set of unit tests. The solution file `ILGPU.sln` references them as needed. Below is a high level overview.

## Projects

- `ILGPU/` – Core runtime and compiler implementation. This project contains the main context classes, device management, the intermediate representation (IR) and backends.
- `ILGPU.Algorithms/` – Collection of accelerator-agnostic algorithms and extension methods. Provides warp and grid helpers, math utilities and fixed precision types.
- `ILGPU.Analyzers/` – Roslyn analyzer package containing the `InterleaveFieldsGenerator` source generator and related diagnostic resources.

## Test Projects

- `ILGPU.Tests*` – Unit tests for the core library across different backends such as CPU, CUDA, OpenCL and Velocity. Each folder hosts a test project targeting the respective accelerator.
- `ILGPU.Algorithms.Tests*` – Unit tests validating the algorithms library on each backend.
- `ILGPU.Analyzers.Tests` – Tests for the Roslyn analyzers.

## Build

The `Directory.Build.props` file defines common MSBuild settings for all projects under this directory.
