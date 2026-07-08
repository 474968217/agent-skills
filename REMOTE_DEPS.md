# 远程依赖安装引导

本文档引导 agent 安全、完整地安装与更新本项目的远程依赖。

核心原则：**先探查再行动，先验证再信任。**

---

## 快速安装

如果用户说"把所有依赖装好"，按以下步骤一次性完成：

1. 先检查环境：`node -v`、`npm -v`、`git --version`
2. 安装 **CLI 工具**：按下方表格逐个执行安装命令
3. 安装 **远程技能**：按下方表格逐个克隆到本仓库根目录
4. 逐个验证：跑 `--version` 或检查目录存在
5. 向用户报告安装结果

---

## 安装规范

### 新增未知依赖时

如果用户要求安装一个本文档中未列出的远程依赖：

1. **fetch 仓库主页**，提取安装方式、前置依赖、许可证
2. **环境检查**：前置依赖版本是否满足
3. **安装 → 验证**：立即跑 `--version` 确认
4. **CLAUDE.md 冲突处理**：如果初始化修改了 `CLAUDE.md`，先 diff 再合并，标注 `<!-- region: 工具名 -->` 来源段落
5. **追加到本文档**：更新下方目录表格，提交 commit

### 更新已有依赖时

当用户说"更新 XX"时，**不要直接重跑安装命令**：

1. **先查远程仓库的更新说明**：`CHANGELOG.md`、Releases 页面、breaking changes
2. 如果仓库提供 `update` 子命令，用它（如 `openspec update`）
3. **对比版本** → **检查前置依赖变化** → **执行更新** → **验证**
4. 如果 `update` 子命令刷新了 `CLAUDE.md`，diff 后合并

---

## 故障排查

| 症状 | 常见原因 | 检查方法 |
|------|---------|---------|
| `<cmd>` not found | PATH 未包含全局 bin | `npm config get prefix` |
| 权限错误 | Windows 需管理员 / Unix 需 sudo | 管理员终端重试 |
| 版本不满足 | Node.js/Python 过低 | `node -v` / `python --version` |
| init 失败 | 不在根目录 / 同名配置已存在 | `pwd` 确认 |
| git clone 卡住 | 网络 / 认证 | `--depth 1` |
| `/opsx:*` 不触发 | CLAUDE.md 未合并 | 手动 diff 检查 |

---

## 远程依赖目录

### CLI 工具

| 工具 | 安装命令 | 前置要求 | 更新命令 |
|------|---------|---------|---------|
| [OpenSpec](https://github.com/Fission-AI/OpenSpec) | `npm install -g @fission-ai/openspec@latest` | Node.js ≥ 20.19.0 | `npm install -g @fission-ai/openspec@latest && openspec update` |

OpenSpec 既是 CLI 工具也是 Skill 集合——安装并 `openspec init` 后，`/opsx:explore` `/opsx:propose` `/opsx:apply` `/opsx:archive` 等命令会作为 skill 注册到 `CLAUDE.md` 中。

### Claude Code Skills

安装远程 skill 到本仓库时，确认其 `SKILL.md` 的 `name` 不与现有 skill 冲突。

| 技能 | 安装命令 | 说明 |
|------|---------|------|
| [neat-freak](https://github.com/KKKKhazix/khazix-skills/tree/main/neat-freak) | `git clone --depth 1 https://github.com/KKKKhazix/khazix-skills.git /tmp/ks && cp -r /tmp/ks/neat-freak ./ && rm -rf /tmp/ks` | 与 task-summary 协同，触发词重叠时 task-summary 优先 |

---

## 新增依赖检查清单

- [ ] 已 fetch 项目主页，确认定位和活跃度
- [ ] 已确认前置依赖版本并验证本地环境
- [ ] 安装成功且验证通过
- [ ] 生成的配置文件已审查（提交 vs .gitignore）
- [ ] `CLAUDE.md` 修改已 diff 合并且标注来源
- [ ] 已更新本文档的目录表格
- [ ] 如果是项目级工具，已更新 `README.md`
