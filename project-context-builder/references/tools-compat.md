# AI编码工具兼容性指南

本文件列出主流AI编码工具的系统提示词格式要求，Agent根据用户使用的工具选择对应的输出格式。

---

## 工具对照表

| 工具 | 配置文件 | 加载位置 | 格式特点 |
|------|----------|----------|----------|
| Claude Code | `CLAUDE.md` | 项目根目录（向上递归查找） | Markdown，支持@引用 |
| Cursor | `.cursorrules` / `.cursor/rules/*.mdc` | 项目根目录或`.cursor/rules/` | Markdown + YAML frontmatter |
| Windsurf | `.windsurfrules` | 项目根目录 | 纯文本/Markdown |
| GitHub Copilot | `.github/copilot-instructions.md` | `.github/` 目录 | Markdown |
| OpenCode | `AGENTS.md` | 项目根目录（及子目录） | Markdown + YAML frontmatter |
| 通用/其他 | `AGENTS.md` | 项目根目录 | Markdown（本Skill默认输出） |

---

## Claude Code — CLAUDE.md

### 文件位置
- 项目根目录: `./CLAUDE.md`
- 向上递归查找（支持组织级和项目级配置）
- 支持 `@path/to/file.md` 语法引用其他文件

### 格式要求

```markdown
# {Project Name}

{项目概述，2-3句话}

## 常用命令
- Build: {command}
- Test: {command}
- Lint: {command}
- Dev server: {command}

## 项目结构
{目录结构说明}

## 编码规范
{规范说明}

## 架构决策
{关键决策说明}
```

### 特殊语法

- `@./other-file.md` — 引用其他markdown文件，Claude Code会自动读取
- `---` frontmatter可选，用于元数据
- 支持hooks和skills配置（详见Claude Code文档）

### 最佳实践

1. 保持简洁，聚焦AI编码时最需要的信息
2. 按场景组织（"当你做X时，参考Y"）
3. 包含具体的命令行命令，AI可以直接执行
4. 明确文件路径和命名约定
5. 可以引用AGENTS.md: `@./AGENTS.md`

---

## Cursor — .cursorrules / .mdc

### 文件位置

**全局规则**:
- 项目根目录: `./.cursorrules` (旧格式，仍支持)

**项目规则（推荐）**:
- 目录: `./.cursor/rules/`
- 文件: `{rule-name}.mdc`
- 支持glob匹配，可针对特定目录生效

### .mdc 格式要求

```markdown
---
description: {rule_description}
globs: {file_patterns}
alwaysApply: {true/false}
---

# {Rule Title}

{规则内容}

## 示例

### 推荐
```language
{good_example}
```

### 避免
```language
{bad_example}
```
```

### Frontmatter字段

| 字段 | 说明 | 示例 |
|------|------|------|
| `description` | 规则描述，AI用于判断何时应用 | "API endpoint development" |
| `globs` | 匹配的文件模式 | `"src/**/*.ts"` 或 `["*.tsx", "*.jsx"]` |
| `alwaysApply` | 是否始终应用 | `true` 或 `false` |

### 最佳实践

1. 将规则按主题拆分多个`.mdc`文件（如api-rules.mdc、component-rules.mdc）
2. 使用globs精确控制规则的应用范围
3. 每个规则文件聚焦一个主题，避免过大
4. 包含具体的代码示例（推荐vs避免）
5. 使用描述性文件名

---

## Windsurf — .windsurfrules

### 文件位置
- 项目根目录: `./.windsurfrules`
- 单个文件，不支持多文件拆分

### 格式要求

```markdown
# {Project Name} Rules

## 技术栈
- Language: {language}
- Framework: {framework}
- Testing: {testing_framework}

## 编码规范
1. {rule_1}
2. {rule_2}

## 项目结构
{structure_description}

## 特殊规则
{special_instructions}
```

### 最佳实践

1. Windsurf规则以纯文本/Markdown解析，不使用YAML frontmatter
2. 规则按优先级排列，越靠前的越重要
3. 使用简洁的指令风格
4. 包含技术栈版本信息
5. 明确Cascade（Windsurf Agent）的特殊行为要求

---

## GitHub Copilot — copilot-instructions.md

### 文件位置
- `.github/copilot-instructions.md`

### 格式要求

```markdown
# {Project Name} Instructions

## 项目概述
{overview}

## 编码规范
{conventions}

## 常用模式
{patterns}

## 避免的做法
{anti_patterns}
```

### 最佳实践

1. 相对简洁，Copilot的上下文窗口处理方式与其他工具不同
2. 聚焦编码模式和常见模式
3. 包含反模式（明确告诉AI不要做什么）
4. 与Copilot的代码补全功能配合使用

---

## AGENTS.md（通用格式）

### 文件位置
- 项目根目录: `./AGENTS.md`
- 子目录也可放置，实现分层配置

### 格式要求（本Skill默认输出）

见 `output-templates.md` 中的"Agent系统提示词"模板。

### 设计原则

1. **单一信源**: AGENTS.md是真实文件（不是symlink），Claude Code的CLAUDE.md通过`@./AGENTS.md`引用它
2. **分层配置**: 子目录可有自己的AGENTS.md，实现模块级配置
3. **元数据驱动**: YAML frontmatter包含last_updated、scope等元数据
4. **渐进式披露**: AI先读frontmatter判断是否需要继续读取
5. **跨工具兼容**: 大多数工具原生支持或可以适配

### 与其他工具的协作

```text
AGENTS.md          <- 源文件（source of truth）
  ├── CLAUDE.md    <- 包含 "@./AGENTS.md"（import）
  ├── .cursorrules <- 可同步规则子集
  └── .windsurfrules <- 可同步规则子集
```

---

## 工具选择决策树

Agent在输出前询问用户（或推断）：

```
用户使用什么AI编码工具？
├── Claude Code → 输出 CLAUDE.md + AGENTS.md
├── Cursor → 输出 .cursor/rules/*.mdc + AGENTS.md
├── Windsurf → 输出 .windsurfrules + AGENTS.md
├── GitHub Copilot → 输出 AGENTS.md + .github/copilot-instructions.md
└── 不确定/多个工具 → 输出 AGENTS.md + 格式转换说明
```

如果用户未指定，默认输出 `AGENTS.md`（通用格式），并提供其他格式的转换指引。
