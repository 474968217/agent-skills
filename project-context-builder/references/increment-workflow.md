# 增量更新工作流 (Increment Workflow)

本文件定义模式3（Increment）的详细操作流程。Agent在执行增量更新时遵循此工作流。

---

## 核心原则

**增量优先于全量**——只更新受变更影响的部分，保留其他内容不变。

**每次增量操作必须**：
1. 分析变更影响范围
2. 只修改受影响的文档和章节
3. 在受影响文档的Changelog中添加记录
4. 更新frontmatter中的`last_updated`
5. 检查文档间交叉引用的一致性

---

## 变更影响分析（核心步骤）

当用户描述变更时，Agent按以下矩阵判断影响范围：

### 变更类型 → 影响文档映射

| 变更类型 | 影响的文档 | 典型更新内容 |
|----------|-----------|-------------|
| **新增功能/需求** | REQUIREMENTS.md（必更）<br>PROJECT_INDEX.md（必更）<br>ARCHITECTURE.md（如涉及架构）<br>AGENTS.md（如涉及编码规范） | 新增需求条目（FR-xxx）<br>新增代码路径映射<br>更新架构图/服务列表<br>更新业务上下文描述 |
| **修改功能** | REQUIREMENTS.md（必更）<br>PROJECT_INDEX.md（路径变时） | 更新对应需求条目<br>更新代码路径 |
| **删除功能** | REQUIREMENTS.md（必更）<br>PROJECT_INDEX.md（必更）<br>ARCHITECTURE.md（架构影响时） | 标记为DEPRECATED<br>标记为DEPRECATED<br>移除对应模块 |
| **技术栈升级** | ARCHITECTURE.md（必更）<br>DEVELOPMENT.md（必更）<br>AGENTS.md（必更） | 更新技术栈版本<br>更新环境搭建/命令<br>更新技术栈信息 |
| **新增服务/模块** | ARCHITECTURE.md（必更）<br>PROJECT_INDEX.md（必更）<br>AGENTS.md（必更） | 新增服务/模块描述<br>新增仓库/路径映射<br>新增模块编码规范 |
| **架构调整** | ARCHITECTURE.md（必更）<br>AGENTS.md（必更）<br>PROJECT_INDEX.md（可能） | 更新架构描述<br>更新架构决策/规范<br>更新模块划分 |
| **接口变更** | ARCHITECTURE.md（接口契约）<br>PROJECT_INDEX.md（可能） | 更新接口定义<br>更新入口文件说明 |
| **数据库变更** | ARCHITECTURE.md（数据架构）<br>REQUIREMENTS.md（如影响功能） | 更新数据模型<br>更新相关功能描述 |
| **部署方式变更** | DEVELOPMENT.md（必更）<br>ARCHITECTURE.md（部署架构） | 更新部署流程<br>更新部署拓扑 |
| **团队/角色变更** | Docs-Governance.md（必更）<br>各文档的Owner字段 | 更新角色分工<br>更新Owner信息 |

### 影响分析执行步骤

1. **识别变更类型**: 从用户描述中判断属于上表哪种类型
2. **确定必更文档**: 根据矩阵确定必须更新的文档
3. **评估可选影响**: 判断是否需要更新其他文档（架构影响？功能影响？）
4. **向用户确认**: "这个变更会影响以下文档：REQUIREMENTS.md（新增需求条目）、PROJECT_INDEX.md（新增代码路径）。ARCHITECTURE.md中的服务列表可能也需要更新。是否继续？"

---

## 增量写入操作流程

### Step 1: 读取现有文档

读取所有受影响的现有文档，了解当前内容。

### Step 2: 定位变更点

在文档中找到需要修改的具体位置（章节、表格行、段落）。

### Step 3: 执行修改

**规则**:
- **表格**: 新增行（不要重新生成整个表格）
- **列表**: 新增条目
- **段落**: 修改受影响句子，保留其他内容
- **代码块**: 只修改变化的部分
- **YAML frontmatter**: 更新`last_updated`，必要时更新`owner`、`update_trigger`

### Step 4: 添加Changelog

在每个受影响文档的顶部（YAML frontmatter之后）添加变更记录：

