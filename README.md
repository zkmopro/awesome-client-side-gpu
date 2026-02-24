# Awesome Client-Side GPU [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

> GPU-accelerated cryptography and zero-knowledge proof projects that run on **user devices** — browsers, phones, and desktops — not remote servers.

## Contents

<!--lint disable double-link-->
- [What Is Client-Side GPU Proving](#what-is-client-side-gpu-proving)
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
<!--lint enable double-link-->

---

## What Is Client-Side GPU Proving

Zero-knowledge proofs require heavy finite-field and elliptic-curve arithmetic that maps
naturally onto GPU parallelism. **Client-side GPU proving** brings these speedups to
ordinary user hardware — a browser tab, a smartphone, a laptop — so privacy-preserving
applications never need to send sensitive inputs to a remote server.

Six classes of operation benefit most from GPU acceleration:

<!--lint disable awesome-list-item-->
- **MSM (Multi-Scalar Multiplication)** — dominant cost in pairing-based systems (Groth16, PLONK).
- **NTT (Number Theoretic Transform)** — core of STARK and FRI-based systems.
- **Polynomial evaluation and matrix-vector multiplication** — high-throughput linear algebra.
- **Merkle tree commitments** — batch hashing for hash-based proof schemes.
- **Sumcheck protocol** — multivariate polynomial reductions (GKR, WHIR).
- **Linear encoding and hashing** — used in Basefold and Brakedown commitment schemes.
<!--lint enable awesome-list-item-->

MSM and NTT yield 10–100× speedups over CPU on modern GPU hardware and are the primary
focus of most projects in this list.

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

- [ICME-Lab/msm-webgpu](https://github.com/ICME-Lab/msm-webgpu) - MSM over BN254 using the cuZK algorithm, compiled to WGSL shaders. `GPU: WebGPU` `Curve: BN254` `Op: MSM` `Lang: Rust`.

- [td-kwj-zp2023/webgpu-msm-bls12-377](https://github.com/td-kwj-zp2023/webgpu-msm-bls12-377) - Winning ZPrize 2023 entry for MSM over BLS12-377. `GPU: WebGPU` `Curve: BLS12-377` `Op: MSM` `Lang: TypeScript`.

- [td-kwj-zp2023/webgpu-msm-twisted-edwards](https://github.com/td-kwj-zp2023/webgpu-msm-twisted-edwards) - ZPrize 2023 MSM entry targeting Twisted Edwards curves with extended-coordinates arithmetic in WGSL. `GPU: WebGPU` `Curve: BLS12-377` `Op: MSM` `Lang: TypeScript`.

- [demox-labs/webgpu-crypto](https://github.com/demox-labs/webgpu-crypto) - Research-stage library covering MSM, NTT, and hash functions over BLS12-377 and BN254. `GPU: WebGPU` `Curve: BLS12-377, BN254` `Op: MSM, NTT, Hash` `Lang: TypeScript`.

- [demox-labs/webgpu-msm](https://github.com/demox-labs/webgpu-msm) - Baseline ZPrize 2023 reference for browser-native MSM over BLS12-377. `GPU: WebGPU` `Curve: BLS12-377` `Op: MSM` `Lang: TypeScript`.

<!--lint disable awesome-spell-check-->
- [penumbra-zone/webgpu](https://github.com/penumbra-zone/webgpu) - Pioneer Groth16 prover for Penumbra's shielded transactions over BLS12-377. `GPU: WebGPU` `Curve: BLS12-377` `Op: Groth16` `Lang: Rust` `Status: Archived`.
<!--lint enable awesome-spell-check-->

- [ligeroinc/ligero-prover](https://github.com/ligeroinc/ligero-prover) - Hash-based ZK prover using FFT-based polynomial IOPs, running in browsers via Dawn/Emscripten. `GPU: WebGPU` `Op: NTT, Hash` `Lang: C++`.

### Metal (Apple Silicon / iOS / macOS)

- [zkmopro/gpu-acceleration](https://github.com/zkmopro/gpu-acceleration) - Metal MSM library for Apple Silicon achieving 40–100× speedup over CPU, integrated into the mopro toolkit. `GPU: Metal` `Curve: BN254` `Op: MSM` `Lang: Rust`.

- [geometryxyz/msl-secp256k1](https://github.com/geometryxyz/msl-secp256k1) - Metal Shading Language implementation of secp256k1 elliptic-curve arithmetic using Jacobian coordinates, targeting Apple Silicon GPUs. `GPU: Metal` `Curve: secp256k1` `Op: EC Arithmetic` `Lang: MSL`.

### Vulkan / ROCm / HIP (Cross-Platform Native)

<!--lint disable double-link-->
The [Cross-Platform Frameworks](#cross-platform-frameworks) listed below all support Vulkan or ROCm as one of their backends.
<!--lint enable double-link-->

### Cross-Platform Frameworks

- [ingonyama-zk/icicle](https://github.com/ingonyama-zk/icicle) - Production-ready ZK acceleration library with CUDA, Metal, and ROCm backends and iOS/Android support. `GPU: Multi` `Op: MSM, NTT` `Lang: C++, Rust`.

- [tracel-ai/cubecl](https://github.com/tracel-ai/cubecl) - GPU compute framework for Rust using a proc-macro kernel language that compiles to WebGPU, CUDA, ROCm, and Metal. `GPU: Multi` `Op: Field Arithmetic` `Lang: Rust`.

- [lambdaclass/lambdaworks](https://github.com/lambdaclass/lambdaworks) - Full ZK cryptography library with GPU-accelerated FFT, MSM, and Merkle tree backends for CUDA and Metal. `GPU: Multi` `Op: FFT, MSM` `Lang: Rust`.

- [mratsim/constantine](https://github.com/mratsim/constantine) - High-performance elliptic-curve and pairing library in Nim with experimental WebGPU and CUDA backends. `GPU: Multi` `Op: EC Arithmetic, MSM` `Lang: Nim`.

- [spaceandtimefdn/blitzar](https://github.com/spaceandtimefdn/blitzar) - MSM and Pedersen commitment library with CUDA and CPU backends. `GPU: CUDA` `Op: MSM` `Lang: C++, Rust`.

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
| ligeroinc/ligero-prover | WebGPU | FFT-based polynomial IOPs |
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
| ligeroinc/ligero-prover | WebGPU | Hash-based proof system |
<!--lint enable table-pipe-alignment table-cell-padding-->

### Full Proof Systems

<!--lint disable table-pipe-alignment table-cell-padding awesome-spell-check-->
| Project | GPU | System | Notes |
|---------|-----|--------|-------|
| penumbra-zone/webgpu | WebGPU | Groth16 | Archived pioneer |
| ligeroinc/ligero-prover | WebGPU | Ligero | Browser via Dawn/Emscripten |
| zkonduit/ezkl | Metal | PLONK / ZKML | iOS/macOS |
| zkmopro/mopro | Metal | Multi-prover | iOS mobile toolkit |
<!--lint enable table-pipe-alignment table-cell-padding awesome-spell-check-->

## Mobile and Edge Proving

- [zkmopro/mopro](https://github.com/zkmopro/mopro) - Multi-prover mobile toolkit exposing a unified Swift/Kotlin API over Halo2, Noir, and Circom backends. `GPU: Metal` `Op: Groth16, PLONK` `Lang: Rust`.

- [zkonduit/ezkl](https://github.com/zkonduit/ezkl) - ZK machine-learning inference library on iOS and macOS, delivering ~2× speedup over the CPU path. `GPU: Metal` `Op: ZKML, PLONK` `Lang: Rust`.

## Competitions and Benchmarks

### ZPrize Entries

[ZPrize](https://www.zprize.io) is an open competition that accelerates ZK hardware and software.
The 2023 WebGPU track (Prize #2) produced the fastest known browser-native MSM implementations;
<!--lint disable double-link-->
all entries are listed in the [WebGPU](#webgpu-browser-native) section above.
<!--lint enable double-link-->

### Benchmarks and Metrics

- [moven0831/field-ops-benchmarks](https://github.com/moven0831/field-ops-benchmarks) - Benchmarks for M31 and BN254 field operations across Metal and WebGPU backends. `GPU: Metal, WebGPU` `Op: Field Arithmetic`.

- [ethproofs.org — client-side proving dashboard](https://ethproofs.org) - Live leaderboard tracking proof generation times and costs for client-side Ethereum proof systems across various hardware targets.

## Learning Resources

### Blog Posts and Articles

- [Mopro: "Metal MSM v2"](https://zkmopro.org/blog/metal-msm-v2) - Technical walkthrough of the second-generation Metal MSM implementation in zkmopro/gpu-acceleration, explaining the 40–100× CPU speedup.

- [zkSecurity: "Accelerating ZK Proving with WebGPU"](https://www.zksecurity.xyz/blog/posts/webgpu) - Explores how WebGPU NTT shaders accelerate Stwo STARK proving in the browser.

- [Geometry Research: "Accelerating Client-Side ZK with WebGPU"](https://geometryresearch.xyz/notebook/accelerating-client-side-zk-with-webgpu) - Introduces the MSL secp256k1 work and frames the broader opportunity for WebGPU in client-side ZK proving.

- [PSE: "Client-Side GPU Acceleration for ZK"](https://pse.dev/blog/client-side-gpu-everyday-ef-privacy) - Frames why client-side GPU proving matters for Ethereum privacy and surveys six key GPU-accelerable ZK primitives.

### Research Papers

- [cuZK: Accelerating Zero-Knowledge Proof with a Faster Parallel Multi-Scalar Multiplication Algorithm on GPUs](https://eprint.iacr.org/2022/1321) - IACR 2022/1321; introduces the cuZK MSM algorithm that underpins most WebGPU and Metal MSM implementations in this list. `Op: MSM`.

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
