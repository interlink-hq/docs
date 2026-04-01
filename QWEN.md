# Project Overview

**interLink Documentation Site** — A Docusaurus-based documentation website for the interLink project, which enables Kubernetes clusters to offload container workloads to remote systems (HPC, other K8s clusters, VMs, batch systems).

## Technology Stack

- **Framework**: Docusaurus v3.0.1 (static site generator)
- **Language**: TypeScript/React
- **API Documentation**: Redocusaurus (OpenAPI/Swagger UI integration)
- **Package Manager**: Yarn (Node.js >=18.0 required)
- **Testing**: Dagger CI/CD pipelines for end-to-end testing

## Repository Structure

```
0.1.0/
├── docs/                    # Main documentation (versioned)
│   ├── cookbook/           # Deployment guides (edge, in-cluster, tunneled)
│   ├── guides/             # Technical guides (API, plugins, monitoring, etc.)
│   └── *.mdx               # Core docs (intro, architecture, limitations)
├── openapi/                # OpenAPI specifications
│   ├── interlink-openapi.json
│   └── plugin-openapi.json
├── src/                    # Custom React components and styles
│   ├── components/
│   ├── css/
│   └── pages/
├── static/                 # Static assets (images, favicon)
├── versioned_docs/         # Archived documentation versions
├── versioned_sidebars/     # Versioned sidebar configurations
└── *.config.ts             # Docusaurus, Babel, TypeScript configs
```

## Building and Running

### Installation

```bash
yarn install
```

### Local Development

```bash
yarn start --config docusaurus.config.local.ts
```

Starts a local dev server with hot reload.

### Build (Production)

```bash
yarn build
```

Generates static content in `build/` directory.

### Serve Built Site Locally

```bash
yarn serve
```

### Type Checking

```bash
yarn typecheck
```

### Deployment (GitHub Pages)

```bash
# With SSH
USE_SSH=true yarn deploy

# Without SSH
GIT_USER=<Your GitHub username> yarn deploy
```

### Dagger Module (CI/CD Preview)

```bash
dagger -m github.com/levlaz/daggerverse/docusaurus@f073c72e0a7345bba2173a15269307df297c3c13 call --src ./ serve up
```

## Development Conventions

### Documentation Standards

- **MDX Format**: Docs use `.mdx` extension for React component embedding
- **Sidebar Structure**: Auto-generated from `docs/` folder via `sidebars.ts`
- **Versioning**: Managed via `versions.json` (current versions: 0.6.x, 0.5.x, 0.4.x)
- **Frontmatter**: Each doc requires `sidebar_position` for ordering

### Code Style

- **TypeScript**: Strict typing with Docusaurus TS config
- **React**: Functional components with hooks (React 18)
- **Styling**: Custom CSS in `src/css/custom.css` with Prism themes (GitHub/Dracula)

### API Documentation

OpenAPI specs in `openapi/` are automatically bundled by Redocusaurus:
- `/plugin-openapi/` — Plugin API specification
- `/interlink-openapi/` — Core interLink API

### Content Guidelines

- Use `:::note` and `:::warning` admonitions for callouts
- Embed themed images with `<ThemedImage>` component for light/dark mode
- Link to cookbook recipes for deployment scenarios
- Maintain backward compatibility notes for versioned docs

## Core interLink Software Knowledge

### Main Repository: interlink-hq/interLink

| Metric | Value |
|--------|-------|
| **Stars** | 104 |
| **Forks** | 17 |
| **Primary Language** | Go (90.1%) |
| **License** | Apache-2.0 (CNCF hosted) |
| **Created** | July 5, 2023 |
| **Open Issues** | 41 |
| **Latest Release** | 0.6.1-pre4 (Dec 16, 2025) |

### Project Description

**interLink** is an abstraction layer for executing Kubernetes pods on remote resources capable of managing container execution lifecycles. It facilitates the development of provider-specific plugins for the Kubernetes Virtual Kubelet interface, enabling resource providers to leverage Virtual Kubelet capabilities without deep Kubernetes internals knowledge.

### Architecture Components

| Component | Description |
|-----------|-------------|
| **Virtual Kubelet (Virtual Node)** | Translates Kubernetes pod execution requests into remote calls to the interLink API server |
| **interLink API Server** | A modular, pluggable REST server with provider-specific plugins (sidecars) for different execution environments |

### Key Features

