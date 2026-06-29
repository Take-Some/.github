# Take Some() Entertainment

<p align="center">
  <img
    src="Banner.png"
    alt="Take Some() Entertainment — Real-time Nature Rendering"
    width="100%"
  />
</p>

<p align="center">
  <strong>Independent game technology studio building modular runtime infrastructure for creators.</strong>
</p>

<p align="center">
  <img alt="Studio" src="https://img.shields.io/badge/Take%20Some%28%29-Entertainment-111827?style=for-the-badge" />
  <img alt="Architecture" src="https://img.shields.io/badge/Architecture-Engine%20as%20Host-0f172a?style=for-the-badge" />
  <img alt="Delivery" src="https://img.shields.io/badge/Delivery-Feature%20as%20Plugin-1e293b?style=for-the-badge" />
</p>

<p align="center">
  <img alt="Runtime" src="https://img.shields.io/badge/Runtime-Modular%20%2F%20Gateway%20Driven-1f2937?style=for-the-badge" />
  <img alt="Assets" src="https://img.shields.io/badge/Assets-AssetManager%20Routed-334155?style=for-the-badge" />
  <img alt="Stack" src="https://img.shields.io/badge/Stack-Rust%20%2B%20Vulkan-475569?style=for-the-badge" />
</p>

---

## Mission

**Take Some() Entertainment** builds game technology for developers who need control, extensibility, and a clean runtime architecture.

Our core principle is simple:

> **Engine as host. Feature as plugin.**

The engine should not trap creators inside rigid assumptions. It should host systems, expose stable contracts, and let specialized modules evolve independently.

We focus on:

- modular runtime architecture;
- gateway-driven service boundaries;
- plugin-first feature delivery;
- deterministic CI and reproducible builds;
- asset, UI, rendering, input, physics, logging and tooling infrastructure;
- creator freedom without sacrificing engineering discipline.

---

## Technology Direction

### NewEngine

**NewEngine** is the core runtime platform.

It is designed around:

- explicit service contracts;
- runtime gateways;
- modular crate and plugin boundaries;
- asset-driven pipelines;
- deterministic loading and diagnostics;
- replaceable platform, renderer, UI, physics, input, logging and tooling layers.

Core architecture rule:

```text
engine-core hosts
plugins implement
gateways route
assets describe
CI verifies
```

---

## Repository Map