```markdown
## 变更记录 (Changelog)

| 日期 | 变更类型 | 变更内容 | 触发人/原因 | 关联文档 |
|------|---------|----------|-------------|----------|
| {date} | 新增功能 | 新增退货流程，需求FR-010 | 产品经理-退货需求 | REQUIREMENTS.md, PROJECT_INDEX.md |
| {date} | 技术升级 | Spring Boot 2.7 → 3.2 | 技术负责人-版本升级 | ARCHITECTURE.md, AGENTS.md, DEVELOPMENT.md |
```

**变更类型标签**: `新增功能`、`修改功能`、`删除功能`、`技术升级`、`架构调整`、`接口变更`、`数据变更`、`文档优化`、`Owner变更`

### Step 5: 交叉引用一致性检查

检查文档间的引用是否一致：

```
AGENTS.md中的功能列表
  ←→ REQUIREMENTS.md中的功能条目（FR-xxx编号对应）
  ←→ PROJECT_INDEX.md中的代码路径（功能→路径映射）
  ←→ ARCHITECTURE.md中的服务/模块列表

不一致 → 提示用户一并更新
```

### Step 6: 输出变更摘要

向用户汇报本次增量更新的结果：

```markdown
## 更新摘要

本次变更涉及以下文档更新：

| 文档 | 变更内容 | 状态 |
|------|----------|------|
| REQUIREMENTS.md | 新增需求条目FR-010（退货流程） | 已更新 |
| PROJECT_INDEX.md | 新增退货模块代码路径映射 | 已更新 |
| ARCHITECTURE.md | 无直接影响，未变更 | 无需更新 |

### 待确认项
- [ ] 退货流程的异常处理场景（退款失败、商品已发出）— 用户暂未确认

### 建议的后续操作
- 补充退货流程的详细需求（REQUIREMENTS.md FR-010可继续细化）
- 确认退货模块的代码实现路径后更新PROJECT_INDEX.md的映射状态
```

---

## 批量变更处理

当用户描述多个变更时：

1. 分别分析每个变更的影响
2. 合并同类影响（如3个新增功能都影响REQUIREMENTS.md）
3. 按文档分组执行更新（先更新所有对REQUIREMENTS.md的变更，再更新PROJECT_INDEX.md）
4. 最终做一次一致性检查

---

## 示例场景

### 场景1: 新增功能

```
用户: "我们新增了退货功能"

Agent分析:
- 变更类型: 新增功能
- 影响文档: REQUIREMENTS.md（新增需求条目）+ PROJECT_INDEX.md（新增代码路径）
- ARCHITECTURE.md: 如退货是独立服务则更新，否则可能不需要

Agent追问:
1. "退货功能的核心流程是什么？（用户申请→审核→退款→退货入库？）"
2. "这是新增一个退货服务，还是在订单服务里加模块？"
3. "代码放在哪个目录/仓库？"

Agent执行:
- REQUIREMENTS.md: 新增 FR-010 退货流程，填充流程、规则、验收标准
- PROJECT_INDEX.md: 新增退货功能→代码路径映射（如CONFIRMED则填路径，UNKNOWN则标注）
- 两个文档的Changelog各新增一条记录
```

### 场景2: 技术栈升级

```
用户: "我们升级到Spring Boot 3了"

Agent分析:
- 变更类型: 技术升级
- 影响文档: ARCHITECTURE.md + DEVELOPMENT.md + AGENTS.md

Agent执行:
- ARCHITECTURE.md: 更新技术栈表格中的版本号
- DEVELOPMENT.md: 更新环境要求、启动命令、已知问题
- AGENTS.md: 更新技术栈信息、可能的编码规范变化（如Jakarta EE包名）
- 三个文档的Changelog各新增一条记录
```

### 场景3: 架构调整

```
用户: "我们把用户服务拆成了认证服务和用户资料服务"

Agent分析:
- 变更类型: 架构调整
- 影响文档: ARCHITECTURE.md（必更）+ AGENTS.md（必更）+ PROJECT_INDEX.md（必更）
- 可能: REQUIREMENTS.md（如影响功能边界）

Agent追问:
1. "拆分后各自负责什么？认证服务处理登录/注册/Token，用户资料服务处理个人信息？"
2. "新服务的仓库名和地址是什么？"
3. "服务间通信方式有变化吗？"

Agent执行:
- ARCHITECTURE.md: 更新服务列表（删除"用户服务"，新增"认证服务"和"用户资料服务"），更新架构图
- AGENTS.md: 更新模块划分、业务上下文描述
- PROJECT_INDEX.md: 更新仓库映射（删除旧映射，新增两个新映射）
- 三个文档的Changelog各新增一条记录
```
