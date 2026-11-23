<div align="center">
  <img src="https://raw.githubusercontent.com/aiperceivable/apcore/main/apcore-logo.svg" alt="apcore" width="160"/>

  <h1>AI-Perceivable</h1>

  <p><strong>Build once, invoke by code or AI.</strong><br/>
  A protocol-neutral runtime that keeps schema validation, ACL, approval, and execution evidence consistent across agent and code surfaces.</p>

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

Three steps, and only the first one touches your code:

1. **Bind** — a decorator, or a YAML binding file for code you cannot annotate
2. **Expose** — the same registered module is projected through whichever adapters you enable
3. **Execute under governance** — validation, identity, ACL, approval, and audit run before and after your logic, identically on every surface

Define a governed capability once. Project it onto any surface. **The guarantees do not weaken when the transport changes.**

---

## How this relates to MCP

This is the most common question, so it is worth answering directly: **apcore is not an alternative to MCP.** MCP defines client-server communication and tool metadata. apcore addresses a different boundary — defining and executing an application capability consistently *before* it is exposed through MCP or anything else.

| Concern | MCP | apcore |
|---|---|---|
| **Primary role** | Client-server protocol and tool surface | Capability definition and governed execution runtime |
| **Schema and hints** | Tool input/output schemas and annotations | Required schemas plus runtime-enforced validation |
| **Access and approval** | Implemented by the server or deployment | ACL and approval gates enforced by the execution pipeline |
| **Auditability** | Implemented by the server or deployment | Trace context, structured errors, events, usage hooks |
| **Language model** | Protocol SDKs | Semantically aligned Python, TypeScript, and Rust SDKs |

**Use MCP directly** when a protocol server is all you need. **Add apcore** when the same business capability must retain validation, access, approval, and audit semantics across MCP, A2A, CLI, HTTP, and direct code.

The same logic applies upward: apcore is complementary to LangChain, LlamaIndex, CrewAI, and AutoGen rather than competing with them. Those frameworks orchestrate *calls between* capabilities; apcore standardizes what an individual capability **is** and what happens when one is invoked.

> apcore solves **how to build modules** (a module standard), not how to call tools (a communication protocol).

---

## Why apcore

| | |
|---|---|
| **Agent-readable** | Descriptions, input schemas, output schemas, and behavioral metadata give automated callers a structured contract. Model interpretation remains a caller responsibility. |
| **Schema-driven** | Strict input/output contracts on every interface — predictable, validatable, self-documenting. |
| **Governed execution** | Identity, ACL, approval gates, call-chain guards, middleware, and validation run before and after application logic. |
| **Protocol-neutral** | Use the native SDK directly, or project registered modules through maintained MCP, A2A, CLI, and framework adapters. |
| **Cross-language** | Maintained Python, TypeScript, and Rust SDKs implement one normative protocol and shared conformance fixtures, with language-idiomatic APIs. |
| **Open source** | Apache-2.0. The protocol, SDKs, conformance fixtures, and governance documents are all public. |

---

## Start here

```bash
pip install apcore          # Python
npm install apcore-js       # TypeScript
cargo add apcore            # Rust
```

