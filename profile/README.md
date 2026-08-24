<div align="center">
  <img src="https://raw.githubusercontent.com/aiperceivable/apcore/main/apcore-logo.svg" alt="apcore" width="160"/>

  <h1>AI-Perceivable</h1>

  <p><strong>Build once, invoke by code or AI.</strong><br/>
  A schema-enforced module standard for software that agents are allowed to operate.</p>

  <p>
    <strong>English</strong> ·
    <a href="https://github.com/aiperceivable/.github/blob/main/profile/README-zh.md">中文</a>
  </p>

  <p>
    <a href="https://apcore.aiperceivable.com/">Documentation</a> ·
    <a href="https://apcore.aiperceivable.com/getting-started/">Getting Started</a> ·
    <a href="https://github.com/aiperceivable/apcore/blob/main/PROTOCOL_SPEC.md">Protocol Spec</a> ·
    <a href="https://aiperceivable.com">aiperceivable.com</a>
  </p>
</div>

---

## The problem we are solving

A tool schema tells a model **how to format a call**. It does not say who may make that call, what it may touch, whether a human must approve it first, or what record survives afterwards.

Those questions do not belong inside the model's reasoning loop. They belong at the execution boundary — and they must be answered the same way regardless of which protocol carried the request.

> **MCP tells an agent what it *can* call. apcore decides whether this call — with these arguments, in this environment, by this identity — should happen at all, and leaves evidence that it did.**

```text
                  ┌──────────────────────────────────────────┐
                  │     Agent  ·  Human  ·  Application       │
                  └────────────────────┬─────────────────────┘
                                       │
          ┌──────────┬──────────┬──────┴────┬──────────┬──────────┐
          ▼          ▼          ▼           ▼          ▼          ▼
       ┌─────┐   ┌───────┐  ┌───────┐   ┌──────┐   ┌───────┐  ┌────────┐
       │ MCP │   │  A2A  │  │  CLI  │   │ HTTP │   │OpenAI │  │ direct │
       └──┬──┘   └───┬───┘  └───┬───┘   └──┬───┘   └───┬───┘  └───┬────┘
          └──────────┴──────────┴─────┬────┴───────────┴──────────┘
                                      ▼
              ╔═══════════════════════════════════════════════╗
              ║              apcore runtime                   ║
              ║   schema · ACL · approval gate · middleware    ║
              ║          observability · audit trail          ║
              ╚═══════════════════════┬═══════════════════════╝
                                      ▼
                  ┌──────────────────────────────────────────┐
                  │      your existing business logic        │
                  │              (unchanged)                 │
                  └──────────────────────────────────────────┘
```

Define a governed capability once. Project it onto any surface. **The guarantees do not weaken when the transport changes.**

---

## Start here

```bash
pip install apcore          # Python
npm install apcore-js       # TypeScript
cargo add apcore            # Rust
```

