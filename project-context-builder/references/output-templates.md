# 输出文档模板

本文档定义所有输出文件的标准模板。Agent在实际输出时根据收集到的信息填充，**不编造未确认的内容**。

---

## 模板1: Agent 系统提示词(主文件)

> **写文件前**:按 `SKILL.md §0` / `tools-compat.md` 识别 AI 工具,确定实际文件名(`CLAUDE.md` / `.cursorrules` / `.cursor/rules/*.mdc` / `.windsurfrules` / `.github/copilot-instructions.md` / `AGENTS.md`)。下方的内容骨架对所有工具通用,仅文件名和格式细节按工具调整。

### 通用骨架(填入识别到的目标文件)

```markdown
---
last_updated: {date}
last_reviewed: {date}
review_cycle: "每季度"
owner: "{role_or_name}"
co_maintainers: ["{role_1}", "{role_2}"]
update_trigger:
  - "AI编码工具切换"
  - "技术栈重大变更"
  - "项目结构重构"
  - "新增/删除核心模块"
status: active
---

# {Project Name} — Agent编码指南

## 变更记录 (Changelog)

| 日期 | 变更类型 | 变更内容 | 触发人/原因 |
|------|---------|----------|-------------|
| {date} | 初始化 | 创建项目文档体系 | project-context-builder |

## 项目概述

{一句话描述项目目标和价值}

## 业务上下文

### 目标用户
{用户群体描述}

### 核心场景
{主要使用场景，3-5个bullet points}

### 关键业务流程
{核心链路描述}

## 技术栈

### 后端
- 语言: {language} {version}
- 框架: {framework} {version}
- 数据库: {database} {version}
- 缓存: {cache_solution}
- 消息队列: {mq_solution}
- 其他中间件: {middleware_list}

### 前端
- 框架: {framework} {version}
- UI库: {ui_library}
- 状态管理: {state_management}
- 构建工具: {build_tool}

### 基础设施
- 部署: {deployment_platform}
- 容器: {container_solution}
- 网关: {gateway}
- 监控: {monitoring_stack}

## 架构概览

### 系统架构
{架构图描述，用文字描述模块关系和调用链路}

### 模块划分
{各模块名称、职责、技术栈}

### 数据流
{关键数据流转路径}

## 编码规范

### 通用规则
- {编码风格要求}
- {命名约定}
- {错误处理原则}

### 项目特有约定
- {特定于本项目的规范}
- {常见的陷阱和避免方式}

## 开发指南

### 环境搭建
{本地开发环境配置步骤}

### 常用命令
```bash
# 启动开发环境
{command}

# 运行测试
{command}

# 构建
{command}
```

### 调试技巧
{调试方法、日志位置、常见问题}

## 关键文件与入口

### 项目结构
```
{目录树状图}
```

### 重要文件
| 文件 | 说明 |
|------|------|
| {path} | {description} |
| {path} | {description} |

## 功能-代码映射表（核心追溯链）

**说明：每个功能/需求必须能追溯到对应的代码仓库或代码路径**

### 多仓项目格式
| 功能/需求 | 仓库名 | 仓库地址 | 代码路径 | 状态 |
|-----------|--------|----------|----------|------|
| {function} | {repo_name} | {repo_url} | {code_path} | CONFIRMED/UNKNOWN |

### 单体项目格式
| 功能/需求 | 代码路径 | 关键文件 | 状态 |
|-----------|----------|----------|------|
| {function} | {code_path} | {key_files} | CONFIRMED/UNKNOWN |

### 待确认映射
| 功能/需求 | 原因 | 影响 |
|-----------|------|------|
| {function} | {reason} | {impact} |

## 待补充事项
{标注尚未确认的信息，供后续完善}
```

### 工具特定调整

按识别到的工具,在通用骨架基础上做以下微调:

