<div align="center">
  <img src="https://raw.githubusercontent.com/aiperceivable/apcore/main/apcore-logo.svg" alt="apcore" width="160"/>

  <h1>AI-Perceivable</h1>

  <p><strong>一次构建，代码与 AI 皆可调用。</strong><br/>
  一套 schema 强制约束的模块标准 —— 面向那些被允许交给 agent 操作的软件。</p>

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

一次定义受治理的能力，然后把它投射到任意表面。**传输协议变了，保证不会随之削弱。**

---

## 从这里开始

```bash
pip install apcore          # Python
npm install apcore-js       # TypeScript
cargo add apcore            # Rust
```

第一次接触？建议先读 [**什么是 apcore**](https://github.com/aiperceivable/apcore#what-is-apcore) —— 它先讲清模块标准本身，再进入任何 SDK 细节。

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
| [fastapi-apcore](https://github.com/aiperceivable/fastapi-apcore) · [django-apcore](https://github.com/aiperceivable/django-apcore) · [flask-apcore](https://github.com/aiperceivable/flask-apcore) · [pydantic-ai-apcore](https://github.com/aiperceivable/pydantic-ai-apcore) | Python |
| [nestjs-apcore](https://github.com/aiperceivable/nestjs-apcore) · [tiptap-apcore](https://github.com/aiperceivable/tiptap-apcore) | TypeScript |
| [axum-apcore](https://github.com/aiperceivable/axum-apcore) | Rust |
| [apcore-toolkit](https://github.com/aiperceivable/apcore-toolkit) ([py](https://github.com/aiperceivable/apcore-toolkit-python) · [ts](https://github.com/aiperceivable/apcore-toolkit-typescript) · [rs](https://github.com/aiperceivable/apcore-toolkit-rust)) | 支撑以上全部集成的共享扫描与 schema 提取逻辑 |

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

## 参与贡献

所有仓库均采用 **Apache-2.0** 许可。核心项目遵循 [OpenSSF Best Practices](https://www.bestpractices.dev/projects/12294)。

- [贡献指南](https://github.com/aiperceivable/apcore/blob/main/CONTRIBUTING.md)
- [治理模式](https://github.com/aiperceivable/apcore/blob/main/GOVERNANCE.md) · [项目范围](https://github.com/aiperceivable/apcore/blob/main/SCOPE.md) · [路线图](https://github.com/aiperceivable/apcore/blob/main/ROADMAP.md)
- [安全策略](https://github.com/aiperceivable/apcore/blob/main/SECURITY.md)

最有价值的贡献往往是让人不舒服的那一类：一个让两种语言给出不同结果的 conformance 用例、一处规范留下歧义的授权语义，或者某个表面上治理保证悄悄失效的地方。欢迎提 issue。
