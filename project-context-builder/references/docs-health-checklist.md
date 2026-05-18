# 文档健康检查清单 (Docs Health Checklist)

本文件定义模式4（Health-check）的检查项和标准。Agent按此清单逐项检查项目文档的健康状况。

> **关于主提示词文件名**:下文表格里出现的 `AGENTS.md` 代指"项目实际使用的 Agent 主提示词文件"——按 `SKILL.md §0` / `tools-compat.md` 识别的结果替换为 `CLAUDE.md` / `.cursorrules` / `.cursor/rules/*.mdc` / `.windsurfrules` / `.github/copilot-instructions.md` / `AGENTS.md` 中的实际文件。

---

## 检查维度

### 1. 时效性 (Freshness)

| 检查项 | 方法 | 通过标准 | 状态 |
|--------|------|---------|------|
| last_updated距今 | 读取frontmatter，计算天数 | < 90天 | PASS/WARN/FAIL |
| last_reviewed距今 | 读取frontmatter，计算天数 | < review_cycle设定值 | PASS/WARN/FAIL |
| Changelog最新条目 | 读取Changelog，计算天数 | < 90天 | PASS/WARN/FAIL |

**状态阈值**:
- PASS: < 30天
- WARN: 30-90天
- FAIL: > 90天

### 2. 完整性 (Completeness)

| 检查项 | 方法 | 通过标准 | 状态 |
|--------|------|---------|------|
| 功能-代码映射覆盖率 | 统计REQUIREMENTS.md中的功能条目 | 100%核心功能（P0/P1）已映射 | PASS/WARN/FAIL |
| UNKNOWN项数量 | 扫描所有文档中的UNKNOWN标记 | 0个或已标注影响和处理计划 | PASS/WARN/FAIL |
| 文档Owner设置 | 检查各文档frontmatter中的owner字段 | 核心文档（AGENTS/ARCH/REQ/INDEX）均有Owner | PASS/WARN/FAIL |
| 必要frontmatter字段 | 检查last_updated, owner, review_cycle, update_trigger | 均存在且非空 | PASS/WARN/FAIL |
| Changelog存在性 | 检查各文档是否有Changelog章节 | 至少AGENTS.md和REQUIREMENTS.md有 | PASS/WARN/FAIL |

### 3. 一致性 (Consistency)

| 检查项 | 方法 | 通过标准 | 状态 |
|--------|------|---------|------|
| 技术栈描述一致 | 对比AGENTS.md、ARCHITECTURE.md、DEVELOPMENT.md中的技术栈 | 无矛盾 | PASS/WARN/FAIL |
| 服务/模块列表一致 | 对比AGENTS.md和ARCHITECTURE.md中的服务列表 | 完全一致 | PASS/WARN/FAIL |
| 功能条目与代码映射对应 | 对比REQUIREMENTS.md（FR-xxx）和PROJECT_INDEX.md | 每个FR有对应代码路径 | PASS/WARN/FAIL |
| 需求编号连续性 | 检查REQUIREMENTS.md中的FR编号 | 无跳号、无重复 | PASS/WARN/FAIL |
| 仓库地址有效性 | 检查PROJECT_INDEX.md中的仓库地址格式 | URL格式正确 | PASS/WARN/FAIL |

### 4. AI编码友好度 (AI-Readiness)

| 检查项 | 方法 | 通过标准 | 状态 |
|--------|------|---------|------|
| 结构化程度 | 扫描文档中表格/列表/代码块占比 | > 40%内容为结构化数据 | PASS/WARN/FAIL |
| YAML frontmatter完整 | 检查所有项目文档 | 均有完整的frontmatter | PASS/WARN/FAIL |
| 功能-代码可追溯 | 从AGENTS.md的功能描述→PROJECT_INDEX.md的路径 | 可追溯链完整 | PASS/WARN/FAIL |
| 无歧义表述 | 扫描"可能""大概""也许是"等模糊词 | 仅在UNKNOWN标注中出现 | PASS/WARN/FAIL |
| 编码规范可操作性 | 检查AGENTS.md中的规范描述 | 具体到文件/目录/命名模式 | PASS/WARN/FAIL |