- **Claude Code (`CLAUDE.md`)**: 可在文件顶部用 `@./AGENTS.md` 引入跨工具信源;支持 `@path` 引用其他文件。详见 `tools-compat.md`。
- **Cursor (`.cursor/rules/*.mdc`)**: 拆成多个主题文件(api-rules.mdc / component-rules.mdc 等),每个文件加 `description` / `globs` / `alwaysApply` frontmatter。
- **Windsurf (`.windsurfrules`)**: 单文件,纯 Markdown,不加 YAML frontmatter,规则按优先级排列。
- **GitHub Copilot (`.github/copilot-instructions.md`)**: 偏简洁,聚焦编码模式与反模式。
- **OpenCode / 多工具混用 / 通用 (`AGENTS.md`)**: 直接使用上方骨架,保留 YAML frontmatter。

---

## 模板2: 架构文档 (ARCHITECTURE.md)

```markdown
---
last_updated: {date}
last_reviewed: {date}
review_cycle: "架构变更时"
owner: "{架构师角色或姓名}"
co_maintainers: ["{技术负责人}", "{开发负责人}"]
update_trigger:
  - "新增/删除服务或模块"
  - "技术栈升级"
  - "架构模式调整"
  - "数据架构变更"
  - "接口契约变更"
status: active
---

# {Project Name} 架构文档

## 变更记录 (Changelog)

| 日期 | 变更类型 | 变更内容 | 触发人/原因 |
|------|---------|----------|-------------|
| {date} | 初始化 | 创建架构文档 | project-context-builder |

## 1. 文档信息

| 属性 | 内容 |
|------|------|
| 项目名称 | {name} |
| 版本 | {version} |
| 编写日期 | {date} |
| 目标读者 | {audience} |

## 2. 业务背景

### 2.1 业务目标
{business_goals}

### 2.2 相关方
{stakeholders}

### 2.3 业务术语表
| 术语 | 定义 |
|------|------|
| {term} | {definition} |

## 3. 架构总览

### 3.1 架构风格
{architecture_pattern}

### 3.2 设计原则
{design_principles}

### 3.3 架构决策记录 (ADR)
| 决策 | 方案 | 原因 | 日期 |
|------|------|------|------|
| {decision} | {choice} | {rationale} | {date} |

## 4. 系统边界

### 4.1 上下文图
```
[外部系统A] --API--> [本系统] --Event--> [外部系统B]
                    |
              --DB--> [Database]
```

### 4.2 外部依赖
| 系统 | 集成方式 | 用途 | 负责人 |
|------|----------|------|--------|
| {system} | {integration} | {purpose} | {owner} |

### 4.3 接口契约
| 接口 | 协议 | 认证 | 用途 |
|------|------|------|------|
| {endpoint} | {protocol} | {auth} | {purpose} |

## 5. 模块设计

### 5.1 模块清单
| 模块名 | 职责 | 技术栈 | 部署单元 |
|--------|------|--------|----------|
| {module} | {responsibility} | {tech} | {deploy_unit} |

### 5.2 模块关系图
{模块间调用关系描述}

### 5.3 关键模块详细设计
#### {ModuleName}
- 职责: {responsibility}
- 核心类/组件: {components}
- 数据流: {data_flow}
- 状态机: {state_machine}
- 特殊说明: {notes}

## 6. 数据架构

### 6.1 数据模型
{ER图或核心实体描述}

### 6.2 数据存储
| 数据类型 | 存储方案 | 访问模式 | 一致性要求 |
|----------|----------|----------|------------|
| {data_type} | {storage} | {access_pattern} | {consistency} |

### 6.3 数据流转
{关键数据在系统中的流转路径}

## 7. 非功能需求

### 7.1 性能
| 指标 | 目标值 | 测试方法 |
|------|--------|----------|
| {metric} | {target} | {test_method} |

### 7.2 可用性
| 指标 | 目标值 |
|------|--------|
| SLA | {sla} |
| RTO | {rto} |
| RPO | {rpo} |

### 7.3 安全
{security_requirements}

### 7.4 扩展性
{scalability_strategy}

## 8. 部署架构

### 8.1 环境拓扑
{环境结构和部署方式}

### 8.2 资源需求
| 环境 | 服务器/容器 | 数据库 | 带宽/存储 |
|------|------------|--------|-----------|
| 生产 | {spec} | {spec} | {spec} |
| 预发 | {spec} | {spec} | {spec} |
| 测试 | {spec} | {spec} | {spec} |

## 9. 风险与对策

| 风险 | 影响 | 概率 | 缓解措施 |
|------|------|------|----------|
| {risk} | {impact} | {probability} | {mitigation} |

## 10. 附录

### 10.1 参考文档
{links}

### 10.2 待确认事项
{open_questions}
```

