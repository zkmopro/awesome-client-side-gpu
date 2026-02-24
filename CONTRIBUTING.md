# Contributing to awesome-client-side-gpu

Thanks for helping curate this list! Please read the guidelines below before opening a PR.

## Scope Rule

**In scope:** Projects that perform GPU-accelerated cryptographic operations on *user devices* — browsers, mobile phones, laptops, or desktops. Examples: WebGPU in a browser, Metal on an iPhone, Vulkan on a gaming PC.

**Out of scope:** Libraries that *only* target server-rack CUDA (no portable GPU backend). Cross-platform libraries that happen to support CUDA *and* at least one portable target (Metal, WebGPU, ROCm, Vulkan) **are in scope**.

If you are unsure, open an issue and ask before putting in the work.

## One Item Per PR

Each PR should add exactly one entry. PR titles must follow the format:

```
Add org/repo-name
```

## Entry Format

```
- [org/repo](https://github.com/org/repo) - One sentence describing what the project does. `GPU: X` `Curve: Y1, Y2` `Op: Z1, Z2` `Lang: W`
```

Rules:
- The description is a single sentence ending with a period (`.`).
- At minimum, the `` `GPU:` `` tag must be present.
- Group multiple values with commas inside a single backtick block (e.g., `` `Op: MSM, NTT` `` not `` `Op: MSM` `Op: NTT` ``).
- Tag order: `GPU` → `Curve` → `Op` → `Lang` → `Status`.
- No star counts — they go stale.
- Archived or unmaintained repositories get an extra `` `Status: Archived` `` tag.
- All URLs must be `https://`.

## Tag Vocabulary

Use only the canonical values listed here to keep tags filterable.

### `GPU:`
| Value | Meaning |
|-------|---------|
| `WebGPU` | Runs in browser or via native WebGPU API |
| `Metal` | Apple Metal (iOS / macOS) |
| `CUDA` | NVIDIA CUDA |
| `ROCm` | AMD ROCm / HIP |
| `Vulkan` | Vulkan Compute (cross-vendor) |
| `Multi` | Supports two or more of the above |

### `Op:`
| Value | Meaning |
|-------|---------|
| `MSM` | Multi-Scalar Multiplication |
| `NTT` | Number Theoretic Transform |
| `FFT` | Fast Fourier Transform |
| `EC Arithmetic` | General elliptic-curve point operations |
| `Field Arithmetic` | Modular field operations |
| `Hash` | Cryptographic hash (Poseidon, Keccak, etc.) |
| `Groth16` | Full Groth16 proof generation |
| `STARK` | STARK proof generation |
| `PLONK` | PLONK / UltraPLONK proof generation |
| `ZKML` | ZK for machine-learning inference |
| `Polynomial Eval` | Polynomial evaluation / matrix-vector multiplication |
| `Merkle` | Merkle tree commitments |
| `Sumcheck` | Sumcheck protocol reductions |

### `Curve:`
| Value | Curve |
|-------|-------|
| `BN254` | Barreto–Naehrig 254-bit (alt_bn128) |
| `BLS12-381` | BLS 381-bit |
| `BLS12-377` | BLS 377-bit (used in ZPrize) |
| `secp256k1` | Bitcoin/Ethereum signing curve |
| `Pasta` | Pallas / Vesta (Zcash Orchard) |
| `M31` | Mersenne-31 small field |
| `BabyBear` | BabyBear small field |
| `Stark252` | Starknet 252-bit field |

### `Lang:`
Common values: `Rust`, `C++`, `TypeScript`, `JavaScript`, `Nim`, `WGSL`, `MSL`, `GLSL`.

## Cross-Reference Policy

Each project has exactly **one** primary full entry in the appropriate GPU-technology section. If it naturally fits in another section (e.g., a ZPrize entry or a Mobile section), add a brief cross-reference there — not a duplicate full entry.

## Submission Checklist

Before opening a PR, verify that:

1. The project is in scope (runs on user devices).
2. The repository is public and accessible.
3. Your entry follows the format above.
4. You are placing the primary entry in the correct section.
5. The PR title is `Add org/repo-name`.
6. You have reviewed existing entries to avoid duplicates.

For significant changes (new sections, re-structuring), open an issue first.