### 5. 治理框架健康度 (Governance)

| 检查项 | 方法 | 通过标准 | 状态 |
|--------|------|---------|------|
| Docs-Governance.md存在 | 检查文件存在性 | 存在 | PASS/FAIL |
| 角色分工明确 | 读取Governance文档 | 核心文档有明确Owner | PASS/WARN/FAIL |
| 更新触发器定义 | 读取Governance文档 | 定义了触发条件 | PASS/WARN/FAIL |
| Review机制 | 读取Governance文档 | 定义了Review周期和Checklist | PASS/WARN/FAIL |

---

## 健康评分算法

```
总体状态:
- HEALTHY: 所有维度均为PASS，或仅WARN<3项且无FAIL
- WARNING: 有1-2项FAIL，或WARN>3项
- CRITICAL: 有3项及以上FAIL，或核心文档（AGENTS/ARCH）FAIL

各维度评分:
- 全部PASS → 维度得分 100
- 有WARN无FAIL → 维度得分 70
- 有1项FAIL → 维度得分 40
- 有2项及以上FAIL → 维度得分 0
```

---

## 健康报告模板

```markdown
# 文档健康检查报告

**检查日期**: {date}
**项目**: {project_name}
**检查者**: Agent (project-context-builder skill)
**总体状态**: HEALTHY / WARNING / CRITICAL
**综合评分**: {score}/100

---

## 各维度评分

| 维度 | 得分 | 状态 | 主要问题 |
|------|------|------|----------|
| 时效性 | {score} | HEALTHY/WARNING/CRITICAL | {issues} |
| 完整性 | {score} | ... | {issues} |
| 一致性 | {score} | ... | {issues} |
| AI编码友好度 | {score} | ... | {issues} |
| 治理框架 | {score} | ... | {issues} |

---

## 详细检查结果

### AGENTS.md
| 检查项 | 状态 | 说明 |
|--------|------|------|
| last_updated时效 | PASS/WARN/FAIL | {detail} |
| frontmatter完整 | PASS/WARN/FAIL | {detail} |
| Owner设置 | PASS/WARN/FAIL | {detail} |
| Changelog | PASS/WARN/FAIL | {detail} |
| 技术栈描述 | PASS/WARN/FAIL | {detail} |

（ARCHITECTURE.md、REQUIREMENTS.md等同理）

---

## 发现的问题

### 严重问题 (P0)
- [ ] {issue} — 影响：{impact} — 修复建议：{fix}

### 警告问题 (P1)
- [ ] {issue} — 影响：{impact} — 修复建议：{fix}

### 建议优化 (P2)
- [ ] {suggestion} — 理由：{reason}

---

## 修复计划

### 立即修复（一键执行）
以下问题可由Agent自动修复，是否执行？
- [ ] {fix_item_1}
- [ ] {fix_item_2}

### 需人工确认后修复
- [ ] {fix_item_3} — 需要确认：{question}

### 建议纳入下次迭代
- [ ] {suggestion_1}

---

## 趋势分析

（如有历史检查数据）
| 检查日期 | 综合评分 | 状态变化 |
|----------|----------|----------|
| {date_1} | {score_1} | → |
| {date_2} | {score_2} | ↑/↓ |
| {date_3} | {score_3} | ↑/↓ |

**趋势**: 改善/稳定/恶化

---

*本报告由 project-context-builder skill 生成。建议在Governance文档中记录本次检查结果和修复决策。*
```

---

## 常见问题的自动修复规则

| 问题 | 自动修复方式 |
|------|-------------|
| last_updated过时 | 更新为当天日期 |
| frontmatter缺失字段 | 按模板补充默认值，标注`[UNKNOWN: 待确认]` |
| Changelog缺失 | 添加空的Changelog框架，提示用户后续使用 |
| 技术栈版本号格式不一致 | 统一为最新文档中的版本 |
| 仓库地址格式错误 | 修正URL格式（如补全https://） |