---

## 模板3: 需求文档 (REQUIREMENTS.md)

```markdown
---
last_updated: {date}
last_reviewed: {date}
review_cycle: "每次迭代"
owner: "{产品经理或业务负责人}"
co_maintainers: ["{技术负责人}", "{测试负责人}"]
update_trigger:
  - "新增需求/功能"
  - "需求变更"
  - "功能删除"
  - "业务流程调整"
status: active
---

# {Project Name} 需求文档

## 变更记录 (Changelog)

| 日期 | 变更类型 | 变更内容 | 触发人/原因 |
|------|---------|----------|-------------|
| {date} | 初始化 | 创建需求文档 | project-context-builder |

## 1. 文档信息

| 属性 | 内容 |
|------|------|
| 项目名称 | {name} |
| 版本 | {version} |
| 编写日期 | {date} |
| 优先级 | {priority} |

## 2. 项目背景

### 2.1 问题陈述
{problem_statement}

### 2.2 目标与收益
| 目标 | 衡量指标 | 当前值 | 目标值 |
|------|----------|--------|--------|
| {objective} | {metric} | {current} | {target} |

### 2.3 范围定义
**范围内:**
{in_scope_items}

**范围外:**
{out_of_scope_items}

## 3. 用户角色

| 角色 | 描述 | 使用频率 | 主要目标 |
|------|------|----------|----------|
| {role} | {description} | {frequency} | {goals} |

### 用户画像
#### {Persona Name}
- **背景**: {background}
- **痛点**: {pain_points}
- **使用场景**: {scenarios}
- **技术熟练度**: {tech_level}

## 4. 功能需求

### 4.1 功能总览
| 功能模块 | 功能点 | 优先级 | 状态 |
|----------|--------|--------|------|
| {module} | {feature} | {P0/P1/P2} | {status} |

### 4.2 功能详细说明

#### FR-{编号}: {功能名称}
**优先级**: {P0/P1/P2}
**角色**: {user_roles}
**前置条件**: {preconditions}
**触发条件**: {triggers}

**正常流程**:
1. {step_1}
2. {step_2}
3. {step_3}

**异常流程**:
- {exception_1}: {handling}
- {exception_2}: {handling}

**业务规则**:
- {rule_1}
- {rule_2}

**验收标准**:
- [ ] {criteria_1}
- [ ] {criteria_2}

**界面原型/说明**:
{ui_description}

## 5. 非功能需求

### 5.1 性能需求
| 场景 | 指标 | 目标值 |
|------|------|--------|
| {scenario} | {metric} | {target} |

### 5.2 安全需求
- {security_requirement}

### 5.3 兼容性需求
- {compatibility_requirement}

### 5.4 可用性需求
- {usability_requirement}

## 6. 数据需求

### 6.1 数据实体
| 实体 | 主要属性 | 数据量预估 |
|------|----------|-----------|
| {entity} | {attributes} | {volume_estimate} |

### 6.2 报表需求
| 报表 | 维度 | 刷新频率 | 受众 |
|------|------|----------|------|
| {report} | {dimensions} | {frequency} | {audience} |

## 7. 集成需求

| 集成系统 | 集成方式 | 数据方向 | 触发方式 |
|----------|----------|----------|----------|
| {system} | {method} | {direction} | {trigger} |

## 8. 实施计划

| 阶段 | 内容 | 交付物 | 时间预估 |
|------|------|--------|----------|
| {phase} | {content} | {deliverables} | {estimate} |

## 9. 附录

### 9.1 术语表
### 9.2 参考文档
### 9.3 待确认事项
```

