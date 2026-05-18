# AI编码工具兼容性指南

本文件列出主流AI编码工具的系统提示词格式要求,Agent **必须先识别用户使用的工具**,再选择对应的原生输出格式。**默认不写 `AGENTS.md`**。

---

## 工具识别(写文件前的强制步骤)

按以下顺序判断,命中即止:

1. **项目根已存在配置文件** → 沿用该工具格式
   - `CLAUDE.md` → Claude Code
   - `.cursorrules` / `.cursor/rules/` → Cursor
   - `.windsurfrules` → Windsurf
   - `.github/copilot-instructions.md` → GitHub Copilot
   - `AGENTS.md` → OpenCode / 跨工具通用
2. **当前会话上下文(系统提示/环境)透露**了运行环境 → 取该工具
3. **用户消息显式声明**("我用 Cursor") → 取声明
4. **以上都没有** → 问用户,**不要静默默认 AGENTS.md**

---

## 工具对照表

| 工具 | 主配置文件 | 加载位置 | 格式特点 |
|------|------------|----------|----------|
| Claude Code | `CLAUDE.md` | 项目根目录(向上递归) | Markdown,支持 `@` 引用 |
| Cursor | `.cursorrules` / `.cursor/rules/*.mdc` | 项目根 或 `.cursor/rules/` | Markdown + YAML frontmatter |
| Windsurf | `.windsurfrules` | 项目根目录 | 纯文本/Markdown |
| GitHub Copilot | `.github/copilot-instructions.md` | `.github/` 目录 | Markdown |
| OpenCode | `AGENTS.md` | 项目根目录(及子目录) | Markdown + YAML frontmatter |
| 跨工具/未知(且用户已确认要通用) | `AGENTS.md` | 项目根目录 | Markdown |

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

## AGENTS.md(跨工具单一信源,非默认)

**何时使用**:
- 团队混用多个 AI 工具(每个工具的原生文件用引用指向 AGENTS.md)
- 当前工具是 OpenCode(原生支持)
- 用户显式要求

**不要**在以下情况静默写 AGENTS.md:
- 当前运行环境就是 Claude Code / Cursor / Windsurf / Copilot 之一(应写各自的原生文件)
- 未识别工具且未问用户

### 文件位置
- 项目根目录: `./AGENTS.md`
- 子目录也可放置,实现分层配置

### 格式要求
见 `output-templates.md` 中的"Agent 系统提示词"模板。

### 设计原则

1. **单一信源**: AGENTS.md 是真实文件(不是 symlink),其他工具的配置文件以引用方式指向它
2. **分层配置**: 子目录可有自己的 AGENTS.md,实现模块级配置
3. **元数据驱动**: YAML frontmatter 包含 last_updated、scope 等元数据
4. **跨工具兼容**: 大多数工具原生支持或可以适配

### 与其他工具的协作

```text
AGENTS.md          <- 跨工具信源
  ├── CLAUDE.md    <- 含 "@./AGENTS.md"(Claude Code 主文件)
  ├── .cursorrules <- 同步规则子集
  └── .windsurfrules <- 同步规则子集
```


---

## 工具选择决策树

```
1. 先执行"工具识别"(本文件顶部) → 得到 {tool}
2. 按 {tool} 产出主文件:
   ├── Claude Code      → CLAUDE.md(若团队也用 AGENTS.md,在 CLAUDE.md 顶部加 @./AGENTS.md)
   ├── Cursor           → .cursor/rules/*.mdc(推荐) 或 .cursorrules
   ├── Windsurf         → .windsurfrules
   ├── GitHub Copilot   → .github/copilot-instructions.md
   ├── OpenCode         → AGENTS.md
   └── 多工具混用       → AGENTS.md(单一信源) + 各工具的引用/转换文件
3. 未识别 → 必须先问用户,不要静默写 AGENTS.md
```

