<div align="center">
  <img src="https://raw.githubusercontent.com/aiperceivable/apcore/main/apcore-logo.svg" alt="apcore" width="160"/>

  <h1>AI-Perceivable</h1>

  <p><strong>一次构建，代码或 AI 均可调用。</strong><br/>
  协议无关的治理运行时，让 Schema 校验、ACL、审批与执行证据在智能体和代码接口之间保持一致。</p>

  <p>
    <a href="https://github.com/aiperceivable/.github/blob/main/profile/README.md">English</a> ·
    <strong>中文</strong>
  </p>

  <p>
    <a href="https://apcore.aiperceivable.com/">文档</a> ·
    <a href="https://apcore.aiperceivable.com/getting-started/">快速上手</a> ·
    <a href="https://github.com/aiperceivable/apcore/blob/main/PROTOCOL_SPEC.md">协议规范</a> ·
    <a href="https://aiperceivable.com">aiperceivable.com</a>
  </p>
</div>

---

## 我们在解决什么问题

一份 tool schema 告诉模型**如何组装一次调用**。但它没有说：谁有权发起这次调用、它可以触及什么、是否必须先经过人工批准、事后又留下了什么记录。

这些问题不属于模型的推理循环。它们属于执行边界 —— 并且无论请求由哪种协议送达，答案都必须完全一致。

> **MCP 告诉 agent 它*能*调用什么。apcore 决定这次调用——带着这些参数、在这个环境里、以这个身份——究竟*该不该*发生，并留下它确实发生过的证据。**

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

三步，其中只有第一步会碰到你的代码：

1. **绑定** —— 一个装饰器；对于无法添加注解的代码，用一份 YAML 绑定文件
2. **暴露** —— 同一个已注册模块，通过你启用的任意适配器对外投影
3. **受治理执行** —— 校验、身份、ACL、审批与审计在你的业务逻辑前后运行，在每一种表面上行为一致

一次定义受治理的能力，然后把它投射到任意表面。**传输协议变了，保证不会随之削弱。**

---

## 与 MCP 是什么关系

这是最常被问到的问题，值得直接回答：**apcore 不是 MCP 的替代品。** MCP 定义的是客户端-服务端通信与工具元数据；apcore 处理的是另一条边界 —— 在一项应用能力被 MCP 或其他任何方式暴露*之前*，如何一致地定义并执行它。

| 关注点 | MCP | apcore |
|---|---|---|
| **主要职责** | 客户端-服务端协议与工具表面 | 能力定义与受治理的执行运行时 |
| **Schema 与提示** | 工具的输入/输出 schema 与注解 | 强制要求 schema，外加运行时强制校验 |
| **访问与审批** | 由 server 或部署方自行实现 | 由执行管线强制施加 ACL 与审批门禁 |
| **可审计性** | 由 server 或部署方自行实现 | 追踪上下文、结构化错误、事件与用量钩子 |
| **语言模型** | 协议 SDK | 语义对齐的 Python、TypeScript、Rust SDK |

**只需要一个协议 server 时，直接用 MCP。** 当同一项业务能力必须在 MCP、A2A、CLI、HTTP 与直接代码调用之间保持一致的校验、访问、审批与审计语义时，**再加上 apcore**。

向上一层同理：apcore 与 LangChain、LlamaIndex、CrewAI、AutoGen 是互补而非竞争关系。那些框架编排的是能力*之间*的调用；apcore 标准化的是单项能力**本身是什么**，以及它被调用时会发生什么。

> apcore 解决的是**如何构建模块**（模块标准），而不是如何调用工具（通信协议）。

---

## 为什么选择 apcore