New to the project? Read [**What is apcore?**](https://github.com/aiperceivable/apcore#what-is-apcore) first — it explains the module standard before any SDK detail.

---

## Repository map

### Core standard

`apcore` is a **protocol specification**. Each language is an independent implementation, kept honest by a shared cross-language conformance suite rather than by shared code.

| Repository | Role |
|---|---|
| [**apcore**](https://github.com/aiperceivable/apcore) | Specification, schemas, conformance fixtures, documentation |
| [apcore-python](https://github.com/aiperceivable/apcore-python) | Python SDK — `pip install apcore` |
| [apcore-typescript](https://github.com/aiperceivable/apcore-typescript) | TypeScript SDK — `npm install apcore-js` |
| [apcore-rust](https://github.com/aiperceivable/apcore-rust) | Rust SDK — `cargo add apcore` |

### Surface adapters

One module, projected onto a different protocol. Each surface is specified once and implemented three times.

| Surface | Specification | Python | TypeScript | Rust |
|---|---|---|---|---|
| **MCP** | [apcore-mcp](https://github.com/aiperceivable/apcore-mcp) | [python](https://github.com/aiperceivable/apcore-mcp-python) | [typescript](https://github.com/aiperceivable/apcore-mcp-typescript) | [rust](https://github.com/aiperceivable/apcore-mcp-rust) |
| **A2A** | [apcore-a2a](https://github.com/aiperceivable/apcore-a2a) | [python](https://github.com/aiperceivable/apcore-a2a-python) | [typescript](https://github.com/aiperceivable/apcore-a2a-typescript) | [rust](https://github.com/aiperceivable/apcore-a2a-rust) |
| **CLI** | [apcore-cli](https://github.com/aiperceivable/apcore-cli) | [python](https://github.com/aiperceivable/apcore-cli-python) | [typescript](https://github.com/aiperceivable/apcore-cli-typescript) | [rust](https://github.com/aiperceivable/apcore-cli-rust) |

### Framework integrations

Scan the code you already have. No rewrite, no parallel definition to keep in sync.

| Repository | Ecosystem |
|---|---|
| [fastapi-apcore](https://github.com/aiperceivable/fastapi-apcore) · [django-apcore](https://github.com/aiperceivable/django-apcore) · [flask-apcore](https://github.com/aiperceivable/flask-apcore) · [pydantic-ai-apcore](https://github.com/aiperceivable/pydantic-ai-apcore) | Python |
| [nestjs-apcore](https://github.com/aiperceivable/nestjs-apcore) · [tiptap-apcore](https://github.com/aiperceivable/tiptap-apcore) | TypeScript |
| [axum-apcore](https://github.com/aiperceivable/axum-apcore) | Rust |
| [apcore-toolkit](https://github.com/aiperceivable/apcore-toolkit) ([py](https://github.com/aiperceivable/apcore-toolkit-python) · [ts](https://github.com/aiperceivable/apcore-toolkit-typescript) · [rs](https://github.com/aiperceivable/apcore-toolkit-rust)) | Shared scanner and schema-extraction logic behind all of the above |

### Beyond the framework

| Repository | What it does |
|---|---|
| [**apexe**](https://github.com/aiperceivable/apexe) | Wraps an existing **CLI binary** into a governed module by scanning its help output — no source access required |
| [**apflow**](https://github.com/aiperceivable/apflow) | Distributed task orchestration where every capability in the graph is an apcore module |
| [mcp-embedded-ui](https://github.com/aiperceivable/mcp-embedded-ui) | A lightweight browser-based tool explorer that any MCP server can embed |

### Development tooling

| Repository | What it does |
|---|---|
| [apdev](https://github.com/aiperceivable/apdev) | Character validation, circular-import detection, and other repo hygiene checks |
| [unirelease](https://github.com/aiperceivable/unirelease) | One release pipeline across Rust, Go, Node, Bun, and Python |
| [apcore-skills](https://github.com/aiperceivable/apcore-skills) | Cross-language SDK synchronization, multi-repo audits, coordinated releases |
| [apcore-refinery](https://github.com/aiperceivable/apcore-refinery) | Module metadata quality assessment for MCP, A2A, and CLI consumption |

---

## Contributing

Every repository is **Apache-2.0**. The core project follows [OpenSSF Best Practices](https://www.bestpractices.dev/projects/12294).

- [Contributing guide](https://github.com/aiperceivable/apcore/blob/main/CONTRIBUTING.md)
- [Governance](https://github.com/aiperceivable/apcore/blob/main/GOVERNANCE.md) · [Scope](https://github.com/aiperceivable/apcore/blob/main/SCOPE.md) · [Roadmap](https://github.com/aiperceivable/apcore/blob/main/ROADMAP.md)
- [Security policy](https://github.com/aiperceivable/apcore/blob/main/SECURITY.md)

The most useful contributions are the uncomfortable ones: a conformance case where two languages disagree, an authorization semantic the spec left ambiguous, or a surface where the governance guarantees quietly do not hold. Open an issue.