---

## 模板4: 项目索引 (PROJECT_INDEX.md)

```markdown
---
last_updated: {date}
last_reviewed: {date}
review_cycle: "目录重构时"
owner: "{开发负责人}"
co_maintainers: ["{架构师}", "{新成员导师}"]
update_trigger:
  - "目录结构变更"
  - "新增/删除模块"
  - "新增微服务/仓库"
  - "入口文件变更"
  - "配置文件位置变更"
status: active
---

# {Project Name} 项目索引

## 变更记录 (Changelog)

| 日期 | 变更类型 | 变更内容 | 触发人/原因 |
|------|---------|----------|-------------|
| {date} | 初始化 | 创建项目索引 | project-context-builder |

## 仓库结构

```
{tree_output}
```

## 入口文件

| 类型 | 文件路径 | 说明 |
|------|----------|------|
| 主入口 | {path} | {description} |
| 配置入口 | {path} | {description} |
| API入口 | {path} | {description} |
| 前端入口 | {path} | {description} |

## 关键代码路径

### 业务模块
| 模块 | 路径 | 核心文件 |
|------|------|----------|
| {module} | {path} | {key_files} |

### 基础设施
| 功能 | 路径 | 说明 |
|------|------|------|
| {function} | {path} | {description} |

## 配置文件

| 文件 | 用途 | 关键配置项 |
|------|------|------------|
| {file} | {purpose} | {key_configs} |

## 请求处理链路

```
{request_flow_description}
```

## 数据流链路

```
{data_flow_description}
```

## 功能-仓库映射表（核心追溯链）

**目标：每个功能/需求都能追溯到具体的代码位置**

### 多仓项目格式
| 功能/需求 | 仓库名 | 仓库地址 | 代码路径 | 关键文件 | 状态 |
|-----------|--------|----------|----------|----------|------|
| {function} | {repo_name} | {repo_url} | {code_path} | {key_files} | CONFIRMED/UNKNOWN |

### 单体项目格式
| 功能/需求 | 代码路径 | 关键文件 | 状态 |
|-----------|----------|----------|------|
| {function} | {code_path} | {key_files} | CONFIRMED/UNKNOWN |

### 待确认映射
| 功能/需求 | 备注 | 影响 |
|-----------|------|------|
| {function} | {note} | {impact} |

## 需求-代码追溯表

**从需求文档中的功能编号追溯到代码位置**

| 需求编号 | 需求名称 | 仓库/路径 | 实现文件 | 测试文件 |
|----------|----------|-----------|----------|----------|
| FR-001 | {req_name} | {repo_or_path} | {impl_file} | {test_file} |

## 常见开发任务速查

| 任务 | 操作位置 | 关键文件 |
|------|----------|----------|
| 新增API | {location} | {files} |
| 新增页面 | {location} | {files} |
| 修改数据库 | {location} | {files} |
| 添加配置 | {location} | {files} |
```

---

## 模板5: 开发指南 (DEVELOPMENT.md)

```markdown
---
last_updated: {date}
last_reviewed: {date}
review_cycle: "环境变更时"
owner: "{技术负责人}"
co_maintainers: ["{开发负责人}", "{DevOps}"]
update_trigger:
  - "开发环境变更"
  - "依赖版本升级"
  - "构建/部署流程变更"
  - "新增开发规范"
  - "CI/CD流水线变更"
status: active
---

# {Project Name} 开发指南

## 变更记录 (Changelog)

| 日期 | 变更类型 | 变更内容 | 触发人/原因 |
|------|---------|----------|-------------|
| {date} | 初始化 | 创建开发指南 | project-context-builder |

## 开发环境

### 前置依赖
- {dependency_1}: {version} ({install_instructions})
- {dependency_2}: {version} ({install_instructions})

### 项目克隆与初始化
```bash
{clone_and_setup_commands}
```

### 环境变量配置
| 变量名 | 说明 | 示例值 | 是否必填 |
|--------|------|--------|----------|
| {var} | {desc} | {example} | {required} |

### 数据库初始化
```bash
{db_init_commands}
```

## 日常开发

### 启动服务
```bash
{start_commands}
```

### 运行测试
```bash
# 单元测试
{unit_test_command}