New to the project? Read [**What is apcore?**](https://github.com/aiperceivable/apcore#what-is-apcore) first — it explains the module standard before any SDK detail.

**Adopt incrementally.** Define and test one application operation in the native SDK. Add an MCP or CLI adapter only after its validation, ACL, approval, and evidence paths are clear. One governed capability is a better starting point than a full migration.

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

### Toolkit and surfaces

[**apcore-surfaces**](https://github.com/aiperceivable/apcore-surfaces) hosts Toolkit, MCP, A2A, and CLI. Each component shares documentation and conformance fixtures across its three implementations.

| Component | Documentation | Python | TypeScript | Rust |
|---|---|---|---|---|
| **Toolkit** | [Toolkit](https://aiperceivable.github.io/apcore-surfaces/toolkit/) | [python](https://github.com/aiperceivable/apcore-surfaces/tree/main/toolkit/python) | [typescript](https://github.com/aiperceivable/apcore-surfaces/tree/main/toolkit/typescript) | [rust](https://github.com/aiperceivable/apcore-surfaces/tree/main/toolkit/rust) |
| **MCP** | [MCP](https://aiperceivable.github.io/apcore-surfaces/mcp/) | [python](https://github.com/aiperceivable/apcore-surfaces/tree/main/mcp/python) | [typescript](https://github.com/aiperceivable/apcore-surfaces/tree/main/mcp/typescript) | [rust](https://github.com/aiperceivable/apcore-surfaces/tree/main/mcp/rust) |
| **A2A** | [A2A](https://aiperceivable.github.io/apcore-surfaces/a2a/) | [python](https://github.com/aiperceivable/apcore-surfaces/tree/main/a2a/python) | [typescript](https://github.com/aiperceivable/apcore-surfaces/tree/main/a2a/typescript) | [rust](https://github.com/aiperceivable/apcore-surfaces/tree/main/a2a/rust) |
| **CLI** | [CLI](https://aiperceivable.github.io/apcore-surfaces/cli/) | [python](https://github.com/aiperceivable/apcore-surfaces/tree/main/cli/python) | [typescript](https://github.com/aiperceivable/apcore-surfaces/tree/main/cli/typescript) | [rust](https://github.com/aiperceivable/apcore-surfaces/tree/main/cli/rust) |

### Framework integrations

Scan the code you already have. No rewrite, no parallel definition to keep in sync.

| Repository | Ecosystem |
|---|---|
| [fastapi-apcore](https://github.com/aiperceivable/fastapi-apcore) · [django-apcore](https://github.com/aiperceivable/django-apcore) · [flask-apcore](https://github.com/aiperceivable/flask-apcore) | Python |
| [nestjs-apcore](https://github.com/aiperceivable/nestjs-apcore) · [hono-apcore](https://github.com/aiperceivable/hono-apcore) · [tiptap-apcore](https://github.com/aiperceivable/tiptap-apcore) | TypeScript |
| [axum-apcore](https://github.com/aiperceivable/axum-apcore) | Rust |

**Agent frameworks** — [pydantic-ai-apcore](https://github.com/aiperceivable/pydantic-ai-apcore) works in both directions: it hands governed apcore modules to a [pydantic-ai](https://ai.pydantic.dev/) agent as an `AbstractToolset`, and registers the tools that agent already has as apcore modules — so they gain schema validation, ACL, approval gates, and audit, and can then be served over MCP, CLI, or A2A without a rewrite.

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

## Help build the standard

apcore is open source and maintainer-led. The contributions that help most are the ones that strengthen conformance, the supported adoption path, or verified documentation.

| | |
|---|---|
| **Implement an SDK** | Port apcore to a new language. [The conformance fixtures](https://github.com/aiperceivable/apcore/tree/main/conformance) are your spec — pass them and you have a compliant SDK. |
| **Build an adapter** | Expose apcore modules over a new surface — a web framework, a protocol, a runtime. One guide, one registry to read from. |
| **Pick a good first issue** | Small, scoped tasks across the SDKs and docs, labeled for newcomers and ready to claim. |
| **Join the discussion** | Ask questions, propose spec changes, and shape the roadmap in the open. |

---

## Contributing

Every repository is **Apache-2.0**. The core project follows [OpenSSF Best Practices](https://www.bestpractices.dev/projects/12294).

- [Contributing guide](https://github.com/aiperceivable/apcore/blob/main/CONTRIBUTING.md) · [Code of Conduct](https://github.com/aiperceivable/.github/blob/main/CODE_OF_CONDUCT.md)
- [Governance](https://github.com/aiperceivable/apcore/blob/main/GOVERNANCE.md) · [Scope](https://github.com/aiperceivable/apcore/blob/main/SCOPE.md) · [Roadmap](https://github.com/aiperceivable/apcore/blob/main/ROADMAP.md)
- [Security policy](https://github.com/aiperceivable/apcore/blob/main/SECURITY.md)

The most useful contributions are the uncomfortable ones: a conformance case where two languages disagree, an authorization semantic the spec left ambiguous, or a surface where the governance guarantees quietly do not hold. Open an issue.