| | |
|---|---|
| **智能体可读取** | 描述、输入输出 Schema 与行为元数据为自动调用方提供结构化契约；模型如何理解和选择工具仍由调用方负责。 |
| **Schema 驱动** | 每个接口都有严格的输入/输出契约 —— 可预测、可验证且自带文档。 |
| **受治理执行** | 身份、ACL、审批门禁、调用链防护、中间件与校验在业务逻辑执行前后统一生效。 |
| **协议无关** | 可直接使用原生 SDK，也可通过受维护的 MCP、A2A、CLI 与框架适配器投影模块。 |
| **跨语言** | 受维护的 Python、TypeScript、Rust SDK 实现同一份规范性协议与共享的一致性测试夹具，并各自保持语言习惯的 API。 |
| **开源** | 采用 Apache-2.0 协议。协议、SDK、一致性测试夹具与治理文档均公开。 |

---

## 从这里开始

```bash
pip install apcore          # Python
npm install apcore-js       # TypeScript
cargo add apcore            # Rust
```

第一次接触？建议先读 [**什么是 apcore**](https://github.com/aiperceivable/apcore#what-is-apcore) —— 它先讲清模块标准本身，再进入任何 SDK 细节。

**渐进式采用。** 先在原生 SDK 中定义并测试一个应用操作；确认它的校验、ACL、审批和证据路径都清晰之后，再添加 MCP 或 CLI 适配器。从一个受治理能力开始，好过一次性全量迁移。

---

## 仓库地图

### 核心标准

`apcore` 是一份**协议规范**。每种语言都是独立实现，把它们约束在一起的是一套共享的跨语言 conformance 套件，而不是共享代码。

| 仓库 | 职责 |
|---|---|
| [**apcore**](https://github.com/aiperceivable/apcore) | 规范、schema、conformance 测试夹具、文档 |
| [apcore-python](https://github.com/aiperceivable/apcore-python) | Python SDK —— `pip install apcore` |
| [apcore-typescript](https://github.com/aiperceivable/apcore-typescript) | TypeScript SDK —— `npm install apcore-js` |
| [apcore-rust](https://github.com/aiperceivable/apcore-rust) | Rust SDK —— `cargo add apcore` |

### 表面适配器

同一个模块，投射到不同协议之上。每个表面只规范一次，实现三次。

| 表面 | 规范 | Python | TypeScript | Rust |
|---|---|---|---|---|
| **MCP** | [apcore-mcp](https://github.com/aiperceivable/apcore-mcp) | [python](https://github.com/aiperceivable/apcore-mcp-python) | [typescript](https://github.com/aiperceivable/apcore-mcp-typescript) | [rust](https://github.com/aiperceivable/apcore-mcp-rust) |
| **A2A** | [apcore-a2a](https://github.com/aiperceivable/apcore-a2a) | [python](https://github.com/aiperceivable/apcore-a2a-python) | [typescript](https://github.com/aiperceivable/apcore-a2a-typescript) | [rust](https://github.com/aiperceivable/apcore-a2a-rust) |
| **CLI** | [apcore-cli](https://github.com/aiperceivable/apcore-cli) | [python](https://github.com/aiperceivable/apcore-cli-python) | [typescript](https://github.com/aiperceivable/apcore-cli-typescript) | [rust](https://github.com/aiperceivable/apcore-cli-rust) |

### 框架集成

扫描你已经写好的代码。不用重写，也不用维护第二份平行定义。

| 仓库 | 生态 |
|---|---|
| [fastapi-apcore](https://github.com/aiperceivable/fastapi-apcore) · [django-apcore](https://github.com/aiperceivable/django-apcore) · [flask-apcore](https://github.com/aiperceivable/flask-apcore) | Python |
| [nestjs-apcore](https://github.com/aiperceivable/nestjs-apcore) · [hono-apcore](https://github.com/aiperceivable/hono-apcore) · [tiptap-apcore](https://github.com/aiperceivable/tiptap-apcore) | TypeScript |
| [axum-apcore](https://github.com/aiperceivable/axum-apcore) | Rust |
| [apcore-toolkit](https://github.com/aiperceivable/apcore-toolkit) ([py](https://github.com/aiperceivable/apcore-toolkit-python) · [ts](https://github.com/aiperceivable/apcore-toolkit-typescript) · [rs](https://github.com/aiperceivable/apcore-toolkit-rust)) | 支撑以上全部集成的共享扫描与 schema 提取逻辑 |

**Agent 框架** —— [pydantic-ai-apcore](https://github.com/aiperceivable/pydantic-ai-apcore) 双向可用：既把受治理的 apcore 模块以 `AbstractToolset` 的形式交给 [pydantic-ai](https://ai.pydantic.dev/) agent，也把该 agent 已经拥有的工具注册为 apcore 模块 —— 于是它们获得 schema 校验、ACL、审批门与审计，并可以不经重写就通过 MCP、CLI 或 A2A 对外提供。

### 框架之外

| 仓库 | 作用 |
|---|---|
| [**apexe**](https://github.com/aiperceivable/apexe) | 通过扫描 help 输出，把现成的 **CLI 二进制文件**包装成受治理的模块 —— 无需接触源码 |
| [**apflow**](https://github.com/aiperceivable/apflow) | 分布式任务编排，图中每一项能力本身都是一个 apcore 模块 |
| [mcp-embedded-ui](https://github.com/aiperceivable/mcp-embedded-ui) | 轻量的浏览器端工具浏览器，任何 MCP server 都能内嵌 |

### 开发工具

| 仓库 | 作用 |
|---|---|
| [apdev](https://github.com/aiperceivable/apdev) | 字符校验、循环导入检测，以及其他仓库卫生检查 |
| [unirelease](https://github.com/aiperceivable/unirelease) | 一条发布流水线覆盖 Rust、Go、Node、Bun 与 Python |
| [apcore-skills](https://github.com/aiperceivable/apcore-skills) | 跨语言 SDK 同步、多仓库审计、协同发布 |
| [apcore-refinery](https://github.com/aiperceivable/apcore-refinery) | 面向 MCP、A2A、CLI 消费场景的模块元数据质量评估 |

---

## 一起构建这个标准

apcore 是开源且由维护者主导的项目。最有帮助的贡献，是那些能强化一致性测试、受支持的采用路径或经过验证的文档的贡献。

| | |
|---|---|
| **实现一个 SDK** | 把 apcore 移植到新语言。[一致性测试夹具](https://github.com/aiperceivable/apcore/tree/main/conformance)就是你的规范 —— 全部通过，你就拥有了一个合规的 SDK。 |
| **开发一个适配器** | 把 apcore 模块暴露到新的表面 —— Web 框架、协议或运行时。只需一份指南、一个注册表。 |
| **认领 good first issue** | 横跨各 SDK 与文档的小型、范围明确的任务，已为新手标注、可直接认领。 |
| **参与讨论** | 公开提问、提议规范变更、共同塑造路线图。 |

---

## 参与贡献

所有仓库均采用 **Apache-2.0** 许可。核心项目遵循 [OpenSSF Best Practices](https://www.bestpractices.dev/projects/12294)。

- [贡献指南](https://github.com/aiperceivable/apcore/blob/main/CONTRIBUTING.md) · [行为准则](https://github.com/aiperceivable/.github/blob/main/CODE_OF_CONDUCT.md)
- [治理模式](https://github.com/aiperceivable/apcore/blob/main/GOVERNANCE.md) · [项目范围](https://github.com/aiperceivable/apcore/blob/main/SCOPE.md) · [路线图](https://github.com/aiperceivable/apcore/blob/main/ROADMAP.md)
- [安全策略](https://github.com/aiperceivable/apcore/blob/main/SECURITY.md)

最有价值的贡献往往是让人不舒服的那一类：一个让两种语言给出不同结果的 conformance 用例、一处规范留下歧义的授权语义，或者某个表面上治理保证悄悄失效的地方。欢迎提 issue。
