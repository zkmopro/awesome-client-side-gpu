## Submission Checklist

Thank you for contributing to **awesome-client-side-gpu**! Please verify every item before opening your PR.

### Scope
- [ ] The project runs on a **user device** (browser, mobile phone, or desktop), not exclusively on a server-side CUDA cluster.
- [ ] If it is a cross-platform library, it supports at least one of: WebGPU, Metal, ROCm, or Vulkan — not only CUDA.

### Entry Format
- [ ] **One item per PR.** PR title is `Add org/repo-name`.
- [ ] Entry is a single sentence ending with a period (`.`).
- [ ] Entry uses the inline-code tag style: `` `GPU: X` `` `` `Curve: Y` `` `` `Op: Z` `` `` `Lang: W` ``.
- [ ] At minimum the `` `GPU:` `` tag is present.
- [ ] All tag values are from the canonical vocabulary in [CONTRIBUTING.md](../CONTRIBUTING.md).
- [ ] Archived/unmaintained projects include `` `Status: Archived` ``.
- [ ] No star counts included.

### Placement
- [ ] Added to the correct **GPU Technology** section (primary entry).
- [ ] If the project spans multiple sections, secondary appearances are brief cross-references, not duplicate full entries.

### Links
- [ ] URL is `https://` (no `http://`).
- [ ] URL resolves without a redirect chain.
- [ ] The repository is public and accessible.

### Description Quality
- [ ] Description explains what the project *does* and its GPU context; not just the repo name rephrased.
- [ ] No marketing language ("blazing fast", "revolutionary", etc.).

---

<!-- Delete this line and anything below before submitting -->
**Example entry:**

```
- [org/repo](https://github.com/org/repo) - WebGPU-accelerated MSM over BN254 using the cuZK algorithm. `GPU: WebGPU` `Curve: BN254` `Op: MSM` `Lang: Rust`
```