# 集成测试
{integration_test_command}

# E2E测试
{e2e_test_command}
```

### 代码规范
```bash
# 格式检查
{lint_command}

# 自动修复
{fix_command}
```

### Git工作流
- 分支命名: {branch_naming_convention}
- Commit规范: {commit_convention}
- PR模板: {pr_template}
- 合并策略: {merge_strategy}

## 调试指南

### 日志查看
| 服务 | 日志位置 | 日志级别配置 |
|------|----------|-------------|
| {service} | {log_path} | {config_location} |

### 本地调试
- IDE配置: {ide_config}
- 远程调试端口: {debug_port}
- 常用断点位置: {common_breakpoints}

### 常见问题排查
| 现象 | 可能原因 | 解决方案 |
|------|----------|----------|
| {symptom} | {cause} | {solution} |

## 构建与部署

### 本地构建
```bash
{build_commands}
```

### 部署流程
1. {step_1}
2. {step_2}
3. {step_3}

### 环境对照
| 环境 | 地址 | 数据库 | 说明 |
|------|------|--------|------|
| 开发 | {url} | {db} | {notes} |
| 测试 | {url} | {db} | {notes} |
| 预发 | {url} | {db} | {notes} |
| 生产 | {url} | {db} | {notes} |

## 技术决策

### 已确认的技术选型
| 技术 | 选型 | 原因 |
|------|------|------|
| {area} | {choice} | {rationale} |

### 待决策事项
| 事项 | 候选方案 | 状态 |
|------|----------|------|
| {item} | {options} | {status} |
```

---

## 模板6: 文档治理指南 (Docs-Governance.md)

