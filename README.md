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

---

## What Is Client-Side GPU Proving

Zero-knowledge proof systems require heavy arithmetic over finite fields and elliptic curves — operations that map naturally onto GPU parallelism. Traditionally, GPU acceleration was the domain of data-centre CUDA clusters. **Client-side GPU proving** brings the same speedups to ordinary user hardware: a browser tab, a smartphone, a laptop. This enables privacy-preserving applications that do not need to trust a remote server with sensitive inputs.

The two most impactful operations targeted by this list are:

<!--lint disable awesome-list-item-->
- **MSM (Multi-Scalar Multiplication):** the dominant cost in pairing-based proof systems (Groth16, PLONK, etc.).
- **NTT (Number Theoretic Transform):** the core of STARK and FRI-based systems.
<!--lint enable awesome-list-item-->

Both can achieve 10–100× speedups over CPU implementations when vectorised on modern GPU hardware.

## GPU API Quick-Reference

<!--lint disable table-pipe-alignment table-cell-padding-->
| API | Platforms | Primary Languages | Browser Support | Notes |
|-----|-----------|-------------------|-----------------|-------|
| **WebGPU** | Browser, native (via `wgpu`) | WGSL, Rust, JS/TS | Chrome 113+, Firefox (flag), Safari TP | W3C standard; replaces WebGL for compute |
| **Metal** | iOS, macOS, Apple Silicon | MSL, Rust (metal-rs), Swift | No | 2–3× faster than OpenGL on Apple hardware |
| **Vulkan Compute** | Windows, Linux, Android | GLSL, SPIR-V, Rust | No | Cross-vendor; lowest-level portable API |
| **ROCm / HIP** | AMD GPUs (Linux) | HIP C++ | No | CUDA-compatible syntax; growing mobile support |
| **CUDA** | NVIDIA GPUs | CUDA C++ | No | Highest throughput; server/desktop only |
| **OpenCL** | Most GPUs | OpenCL C | No | Legacy; being superseded by Vulkan/WebGPU |
<!--lint enable table-pipe-alignment table-cell-padding-->

## Projects by GPU Technology

### WebGPU (Browser-Native)

