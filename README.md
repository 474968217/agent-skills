# Agent Skills

本仓库是 agent 自举部署的技能集合。agent 拿到此仓库后，目标单一：**准备好所有需要的技能**。

- **本地技能**：仓库内保留完整文件（`SKILL.md` + `references/`、`scripts/`、`assets/` 等），即装即用
- **远程技能**：不落盘到本仓库（远端会更新），仅在 `REMOTE_DEPS.md` 中保留索引（源仓库地址 + 拉取命令），agent 自行按索引拉取

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

## 远程技能

以下技能不在本仓库中，需从远程安装。详见 [`REMOTE_DEPS.md`](REMOTE_DEPS.md)。

| 技能 | 来源 | 触发场景 |
|------|------|---------|
| [neat-freak](https://github.com/KKKKhazix/khazix-skills/tree/main/neat-freak) | [khazix-skills](https://github.com/KKKKhazix/khazix-skills) | 会话结束后的文档与记忆同步审查 |
| [leader](https://github.com/KKKKhazix/khazix-skills/tree/main/leader) | [khazix-skills](https://github.com/KKKKhazix/khazix-skills) | 一句话想法拆成独立目标任务书，输出可直接粘贴到 `/goal` 执行 |

## 远程工具

以下 CLI 工具不在本仓库中，需全局安装。详见 [`REMOTE_DEPS.md`](REMOTE_DEPS.md)。

| 工具 | 仓库 | 安装命令 | 用途 |
|------|------|---------|------|
| [OpenSpec](https://github.com/Fission-AI/OpenSpec) | `@fission-ai/openspec` | `npm install -g @fission-ai/openspec@latest && openspec init` | CLI 工具 + Skill，提供 `/opsx:explore` `/opsx:propose` `/opsx:apply` `/opsx:archive` |