| Repository | Purpose |
| --- | --- |
| [`NewEngine`](https://github.com/Take-Some/NewEngine) | Core runtime, APIs, gateways, shared engine crates and platform contracts. |
| [`AssetManager`](https://github.com/Take-Some/AssetManager) | Asset registry, codecs, NEF8 containers, package/index handling and runtime asset access. |
| [`AureliaUI`](https://github.com/Take-Some/AureliaUI) | UI provider, layout, styling, interaction, paint commands, font/icon support. |
| [`winit-platform-plugin`](https://github.com/Take-Some/winit-platform-plugin) | Winit-based native window, platform loop, input surface and runtime host bridge. |
| [`JoltPhysics`](https://github.com/Take-Some/JoltPhysics) | Jolt-backed physics integration and NewEngine physics adapter. |
| [`VulkanRenderer`](https://github.com/Take-Some/VulkanRenderer) | Vulkan renderer plugin and graphics runtime integration. |
| [`InputPlugin`](https://github.com/Take-Some/InputPlugin) | Input module, keyboard/mouse/gamepad state and event normalization. |
| [`LoggingPlugin`](https://github.com/Take-Some/LoggingPlugin) | Runtime logging, diagnostics, `.ulog` pipeline and event infrastructure. |
| [`ProfilerPlugin`](https://github.com/Take-Some/ProfilerPlugin) | Runtime profiling, frame diagnostics and performance reporting. |
| [`FlecsECS`](https://github.com/Take-Some/FlecsECS) | ECS integration and entity/component runtime bridge. |

---

## Engineering Policy

### 1. No hardcoded feature ownership

Features belong behind contracts, not inside the engine core.

```text
Bad:  engine knows every concrete system.
Good: engine routes to declared services and plugins.
```

### 2. Asset access goes through AssetManager

Runtime systems should not bypass asset routing.

```text
All production asset reads must go through the AssetManager API.
```

### 3. CI is part of the code

A repository is not healthy unless it builds in automation.

Required baseline:

```bash
cargo check --workspace --all-targets --locked
cargo test --workspace --all-targets --locked
cargo clippy --workspace --all-targets --locked --no-deps -- -D warnings
```

Native plugins may add platform-specific checks, release builds, artifact upload and packaging validation.

### 4. Warnings are treated as failures

Warnings are future bugs. CI uses strict warning gates where practical.

```bash
-D warnings
```

Lint exceptions must be explicit and justified at crate or module level.

### 5. Build outputs are artifacts, not source

Generated binaries, cache files, target directories and temporary build products must not be committed unless the repository explicitly defines them as release artifacts.

---

## Git & GitHub Standards

### Branches

Use short, scoped branch names:

```text
fix/ci-upload-artifact-v7
fix/jolt-native-build
feat/loading-kernel
refactor/asset-registry
docs/plugin-policy
```

### Commits

Prefer clear, imperative commit messages:

```text
Fix release artifact output path
Update upload artifact action
Add loading manifest registry
Stabilize AureliaUI clippy policy
```

For larger work, use structured commits:

```text
feat(asset): add NEF8 manifest route
fix(ci): pin native build generator
docs(policy): document plugin boundaries
refactor(ui): split layout registry
```

### Pull Requests

Every PR should answer:

- What changed?
- Why was it needed?
- What contracts or runtime behavior are affected?
- What was tested?
- Are there migration notes?

### Recommended repository files

Each production repository should include:

```text
README.md
LICENSE
CONTRIBUTING.md
SECURITY.md
CODEOWNERS
.github/workflows/ci.yml
.github/dependabot.yml
.gitignore
```

### CI artifact policy

GitHub Actions artifact upload should use the current maintained action version:

```yaml
uses: actions/upload-artifact@v7
```

Release DLLs should be uploaded from stable target paths:

```yaml
path: target/release/*.dll
```

### Protection rules

Recommended `main` branch protection:

- require pull request before merge;
- require status checks;
- require CI success;
- block force-pushes;
- block branch deletion;
- require linear history where practical;
- require review for runtime-critical repositories.

---

## Build Philosophy

NewEngine repositories should support both local iteration and CI reproducibility.

Local developer flow:

```bash
cargo check --workspace --all-targets --locked
cargo test --workspace --all-targets --locked
cargo clippy --workspace --all-targets --locked --no-deps -- -D warnings
```

Release flow:

```bash
cargo build --workspace --locked --profile release
```

Native plugins must document external toolchain requirements such as:

- MSVC / clang-cl;
- CMake;
- Ninja;
- Vulkan SDK;
- platform-specific SDKs.

---

## Teams

| Team | Responsibility |
| --- | --- |
| Engine Team | Runtime host, gateways, API contracts, lifecycle, loading, diagnostics. |
| Rendering Team | Vulkan renderer, frame graph, shader/material systems, render passes. |
| Asset Team | AssetManager, codecs, NEF8 packages, import/export, content routing. |
| UI Team | AureliaUI, layout, styling, interaction, font/icon/render command pipeline. |
| Physics Team | Physics API, Jolt integration, simulation adapters, collision pipeline. |
| Tools Team | Developer tools, packers, inspectors, validators, build utilities. |
| Docs Team | Architecture, policies, onboarding, contribution guides. |
| Art & Experience Team | UI/UX, concepts, visual identity, demos, presentation assets. |

---

## Contribution

We welcome contributors who value disciplined engineering and creative freedom.

Before contributing:

1. Read the repository `README.md`.
2. Check `CONTRIBUTING.md`.
3. Run the repository CI commands locally.
4. Keep changes scoped.
5. Avoid unrelated formatting churn.
6. Prefer explicit contracts over hidden behavior.
7. Document new runtime assumptions.

---

## Security

Please do not report security issues through public issues.

Use the repository `SECURITY.md` when available. If a repository does not yet provide one, open a minimal private contact request through the maintainers.

---

## Licensing

Licensing is repository-specific.

Some repositories may use permissive licensing, while internal runtime or studio infrastructure may use a proprietary policy. Always check the repository-level `LICENSE`, crate metadata and contribution terms before reuse.

---

## Core Statement

**Take Some() Entertainment** builds tools for people who want to create without asking the engine for permission.

```text
Host the runtime.
Declare the contract.
Load the plugin.
Route the asset.
Verify with CI.
Ship the experience.
```
