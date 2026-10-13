# 员工定义卡规范（employee-card-spec）

> `employee.md` 是员工的人读快照：**12 主字段 + 2 扩展组**，全部数据来自
> `employee.yaml` 与 `content/registry/*.yaml`，由 `tools/upgrade_facade.py`
> 自动生成。**禁止手工编辑定义卡**——改 `employee.yaml` 后重跑工具刷新。

## 设计原则

1. **机器契约与人读快照分离**：`employee.yaml` 是唯一事实源（可 Schema 校验、可编排）；
   `employee.md` 是零漂移的投影。两处手工各改一份必然漂移，所以只允许机器生成。
2. **面向三读者**：潜在用户看 A–D（它是什么、能干什么、多少钱）；
   平台接入方看 E–H（怎么装载、怎么编排）；审阅者看 I–J（依据是否可信）。

## 12 主字段

### A. 身份定义

| 字段 | 来源 | 说明 |
|------|------|------|
| 名称 | `metadata.name` | 正式名称 |
| 别名 / 旧名存档 | `metadata.alias` | 记录历史命名，避免改名后混淆 |
| 一句话定位 | `spec.positioning` | 替用户解决什么，一句话说清 |
| 目标用户 | `spec.users` | 用户画像 |
| 所属客群 | `metadata.group_label` | indie / creator / ecommerce / local |
| 仓库 | `metadata.repo` | GitHub 仓库名 |
| 定义卡编号 | `metadata.card_no` | 调研报告定义卡编号（如 4.4.1），需求溯源锚点 |
| 版本 | `metadata.version` | 语义化版本 |
| 阶段 | `metadata.phase` | P0 首发 / P1 迭代 / P2 规划 |
| 复杂度 | `metadata.complexity` | S/M/L 及其构成说明（连接器、人日估算） |

### B. 角色边界（做 / 不做）

两栏对照表，来源 `spec.boundary`（格式：`做：…；不做：…`）。
**不做列是红线的一部分**：它声明本员工拒绝执行的动作（如代刷量、付费投放、
虚假人设包装），与技能 SKILL.md 的「禁止事项」、compliance 套件的校验共同构成三层防线。

### C. 目标与 KPI

来源 `spec.kpi`。必须是**可量化**指标（频率、百分比、时延），不许写「提升效率」这类空话。

### D. 工程与商业属性

| 字段 | 来源 | 说明 |
|------|------|------|
| 评分 | `metadata.score` | 高频/痛点/客单价/广度/价值 五维评分合计 |
| 定价 | `metadata.price` | 月订阅区间或按次计费，含调研出处 |
| 投入产出（ROI） | `spec.roi_summary` | 节省时长 × 时薪 ≈ 月省金额 vs 订阅成本 |
| 分发渠道 | `metadata.channels` | GitHub / 小红书 / Coze / WorkBuddy Skill 市场 等 |

### E. System Prompt

来源 `content/registry/employees.yaml` 的 `system_prompt` 字段（与
`persona/system-prompt.md` 同源）。展示可直接装载的员工级系统提示词，
声明数据纪律（只用用户提供的材料、拒绝编造、人工确认环节）。

### F. 工作流清单

Mermaid 工作流树 + 明细表（工作流 / 名称 / 触发方式 / 阶段 / 调用技能）+ 步骤链路。
触发方式来自 `content/registry/workflows.yaml` 的 `trigger` 字段
（定时 / 人工 / 事件三类）。

### G. 技能清单

- **私有技能表**：slug / 名称 / 成熟度 / 版本（来源 `deliverables.skills`）
- **共享技能表**：slug / 来源仓 / 版本（来源 `external_dependencies`，
  物理上挂载自 `agent-skills-core`）

### H. 知识库与连接器

指向 `kb/`（RAG 契约）与 `connectors/`（绑定清单 + 合规边界：只读为主、
写操作人工确认、只走官方 API）。

### I. 需求溯源

| 字段 | 来源 | 说明 |
|------|------|------|
| 需求编号 | `spec.demand_refs` | 调研报告需求编号（D01…），可回溯到原始需求 |
| 需求支持度 | `spec.demand_support` | 充分 / 部分 / 待验证 |
| 证据置信度 | `spec.demand_evidence` | 高 / 中 / 低 |

### J. 可信三字段（扩展组 1）

| 字段 | 值 |
|------|-----|
| 需求支持度 | 同 I |
| 证据置信度 | 同 I |
| 使用边界 | 指回 B 段 |

**为什么单列**：审阅者第一眼需要知道「这个员工的依据有多硬、边界在哪」，
而不必读完全文。这是自我声明机制——置信度标「低」不是缺陷，**谎报才是**。

### 分发与调度（扩展组 2）

A 段的 `分发渠道` 与 F 段的 `触发方式` 已覆盖 bangwozuo 式定义卡的
「触发调度与发布端点」职责，不再单列一节。

## 生成与校验

```bash
python3 tools/upgrade_facade.py            # 全量刷新 30 仓
python3 tools/upgrade_facade.py --only de-xxx
python3 tools/upgrade_facade.py --dry-run  # 只看将发生什么
```

工具幂等：重复运行产出逐字节一致（除 `dry-run` 外直接写盘）。
定义卡中的相对链接（`persona/system-prompt.md`、`kb/` 等）由
structure 套件的链接可达校验兜底——写错路径会在 `tests/run.py` 当场失败。

最小示例：[`examples/employee-card.example.md`](../examples/employee-card.example.md)。
