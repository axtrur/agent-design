# Agent Design 规范与框架蓝图仓库

这个仓库用于**系统化记录**所有与 *agent-design / agent-framework* 相关的知识、规范与参考实现，并保持长期可演进的结构边界，重点关注：

- **独立性**：协议/规范（Specs）可复用，不绑定某一实现
- **整体性**：从概念 → 架构 → 模式 → 规范 → 参考实现 → 示例/评测，链路完整
- **可维护性**：依赖方向清晰，避免循环依赖与“模块越写越像大杂烩”

> 读者对象：做 agent framework / multi-agent 系统、工具编排、记忆与运行时架构的工程师与架构师。

---

## 快速导航

- 总览与导航入口：[`AGENT.md`](AGENT.md)
- 文档索引：[`docs/index.md`](docs/index.md)
- 架构总览：[`docs/architecture/overview.md`](docs/architecture/overview.md)
- 依赖规则（非常重要）：[`docs/architecture/dependency-rules.md`](docs/architecture/dependency-rules.md)
- 规范入口：[`specs/`](specs/)
- 参考实现入口：[`packages/`](packages/)
- 示例：[`examples/`](examples/)
- 评测：[`evals/`](evals/)

---

## 仓库结构与分层理念

我们将内容分成三类（强约束）：

1) **Docs（叙述性知识）**：解释 *why / how / trade-offs*  
2) **Specs（规范/契约）**：定义 *what*（schema + semantics），应相对稳定、可版本化  
3) **Packages（参考实现）**：实现 *how*（runtime/orchestration/providers/adapters/tools），允许快速迭代但不能污染核心抽象

---

## 目录结构（最新规范）

```text
.
├── README.md
├── AGENT.md
├── CONTRIBUTING.md
├── LICENSE
├── .github/
│   └── workflows/
│
├── docs/                         # 叙述性知识：why/how、权衡、案例
│   ├── index.md
│   ├── concepts/                 # 术语/边界/心智模型
│   ├── architecture/             # 总体架构、依赖规则、ADR
│   │   ├── overview.md
│   │   ├── dependency-rules.md
│   │   └── adr/
│   ├── patterns/                 # 可复用设计模式库（成熟方案）
│   ├── rfc/                      # 提案/草案（未定稿方案）
│   └── playbooks/                # 落地手册：接入、排障、评测、运维
│
├── specs/                        # 跨实现可复用的“契约”（what）
│   ├── message/                  # message schema + role + attachment + semantics
│   ├── events/                   # event types + envelope + tracing fields
│   ├── hooks/                    # hook points + context + 调用语义（可拦截）
│   ├── tool/                     # tool contract（调用、权限、I/O、错误模型）
│   ├── memory/                   # memory contract（provider、retrieval、write policies）
│   ├── state/                    # workspace/state model（快照、回放、合并策略）
│   └── session/                  # session semantics（生命周期、隔离范围、TTL等）
│
├── packages/                     # 参考实现（how），按依赖方向分层
│   ├── core/                     # 最小核心抽象（不依赖具体实现）
│   │   ├── agent/
│   │   ├── policy/
│   │   ├── ports/                # ToolPort/MemoryPort/ChannelPort/GatewayPort/LLMPort...
│   │   ├── types/                # shared domain types（基于 specs 的类型）
│   │   └── errors/
│   ├── runtime/                  # 执行引擎：loop/scheduler/hooks/eventbus/state
│   │   ├── loop/
│   │   ├── scheduler/
│   │   ├── hooks/                # hooks 机制实现（注册、顺序、异常策略、超时等）
│   │   ├── eventbus/             # events 分发实现（publish/subscribe）
│   │   └── state/
│   ├── orchestration/            # 编排层：planner/router/multi-agent
│   │   ├── planner/
│   │   ├── router/
│   │   └── multi_agent/
│   ├── memory/                   # memory providers（db/vector/cache 等）
│   ├── tools/                    # tool implementations（可做插件化）
│   ├── workspace/                # sandbox/fs/artifacts（执行工作区实现）
│   ├── session/                  # session store/ttl/serialization/locking
│   ├── gateway/                  # inbound API（http/grpc/webhook 等）
│   ├── channels/                 # 多渠道接入（slack/discord/cli/ws 等）
│   ├── llm/                      # 模型 provider/adapter（OpenAI/Anthropic/本地等）
│   └── observability/            # logging/tracing/metrics/eval hooks
│
├── examples/                     # 最小可运行示例（单agent/多agent/tool/memory）
├── evals/                        # 评测基准与回归测试用例
└── scripts/                      # 开发脚本：schema 校验、生成器、lint 等
```
