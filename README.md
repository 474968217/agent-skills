# Agent Skills

Claude Code 技能集合。每个子目录是一个独立技能，包含 `SKILL.md`（入口）及可选的 `references/`、`scripts/`、`assets/`。

> **入口文档**：[`REMOTE_DEPS.md`](REMOTE_DEPS.md) — 远程依赖安装与更新引导（新增工具/技能时先看这个）。

## 技能索引

### 开发流程

| 技能 | 触发场景 |
|------|---------|
| [task-summary](task-summary/) | 任务完成后的总结与资产沉淀："总结一下"、"收尾"、"这个阶段做完了" |
| [skill-creator](skill-creator/) | 创建、优化、评估 skill |
| [karpathy-guidelines](karpathy-guidelines/) | 写代码时的行为准则：避免过度设计、只做必须的修改 |

### 架构与设计评审

| 技能 | 触发场景 |
|------|---------|
| [grill-me](grill-me/) | 方案压力测试："grill me"、想让 agent 反复追问设计方案 |
| [grill-with-docs](grill-with-docs/) | 结合项目文档的方案评审，评审同时更新 CONTEXT.md / ADR |
| [project-context-builder](project-context-builder/) | 项目文档体系建设：初始化、深入分析、增量更新、健康检查、代码同步 |

### 文档生成

| 技能 | 触发场景 |
|------|---------|
| [docs-page](docs-page/) | 三栏技术文档页："技术文档"、"API文档"、"docs page" |
| [topo-page](topo-page/) | 知识图谱/架构图页面生成 |

### 维护

| 技能 | 触发场景 |
|------|---------|
| [neat-freak](neat-freak/) | 会话结束后的文档与记忆同步审查："sync up"、"整理文档"、"整理一下" |
