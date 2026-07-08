# 远程依赖安装引导

本文档引导 agent 安全、完整地安装项目所依赖的远程工具与技能。

核心原则：**先探查再行动，先验证再信任。**

---

## 安装流程

收到安装请求后，不要直接执行安装命令。按以下步骤走：

### 1. 情报收集

- **确认来源**：fetch 仓库主页，提取 README 中的项目定位、安装方式、前置依赖、许可证、活跃度（最近更新时间 / Star 数）。
- **如果是 npm/pip/cargo 包**：直接查对应 registry（`npm view` / `pip show` / fetch pypi.org）。
- **如果是 Claude Code Skill**：确认其 `SKILL.md` 结构符合规范（`name` + `description` frontmatter）。

### 2. 环境检查

在本地确认前置条件是否满足：
- 系统级依赖（Node.js、Python、Git 等）及版本
- 包管理器是否可用（npm/pnpm/yarn/pip/cargo）
- 磁盘空间是否充足

### 3. 安装执行

- **全局 CLI 工具**：优先用项目推荐的包管理器安装，安装后立即跑 `--version` 验证。
- **Claude Code Skill**：将 skill 目录放入本仓库根目录，确认 `name` 不与现有 skill 冲突。
- **从 GitHub 直接克隆**：`git clone` 到合适位置；如果有构建脚本，先读取再运行，不要盲跑。

### 4. 初始化与配置

- 运行工具的 `init` / `setup` 子命令（如果有）
- 检查生成的配置文件：哪些该提交、哪些该入 `.gitignore`
- 检查是否有环境变量需要设置
- 如果初始化修改了 `CLAUDE.md` / `AGENTS.md`：**先 diff，合并而非覆盖，标注来源段落**

### 5. 验证与收尾

- 版本验证、最小功能烟雾测试
- 向用户报告版本、修改了哪些文件、下一步入口
- 如果是项目级基础设施，追加安装步骤到 `README.md` 或 `docs/setup.md`

### 6. 更新远程依赖

当用户说"更新 XX"时，**不要直接重跑安装命令**。更新可能涉及 breaking changes、迁移步骤、或新的配置要求。

1. **先查远程仓库的更新说明**：
   - 优先找 `CHANGELOG.md`、`RELEASES.md`、GitHub Releases 页面
   - 关注 breaking changes、migration guide、deprecation 标记
   - 如果仓库提供了 `update` 子命令（如 OpenSpec 的 `openspec update`），用它而不是重跑 `install`
2. **对比版本差异**：`<cmd> --version` 查当前版本 → 对比远程最新版本号 → 确认是否有跳版本风险
3. **检查前置依赖变化**：新版本是否提高了 Node.js / Python / 其他依赖的最低版本要求
4. **检查配置文件变更**：更新后本地配置是否需要调整（新字段、废弃字段、格式变更）
5. **执行更新 → 验证**：按远程文档指示操作，完成后跑 `--version` 和烟雾测试
6. **更新 `CLAUDE.md`**：如果工具通过 `update` 子命令刷新了 agent 指令，diff 检查后再合并

---

## 故障排查速查

| 症状 | 常见原因 | 检查方法 |
|------|---------|---------|
| `<cmd>` not found | PATH 未包含全局 bin | `npm config get prefix`，确认 `{prefix}/bin` 在 PATH |
| 权限错误 (EACCES) | Windows 需管理员 / Unix 需 sudo | 管理员终端重试 |
| 版本不满足 | Node.js/Python 版本过低 | `node -v` / `python --version` |
| init 失败 | 不在项目根目录 / 已有同名配置 | `pwd` 确认位置 |
| npm ERR! 404 | 包名错误或 private | `npm search <keyword>` |
| git clone 卡住 | 网络问题 / 需要认证 | 尝试 `--depth 1` |
| `/opsx:*` 命令不触发 | `CLAUDE.md` 未正确合并 | 手动检查 `CLAUDE.md` |

---

## 远程依赖目录

> 以下为本项目依赖的远程工具与技能清单。新增依赖时，按格式追加到对应分类。

### CLI 工具

| 工具 | 仓库 | 安装命令 | 前置要求 |
|------|------|---------|---------|
| [OpenSpec](https://github.com/Fission-AI/OpenSpec) | `@fission-ai/openspec` | `npm install -g @fission-ai/openspec@latest` → `openspec init` | Node.js ≥ 20.19.0 |

**OpenSpec** 是 Spec-Driven Development (SDD) 框架，在人和 AI 之间插入结构化 spec 层。核心命令：

| 命令 | 用途 |
|------|------|
| `/opsx:explore` | 读取代码、形成计划 |
| `/opsx:propose` | 创建结构化变更提案 |
| `/opsx:apply` | 实施提案中的任务 |
| `/opsx:archive` | 归档已完成的变更 |

安装后 `openspec init` 会修改 `CLAUDE.md`，需手动检查合并结果。推荐使用 **expanded** profile（`openspec config profile expanded`）。参见 [官方仓库](https://github.com/Fission-AI/OpenSpec) 获取完整文档。

### Claude Code Skills

<!-- 后续添加的远程 skill 仓库在此列出，格式参考：
| [Skill 名称](repo-url) | 简介 | 安装说明 |
|------|------|---------|
-->

---

## 新增依赖检查清单

每次在本项目中添加新的远程依赖时，完成以下检查项：

- [ ] 已 fetch 项目主页，确认项目定位和活跃度
- [ ] 已确认前置依赖版本并验证本地环境
- [ ] 安装成功且 `--version` 正常
- [ ] 生成的配置文件已审查（提交 vs .gitignore）
- [ ] `CLAUDE.md` / 项目文档的修改已合并且标注来源
- [ ] 已更新本文档的"远程依赖目录"表格
- [ ] 如果是项目基础设施工具，已更新 `README.md`
