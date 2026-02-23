# Awesome Client-Side GPU [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

> A curated list of GPU-accelerated cryptography and zero-knowledge proof projects that run on **user devices** — browsers, mobile phones, and desktops.

> **"Client-side"** means the computation runs on the user's own hardware, not on a remote server or cloud cluster. This includes WebGPU in a browser, Metal on an iPhone, and Vulkan on a gaming laptop.

## Contents

- [What Is Client-Side GPU Proving?](#what-is-client-side-gpu-proving)
- [GPU API Quick-Reference](#gpu-api-quick-reference)
- [Projects by GPU Technology](#projects-by-gpu-technology)
  - [WebGPU (Browser-Native)](#webgpu-browser-native)
  - [Metal (Apple Silicon / iOS / macOS)](#metal-apple-silicon--ios--macos)
  - [Vulkan / ROCm / HIP (Cross-Platform Native)](#vulkan--rocm--hip-cross-platform-native)
  - [Cross-Platform Frameworks](#cross-platform-frameworks)
- [Projects by Cryptographic Operation](#projects-by-cryptographic-operation)
  - [Multi-Scalar Multiplication (MSM)](#multi-scalar-multiplication-msm)
  - [Number Theoretic Transform (NTT / FFT)](#number-theoretic-transform-ntt--fft)
  - [Field Arithmetic](#field-arithmetic)
  - [Hash Functions](#hash-functions)
  - [Full Proof Systems](#full-proof-systems)
- [Mobile and Edge Proving](#mobile-and-edge-proving)
- [Competitions and Benchmarks](#competitions-and-benchmarks)
  - [ZPrize Entries](#zprize-entries)
  - [Benchmarks and Metrics](#benchmarks-and-metrics)
- [Learning Resources](#learning-resources)
  - [Blog Posts and Articles](#blog-posts-and-articles)
  - [Research Papers](#research-papers)
- [Related Lists](#related-lists)

---

## What Is Client-Side GPU Proving?

Zero-knowledge proof systems require heavy arithmetic over finite fields and elliptic curves — operations that map naturally onto GPU parallelism. Traditionally, GPU acceleration was the domain of data-centre CUDA clusters. **Client-side GPU proving** brings the same speedups to ordinary user hardware: a browser tab, a smartphone, a laptop. This enables privacy-preserving applications that do not need to trust a remote server with sensitive inputs.

The two most impactful operations targeted by this list are:

- **MSM (Multi-Scalar Multiplication):** the dominant cost in pairing-based proof systems (Groth16, PLONK, etc.).
- **NTT (Number Theoretic Transform):** the core of STARK and FRI-based systems.

Both can achieve 10–100× speedups over CPU implementations when vectorised on modern GPU hardware.

## GPU API Quick-Reference

| API | Platforms | Primary Languages | Browser Support | Notes |
|-----|-----------|-------------------|-----------------|-------|
| **WebGPU** | Browser, native (via `wgpu`) | WGSL, Rust, JS/TS | Chrome 113+, Firefox (flag), Safari TP | W3C standard; replaces WebGL for compute |
| **Metal** | iOS, macOS, Apple Silicon | MSL, Rust (metal-rs), Swift | No | 2–3× faster than OpenGL on Apple hardware |
| **Vulkan Compute** | Windows, Linux, Android | GLSL, SPIR-V, Rust | No | Cross-vendor; lowest-level portable API |
| **ROCm / HIP** | AMD GPUs (Linux) | HIP C++ | No | CUDA-compatible syntax; growing mobile support |
| **CUDA** | NVIDIA GPUs | CUDA C++ | No | Highest throughput; server/desktop only |
| **OpenCL** | Most GPUs | OpenCL C | No | Legacy; being superseded by Vulkan/WebGPU |

## Projects by GPU Technology

### WebGPU (Browser-Native)

- [ICME-Lab/msm-webgpu](https://github.com/ICME-Lab/msm-webgpu) - WebGPU-accelerated MSM over BN254 using the cuZK algorithm, implemented in Rust and compiled to WGSL shaders. `GPU: WebGPU` `Curve: BN254` `Op: MSM` `Lang: Rust`

- [td-kwj-zp2023/webgpu-msm-bls12-377](https://github.com/td-kwj-zp2023/webgpu-msm-bls12-377) - Winning ZPrize 2023 entry for WebGPU MSM over BLS12-377, achieving top performance in the browser-native competition. `GPU: WebGPU` `Curve: BLS12-377` `Op: MSM` `Lang: TypeScript`

- [td-kwj-zp2023/webgpu-msm-twisted-edwards](https://github.com/td-kwj-zp2023/webgpu-msm-twisted-edwards) - ZPrize 2023 WebGPU MSM entry targeting Twisted Edwards curves, demonstrating efficient extended-coordinates arithmetic in WGSL. `GPU: WebGPU` `Curve: BLS12-377` `Op: MSM` `Lang: TypeScript`

- [demox-labs/webgpu-crypto](https://github.com/demox-labs/webgpu-crypto) - Research-stage WebGPU library covering MSM, NTT, and hash functions over BLS12-377 and BN254. `GPU: WebGPU` `Curve: BLS12-377` `Curve: BN254` `Op: MSM` `Op: NTT` `Op: Hash` `Lang: TypeScript`

- [demox-labs/webgpu-msm](https://github.com/demox-labs/webgpu-msm) - Baseline ZPrize 2023 reference framework for browser-native MSM over BLS12-377, used as a starting point by competition entrants. `GPU: WebGPU` `Curve: BLS12-377` `Op: MSM` `Lang: TypeScript`

- [penumbra-zone/webgpu](https://github.com/penumbra-zone/webgpu) - Pioneer WebGPU Groth16 prover for Penumbra's shielded transactions over BLS12-377; archived after proving the concept. `GPU: WebGPU` `Curve: BLS12-377` `Op: Groth16` `Lang: Rust` `Status: Archived`

### Metal (Apple Silicon / iOS / macOS)

- [zkmopro/gpu-acceleration](https://github.com/zkmopro/gpu-acceleration) - Metal MSM library for Apple Silicon achieving 40–100× speedup over ARM CPU, integrated into the mopro mobile proving toolkit. `GPU: Metal` `Curve: BN254` `Op: MSM` `Lang: Rust`

- [geometryxyz/msl-secp256k1](https://github.com/geometryxyz/msl-secp256k1) - Metal Shading Language implementation of secp256k1 elliptic-curve arithmetic using Jacobian coordinates, targeting Apple Silicon GPUs. `GPU: Metal` `Curve: secp256k1` `Op: EC Arithmetic` `Lang: MSL`

### Vulkan / ROCm / HIP (Cross-Platform Native)

<!-- Entries added in Phase 3 -->

### Cross-Platform Frameworks

<!-- Entries added in Phase 3 -->

## Projects by Cryptographic Operation

### Multi-Scalar Multiplication (MSM)

<!-- Cross-references added in Phase 4 -->

### Number Theoretic Transform (NTT / FFT)

<!-- Cross-references added in Phase 4 -->

### Field Arithmetic

<!-- Cross-references added in Phase 4 -->

### Hash Functions

<!-- Cross-references added in Phase 4 -->

### Full Proof Systems

<!-- Cross-references added in Phase 4 -->

## Mobile and Edge Proving

<!-- Entries added in Phase 3 -->

## Competitions and Benchmarks

### ZPrize Entries

<!-- Entries added in Phase 4 -->

### Benchmarks and Metrics

<!-- Entries added in Phase 4 -->

## Learning Resources

### Blog Posts and Articles

<!-- Entries added in Phase 4 -->

### Research Papers

<!-- Entries added in Phase 4 -->

## Related Lists

<!-- Entries added in Phase 4 -->

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for the scope rules, tag vocabulary, and submission checklist.