- [ICME-Lab/msm-webgpu](https://github.com/ICME-Lab/msm-webgpu) - WebGPU-accelerated MSM over BN254 using the cuZK algorithm, implemented in Rust and compiled to WGSL shaders. `GPU: WebGPU` `Curve: BN254` `Op: MSM` `Lang: Rust`.

- [td-kwj-zp2023/webgpu-msm-bls12-377](https://github.com/td-kwj-zp2023/webgpu-msm-bls12-377) - Winning ZPrize 2023 entry for WebGPU MSM over BLS12-377, achieving top performance in the browser-native competition. `GPU: WebGPU` `Curve: BLS12-377` `Op: MSM` `Lang: TypeScript`.

- [td-kwj-zp2023/webgpu-msm-twisted-edwards](https://github.com/td-kwj-zp2023/webgpu-msm-twisted-edwards) - ZPrize 2023 WebGPU MSM entry targeting Twisted Edwards curves, demonstrating efficient extended-coordinates arithmetic in WGSL. `GPU: WebGPU` `Curve: BLS12-377` `Op: MSM` `Lang: TypeScript`.

- [demox-labs/webgpu-crypto](https://github.com/demox-labs/webgpu-crypto) - Research-stage WebGPU library covering MSM, NTT, and hash functions over BLS12-377 and BN254. `GPU: WebGPU` `Curve: BLS12-377` `Curve: BN254` `Op: MSM` `Op: NTT` `Op: Hash` `Lang: TypeScript`.

- [demox-labs/webgpu-msm](https://github.com/demox-labs/webgpu-msm) - Baseline ZPrize 2023 reference framework for browser-native MSM over BLS12-377, used as a starting point by competition entrants. `GPU: WebGPU` `Curve: BLS12-377` `Op: MSM` `Lang: TypeScript`.

<!--lint disable awesome-spell-check-->
- [penumbra-zone/webgpu](https://github.com/penumbra-zone/webgpu) - Pioneer WebGPU Groth16 prover for Penumbra's shielded transactions over BLS12-377; archived after proving the concept. `GPU: WebGPU` `Curve: BLS12-377` `Op: Groth16` `Lang: Rust` `Status: Archived`.
<!--lint enable awesome-spell-check-->

### Metal (Apple Silicon / iOS / macOS)

- [zkmopro/gpu-acceleration](https://github.com/zkmopro/gpu-acceleration) - Metal MSM library for Apple Silicon achieving 40–100× speedup over ARM CPU, integrated into the mopro mobile proving toolkit. `GPU: Metal` `Curve: BN254` `Op: MSM` `Lang: Rust`.

- [geometryxyz/msl-secp256k1](https://github.com/geometryxyz/msl-secp256k1) - Metal Shading Language implementation of secp256k1 elliptic-curve arithmetic using Jacobian coordinates, targeting Apple Silicon GPUs. `GPU: Metal` `Curve: secp256k1` `Op: EC Arithmetic` `Lang: MSL`.

### Vulkan / ROCm / HIP (Cross-Platform Native)

See the Cross-Platform Frameworks section below — the libraries below all support Vulkan/ROCm as one of their backends.

### Cross-Platform Frameworks

- [ingonyama-zk/icicle](https://github.com/ingonyama-zk/icicle) - Production-ready ZK acceleration library supporting CUDA, Metal, and ROCm backends, with iOS and Android support for mobile proving. `GPU: Multi` `Op: MSM` `Op: NTT` `Lang: C++` `Lang: Rust`.

- [tracel-ai/cubecl](https://github.com/tracel-ai/cubecl) - Unified GPU compute framework for Rust using a proc-macro kernel language that compiles to WebGPU, CUDA, ROCm, and Metal without per-backend rewrites. `GPU: Multi` `Op: Field Arithmetic` `Lang: Rust`.

- [lambdaclass/lambdaworks](https://github.com/lambdaclass/lambdaworks) - Full ZK cryptography library with GPU-accelerated FFT, MSM, and Merkle tree backends for CUDA and Metal. `GPU: Multi` `Op: FFT` `Op: MSM` `Lang: Rust`.

- [mratsim/constantine](https://github.com/mratsim/constantine) - High-performance elliptic-curve and pairing library in Nim with an experimental WebGPU backend and CUDA support via a runtime JIT compiler. `GPU: Multi` `Op: EC Arithmetic` `Op: MSM` `Lang: Nim`.

- [spaceandtimefdn/blitzar](https://github.com/spaceandtimefdn/blitzar) - C++ MSM and Pedersen commitment library with CUDA and CPU backends and published Rust bindings, targeting desktop and server environments. `GPU: CUDA` `Op: MSM` `Lang: C++` `Lang: Rust`.

## Projects by Cryptographic Operation

### Multi-Scalar Multiplication (MSM)

*Primary entries are in the GPU Technology sections above.*

<!--lint disable table-pipe-alignment table-cell-padding-->
| Project | GPU | Curve |
|---------|-----|-------|
| ICME-Lab/msm-webgpu | WebGPU | BN254 |
| td-kwj-zp2023/webgpu-msm-bls12-377 | WebGPU | BLS12-377 |
| td-kwj-zp2023/webgpu-msm-twisted-edwards | WebGPU | BLS12-377 (Twisted Edwards) |
| demox-labs/webgpu-msm | WebGPU | BLS12-377 |
| zkmopro/gpu-acceleration | Metal | BN254 |
| ingonyama-zk/icicle | Multi | Multi-curve |
| lambdaclass/lambdaworks | Multi | Multi-curve |
| spaceandtimefdn/blitzar | CUDA | Multi-curve |
<!--lint enable table-pipe-alignment table-cell-padding-->

### Number Theoretic Transform (NTT / FFT)

*Primary entries are in the GPU Technology sections above.*

<!--lint disable table-pipe-alignment table-cell-padding-->
| Project | GPU | Notes |
|---------|-----|-------|
| demox-labs/webgpu-crypto | WebGPU | Research-stage |
| ingonyama-zk/icicle | Multi | Production-ready |
| lambdaclass/lambdaworks | Multi | Full ZK library |
<!--lint enable table-pipe-alignment table-cell-padding-->

### Field Arithmetic

<!--lint disable table-pipe-alignment table-cell-padding-->
| Project | GPU | Notes |
|---------|-----|-------|
| tracel-ai/cubecl | Multi | Kernel language targets any backend |
| mratsim/constantine | Multi | Nim runtime JIT |
<!--lint enable table-pipe-alignment table-cell-padding-->

### Hash Functions

<!--lint disable table-pipe-alignment table-cell-padding-->
| Project | GPU | Notes |
|---------|-----|-------|
| demox-labs/webgpu-crypto | WebGPU | Poseidon, Keccak research |
<!--lint enable table-pipe-alignment table-cell-padding-->

### Full Proof Systems

<!--lint disable table-pipe-alignment table-cell-padding-->
| Project | GPU | System | Notes |
|---------|-----|--------|-------|
| penumbra-zone/webgpu | WebGPU | Groth16 | Archived pioneer |
| zkonduit/ezkl | Metal | PLONK / ZKML | iOS/macOS |
| zkmopro/mopro | Metal | Multi-prover | iOS mobile toolkit |
<!--lint enable table-pipe-alignment table-cell-padding-->

## Mobile and Edge Proving

- [zkmopro/mopro](https://github.com/zkmopro/mopro) - Multi-prover mobile toolkit for iOS using Metal GPU acceleration, exposing a unified Swift/Kotlin API over Halo2, Noir, and Circom backends. `GPU: Metal` `Op: Groth16` `Op: PLONK` `Lang: Rust`.

- [zkonduit/ezkl](https://github.com/zkonduit/ezkl) - ZK machine-learning inference library with Metal GPU support on iOS and macOS, delivering approximately 2× speedup over the CPU path. `GPU: Metal` `Op: ZKML` `Op: PLONK` `Lang: Rust`.

## Competitions and Benchmarks

### ZPrize Entries

[ZPrize](https://www.zprize.io) is an open competition that accelerates ZK hardware and software.
The 2023 WebGPU track (Prize #2) produced the fastest known browser-native MSM implementations;
the top-placing entries — `td-kwj-zp2023/webgpu-msm-bls12-377` (BLS12-377 MSM),
`td-kwj-zp2023/webgpu-msm-twisted-edwards` (Twisted Edwards MSM), and the official baseline
`demox-labs/webgpu-msm` — are listed in the WebGPU section above.

### Benchmarks and Metrics

- [moven0831/field-ops-benchmarks](https://github.com/moven0831/field-ops-benchmarks) - Comparative benchmarks for M31 and BN254 field operations across Metal and WebGPU backends, useful for selecting the right field for mobile proving. `GPU: Metal` `GPU: WebGPU` `Op: Field Arithmetic`.

- [ethproofs.org — client-side proving dashboard](https://ethproofs.org) - Live leaderboard tracking proof generation times and costs for client-side Ethereum proof systems across various hardware targets.

## Learning Resources

### Blog Posts and Articles

- [Mopro: "Metal MSM v2"](https://zkmopro.org/blog/metal-msm-v2) - Technical walkthrough of the second-generation Metal MSM implementation in zkmopro/gpu-acceleration, explaining the 40–100× CPU speedup.

- [zkSecurity: "Accelerating ZK Proving with WebGPU"](https://www.zksecurity.xyz/blog/posts/webgpu) - Explores how WebGPU NTT shaders accelerate Stwo STARK proving in the browser.

- [Geometry Research: "Accelerating Client-Side ZK with WebGPU"](https://geometryresearch.xyz/notebook/accelerating-client-side-zk-with-webgpu) - Introduces the MSL secp256k1 work and frames the broader opportunity for WebGPU in client-side ZK proving.

### Research Papers

- [cuZK: Accelerating Zero-Knowledge Proof with a Faster Parallel Multi-Scalar Multiplication Algorithm on GPUs](https://eprint.iacr.org/2022/1321) - IACR 2022/1321; introduces the cuZK MSM algorithm that underpins most WebGPU and Metal MSM implementations in this list. `Op: MSM`

## Related Lists

### WebGPU and GPU Compute

- [mikbry/awesome-webgpu](https://github.com/mikbry/awesome-webgpu) - Comprehensive WebGPU ecosystem list covering specs, demos, shader libraries, and tutorials.

- [rofrol/awesome-wgpu](https://github.com/rofrol/awesome-wgpu) - Curated resources for `wgpu`, the primary Rust WebGPU implementation used by many projects in this list.

- [jslee02/awesome-gpgpu](https://github.com/jslee02/awesome-gpgpu) - General-purpose GPU compute frameworks including CUDA, OpenCL, and Vulkan Compute.

- [goabiaryan/awesome-gpu-engineering](https://github.com/goabiaryan/awesome-gpu-engineering) - GPU architecture, optimization techniques, and Metal Performance Shaders resources.

### Zero-Knowledge Proofs

- [matter-labs/awesome-zero-knowledge-proofs](https://github.com/matter-labs/awesome-zero-knowledge-proofs) - Comprehensive ZKP educational resources covering theory, tools, and applications.

- [ventali/awesome-zk](https://github.com/ventali/awesome-zk) - ZK resources spanning zkVMs, L1/L2 protocols, developer tools, and benchmarks.

- [odradev/awesome-zero-knowledge](https://github.com/odradev/awesome-zero-knowledge) - Blockchain-focused ZK resources with a companion weekly newsletter at [zknewsletter.com](https://zknewsletter.com).

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for the scope rules, tag vocabulary, and submission checklist.