```markdown
---
last_updated: {date}
last_reviewed: {date}
review_cycle: "每季度"
owner: "整个团队（轮转）"
co_maintainers: ["{技术负责人}", "{产品经理}", "{架构师}"]
update_trigger:
  - "新增/删除项目文档"
  - "团队人员变动"
  - "文档Owner变更"
  - "AI编码工具切换"
status: active
---

# {Project Name} 文档治理指南

## 变更记录 (Changelog)

| 日期 | 变更类型 | 变更内容 | 触发人/原因 |
|------|---------|----------|-------------|
| {date} | 初始化 | 创建文档治理框架 | project-context-builder |

---

## 1. 项目文档清单

| 文档 | 路径 | Owner | Review Cycle | 状态 |
|------|------|-------|-------------|------|
| AGENTS.md | 项目根目录 | {owner} | 每季度+工具切换 | active |
| ARCHITECTURE.md | {path} | {owner} | 架构变更时 | active |
| REQUIREMENTS.md | {path} | {owner} | 每次迭代 | active |
| PROJECT_INDEX.md | {path} | {owner} | 目录重构时 | active |
| DEVELOPMENT.md | {path} | {owner} | 环境变更时 | active |

### 文档说明

**AGENTS.md** — Agent编码提示词
- 用途: 为AI编码工具提供项目上下文和编码规范
- 读者: AI编码工具（Claude Code/Cursor/Windsurf等）
- 维护重点: 技术栈准确性、编码规范可操作性、项目结构描述

**ARCHITECTURE.md** — 架构文档
- 用途: 记录系统架构、技术决策、模块关系
- 读者: 开发人员、架构师、新成员
- 维护重点: 架构图准确性、ADR（架构决策记录）及时更新

**REQUIREMENTS.md** — 需求文档
- 用途: 记录功能需求、业务流程、验收标准
- 读者: 产品经理、开发人员、测试人员
- 维护重点: 需求条目完整性、业务流程准确性、功能-代码映射

**PROJECT_INDEX.md** — 项目索引
- 用途: 代码地图，帮助AI和人类快速定位代码
- 读者: AI编码工具、新成员、代码审查者
- 维护重点: 目录结构准确性、功能-代码映射覆盖率、入口文件位置

**DEVELOPMENT.md** — 开发指南
- 用途: 开发环境搭建、常用命令、调试方法
- 读者: 新成员、开发人员
- 维护重点: 环境搭建步骤可用性、命令准确性、常见问题时效性

## 2. 角色与职责

### 文档Owner职责
- 确保文档内容的准确性和时效性
- 在update_trigger触发时及时更新文档
- 定期Review（按review_cycle）并更新last_reviewed
- 指定Co-maintainer在自己缺席时代理

### 文档Owner变更流程
1. 当前Owner在相关文档中更新owner字段
2. 在本文档（Docs-Governance.md）的文档清单中同步更新
3. 新Owner确认接收并了解维护职责
4. 在Changelog中记录变更

### 团队共同职责
- 发现文档错误或过时时，及时在Issue/群里指出
- 代码评审时检查相关文档是否需要同步更新
- 新成员入职后阅读文档并反馈问题

## 3. 文档更新触发器

### 事件→文档映射

| 事件 | 必须更新的文档 | 建议检查的文档 |
|------|--------------|--------------|
| 新增功能/需求 | REQUIREMENTS.md | PROJECT_INDEX.md, ARCHITECTURE.md |
| 修改功能 | REQUIREMENTS.md | PROJECT_INDEX.md |
| 删除功能 | REQUIREMENTS.md, PROJECT_INDEX.md | ARCHITECTURE.md |
| 新增/删除服务或模块 | ARCHITECTURE.md, PROJECT_INDEX.md | AGENTS.md |
| 技术栈升级 | ARCHITECTURE.md, DEVELOPMENT.md | AGENTS.md |
| 接口变更 | ARCHITECTURE.md | PROJECT_INDEX.md |
| 数据库变更 | ARCHITECTURE.md | REQUIREMENTS.md |
| 目录结构重构 | PROJECT_INDEX.md | AGENTS.md |
| 开发环境变更 | DEVELOPMENT.md | - |
| 部署方式变更 | DEVELOPMENT.md, ARCHITECTURE.md | - |
| AI编码工具切换 | AGENTS.md | Docs-Governance.md |
| 团队/Owner变更 | Docs-Governance.md | - |

### 更新流程
1. **识别事件**: 确定变更类型
2. **定位文档**: 查上表确定需要更新的文档
3. **执行更新**: 修改文档内容 + 更新frontmatter + 添加Changelog条目
4. **提交评审**: 通过PR提交，标注`[docs]`前缀
5. **交叉检查**: 确保文档间引用一致

## 4. 定期Review机制

### Review周期
- AGENTS.md: 每季度一次 + AI工具切换时
- ARCHITECTURE.md: 架构变更时（强制）+ 每半年一次（主动）
- REQUIREMENTS.md: 每次迭代结束时
- PROJECT_INDEX.md: 每次大版本发布时 + 目录重构时（强制）
- DEVELOPMENT.md: 环境变更时（强制）+ 每季度一次（主动）
- Docs-Governance.md: 每季度一次

### Review Checklist
每次Review时检查：

- [ ] frontmatter中的last_reviewed已更新
- [ ] 技术栈信息与实际代码一致
- [ ] 功能-代码映射覆盖率100%（核心功能）
- [ ] UNKNOWN项数量在可接受范围（<5个或有处理计划）
- [ ] Changelog已记录上次Review以来的变更
- [ ] 与其他文档的交叉引用一致
- [ ] AI编码友好度（结构化程度、可读性）

### Review执行方式
1. Owner发起Review（可借助Agent的Health-check模式）
2. 逐项检查Review Checklist
3. 发现问题→创建修复任务→执行修复→更新last_reviewed
4. 在Changelog中记录Review完成

## 5. AI编码友好度检查

### 检查项
- [ ] 文档使用Markdown格式
- [ ] 有完整的YAML frontmatter（last_updated, owner, review_cycle等）
- [ ] 结构化内容占比>40%（表格、列表、代码块 vs 长段落）
- [ ] 功能-代码映射完整可追溯
- [ ] 无歧义表述（"可能""大概"仅在UNKNOWN标注中出现）
- [ ] 编码规范具体可操作（到文件/目录/命名模式级别）
- [ ] 变更记录Changelog存在且最新

### 优化建议
- 用表格替代长段落描述
- 用代码块展示具体命令和路径
- 用列表展示可选项和检查项
- 每个功能有唯一编号（FR-xxx）便于引用
- 文档间使用明确链接引用（如"详见ARCHITECTURE.md §3.2"）

## 6. 健康度自评表

团队每季度使用以下表格自评文档体系健康度：

| 维度 | 评分(1-5) | 说明 |
|------|----------|------|
| 时效性 | {1-5} | 文档是否及时更新 |
| 完整性 | {1-5} | 功能-代码映射是否完整 |
| 一致性 | {1-5} | 文档间是否一致 |
| AI友好度 | {1-5} | 是否利于AI消费 |
| 团队参与度 | {1-5} | 团队成员是否参与维护 |
| **总分** | **{/25}** | |

评分标准：5=优秀，4=良好，3=合格，2=需改进，1=严重

## 7. 增量更新工作流

增量更新（新增需求、技术升级等）遵循以下工作流：

1. **变更影响分析**: 确定变更影响哪些文档
2. **定向更新**: 只更新受影响的文档和章节
3. **Changelog记录**: 在每个受影响的文档中添加变更记录
4. **交叉引用检查**: 确保文档间引用一致
5. **变更摘要汇报**: 向团队汇报更新了哪些文档

详见增量更新工作流文档（如有）。

## 8. 新成员入职指引

### 必读文档（按顺序）
1. Docs-Governance.md（本文档）— 了解文档体系和维护规则
2. AGENTS.md — 了解项目技术上下文
3. ARCHITECTURE.md — 了解系统架构
4. PROJECT_INDEX.md — 了解代码结构
5. DEVELOPMENT.md — 了解开发环境

### 入职任务
- [ ] 阅读上述文档并标记不理解的地方
- [ ] 尝试搭建本地开发环境，根据DEVELOPMENT.md操作，反馈问题
- [ ] 完成一个小改动，体验文档更新流程
- [ ] 在团队会议上提出文档改进建议（至少1条）

## 9. 常见问题

### Q: 发现文档过时了怎么办？
A: 如果知道正确信息→直接修改→提交PR。如果不确定→在文档中标注`[UNKNOWN: 待确认]`→通知Owner。

### Q: 文档该放在项目的哪个目录？
A: 推荐 `docs/` 目录或项目根目录。AGENTS.md必须在根目录（AI工具要求）。保持所有文档在同一目录。

### Q: 可以用什么工具辅助维护？
A: 本项目的project-context-builder Skill可随时运行，支持Initialize/Deep-dive/Increment/Health-check/Sync五种模式。

### Q: Owner离职/转岗了怎么办？
A: 按"文档Owner变更流程"执行，及时指定新Owner并更新所有相关文档。

## 10. 待决策/待完善事项

| 事项 | 状态 | 负责人 | 目标日期 |
|------|------|--------|----------|
| {item} | OPEN/IN_PROGRESS/DONE | {owner} | {date} |
```

---

## 使用指南

Agent生成文档时：
1. 根据信息完整度选择填充模板中的相应章节
2. 未收集到的信息保留模板占位符
3. 用户已确认的信息直接填入
4. **用户明确表示"不清楚"的信息，使用标准标注格式**：
   - 行内标注：`{字段} — [UNKNOWN: 用户暂未确认]`
   - 待补充清单条目：`- [ ] {信息项} — 用户暂未确认，影响：{影响说明}`
   - 如果该信息项为空白，保留占位符并标注：`<!-- UNKNOWN: 待后续补充 -->`
5. 文档应保持Markdown格式，使用清晰的层级结构
6. 表格优先于长段落，提高AI可解析性