- **Plugin-based Architecture**: Extensible sidecar system for different remote providers
- **Multiple Deployment Patterns**: Edge-node, in-cluster, and tunneled configurations
- **Built-in Observability**: OpenTelemetry integration with distributed tracing and metrics
- **Secure Communication**: TLS/mTLS encryption and authentication between components
- **Authentication**: OAuth2 integration and bearer token support
- **Standard Kubernetes API**: Maintains full compatibility with existing K8s tooling

### Supported Providers

| Provider Type | Examples | Repository |
|---------------|----------|------------|
| **Docker** | Local/remote Docker daemon with DIND isolation | `interlink-docker-plugin` |
| **SLURM** | HPC batch systems with Singularity containers | `interlink-slurm-plugin` |
| **HTCondor** | HPC batch scheduling | Coming soon |
| **Remote Kubernetes** | Offload to external K8s clusters | In development |
| **Serverless** | Lambda-like event-driven execution | In development |
| **Custom** | Any container-capable remote resource | Plugin SDK available |

### Plugin Ecosystem

#### interlink-docker-plugin
- **Purpose**: Translates pod requests into Docker containers
- **Architecture**: Docker-in-Docker (DIND) for isolation
- **Latest Release**: 0.0.26-no-gpu (Mar 13, 2025)
- **Language**: Go (99.7%)
- **Features**: Create/Delete/Logs/Status endpoints, GPU support optional

#### interlink-slurm-plugin
- **Purpose**: Execute pods on SLURM batch systems with Singularity
- **Latest Release**: 0.6.1-pre1 (Mar 12, 2026)
- **Language**: Go (98.5%)
- **Features**: Flavor system (resource profiles), custom UID, HostPath volumes, ConfigMap/Secret export, health probes

#### vk-test-set (Testing Framework)
- **Purpose**: pytest-based test infrastructure for Virtual Kubelet providers
- **Language**: Python (97.8%)
- **Features**: Manifest template engine (jinja2), Kubernetes client interface, regex-based validation, pydantic config parsing

### Use Cases

**In Scope:**
- HPC Workloads: AI training, ML inference, scientific simulations
- GPU-intensive Tasks: Remote execution on GPU resources
- Batch Processing: On-demand container execution
- Hybrid Cloud: Workload distribution across multiple providers

**Out of Scope:**
- Long-running Services with continuous availability requirements
- Kubernetes Federation (multi-cluster resource management)

### Build Commands (Core Repository)

```bash
# Build all components
make all

# Run tests (Dagger containerized)
make test

# Generate OpenAPI specs
make openapi

# Clean build artifacts
make clean
```

### Recent Release Changes (0.6.1 series)

| Version | Date | Key Changes |
|---------|------|-------------|
| 0.6.1-pre4 | Dec 16, 2025 | Move pod to ready only after one container is ready |
| 0.6.1-pre3 | Dec 15, 2025 | Fix pod ready state bug at creation |
| 0.6.1-pre2 | Dec 15, 2025 | Bug fixes and improvements |

## Key Configuration Files

| File | Purpose |
|------|---------|
| `docusaurus.config.ts` | Site metadata, theme, navbar, footer, plugins |
| `sidebars.ts` | Navigation sidebar configuration |
| `package.json` | Dependencies and scripts |
| `tsconfig.json` | TypeScript compiler options |
| `versions.json` | Active documentation versions |

## Project Context

interLink is a **Virtual Kubelet ecosystem** project (originally by INFN, now a Series of LF Projects, LLC) that enables workload offloading from local Kubernetes clusters to remote providers (HPC/SLURM, remote K8s, VMs, batch systems). The documentation covers:

- **Deployment Scenarios**: Edge-node, In-cluster, Tunneled
- **Plugin Development**: API spec and provider integration guides
- **Operations**: Monitoring, mTLS, OIDC/IAM, systemd deployment
- **Cookbook**: Step-by-step deployment guides

**Warning**: interLink is in early development with potential breaking changes.

## Related GitHub Repositories

| Repository | Purpose | Stars | Latest Release |
|------------|---------|-------|----------------|
| [interLink](https://github.com/interlink-hq/interLink) | Core API server & Virtual Kubelet integration | 104 | 0.6.1-pre4 |
| [interlink-docker-plugin](https://github.com/interlink-hq/interlink-docker-plugin) | Docker container execution plugin | 1 | 0.0.26-no-gpu |
| [interlink-slurm-plugin](https://github.com/interlink-hq/interlink-slurm-plugin) | SLURM/Singularity plugin for HPC | 1 | 0.6.1-pre1 |
| [vk-test-set](https://github.com/interlink-hq/vk-test-set) | pytest-based testing framework | 1 | None (dev) |
