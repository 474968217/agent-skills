# Agent Skills

Claude Code 技能集合。每个子目录是一个独立技能，包含 `SKILL.md`（入口）及可选的 `references/`、`scripts/`、`assets/`。

## 快速开始

首次进入本仓库时，按以下顺序初始化：

1. **安装远程依赖** — 阅读 [`REMOTE_DEPS.md`](REMOTE_DEPS.md)，按表格执行安装命令
2. **验证** — 确认所有依赖版本正确、工具可正常调用

> 日常工作中如需新增远程工具或技能，参考 [`REMOTE_DEPS.md`](REMOTE_DEPS.md) 的安装流程和检查清单。

## 本地技能

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

### 远程技能

以下技能不在本仓库中，需从远程安装。详见 [`REMOTE_DEPS.md`](REMOTE_DEPS.md)。

| 技能 | 来源 | 安装命令 | 触发场景 |
|------|------|---------|---------|
| neat-freak | [khazix-skills](https://github.com/KKKKhazix/khazix-skills/tree/main/neat-freak) | sparse-checkout 或克隆后复制 `neat-freak/` 到本仓库根目录 | 会话结束后的文档与记忆同步审查 |

### 远程工具

以下 CLI 工具不在本仓库中，需全局安装。详见 [`REMOTE_DEPS.md`](REMOTE_DEPS.md)。

| 工具 | 仓库 | 安装命令 | 用途 |
|------|------|---------|------|
| [OpenSpec](https://github.com/Fission-AI/OpenSpec) | `@fission-ai/openspec` | `npm install -g @fission-ai/openspec@latest && openspec init` | CLI 工具 + Skill，提供 `/opsx:explore` `/opsx:propose` `/opsx:apply` `/opsx:archive` |
