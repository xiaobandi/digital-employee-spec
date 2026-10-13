# 资产包目录结构规范（employee-package-spec）

> 每个数字员工 = 一个独立 Git 仓库 = 一个自包含资产包。
> 本规范定义必需文件、目录语义与命名规则。

## 1. 仓库命名

```
de-<客群>-<能力 slug>
```

客群取值（三选一）：`indie`（独立开发者）/ `creator`（自媒体从业者）/ `ecommerce`（电商商家）/ `local`（本地生活服务商家）。

示例：`de-indie-coldstart-growth-officer`、`de-local-review-firefighter`。

## 2. 必需文件（根目录）

| 文件 | 必需 | 说明 |
|------|------|------|
| `README.md` | ✅ | 门面层：演示 GIF + 效果对比图 + 30 秒上手 + 验证状态 |
| `employee.yaml` | ✅ | 机器契约（能力清单 + 依赖锁），Schema 见 [`../schemas/employee.schema.json`](../schemas/employee.schema.json) |
| `employee.md` | ✅ | 人读定义卡（12 主字段 + 2 扩展组），由 employee.yaml 生成，**禁止手工编辑** |
| `dependencies.yaml` | ✅ | 对 `agent-skills-core` 的外部依赖（含版本锁） |
| `CHANGELOG.md` | ✅ | 变更日志 |
| `NOTICE` | ✅ | 版权归属与第三方商标声明 |
| `LICENSE` | ✅ | 许可 |
| `CONTRIBUTING.md` | ✅ | 贡献说明 |

## 3. 目录语义

```
<repo>/
├── employee.yaml          # 机器契约（能力清单 + 依赖锁）
├── employee.md            # 人读定义卡（由 employee.yaml 生成）
├── dependencies.yaml      # 对 core 的外部依赖（含版本）
├── persona/               # 运行提示词 / 边界 / KPI
│   ├── system-prompt.md   #   可直接装载的系统提示词
│   ├── boundary.md        #   做 / 不做边界
│   └── kpi.md             #   可量化目标
├── skills/                # 私有原子技能
│   └── <skill-slug>/
│       ├── SKILL.md       #   技能定义（frontmatter + 正文）
│       ├── scripts/       #   确定性 Python 脚本（可选，必须带 --selftest）
│       └── docs/          #   技能级文档 + 实测记录
├── spec-bindings/         # 有意不公共化能力的本地绑定声明
├── workflows/             # 本员工工作流
│   └── <wf-slug>/
│       ├── SKILL.md       #   工作流定义（触发方式 + 调用链 + 步骤）
│       ├── scripts/       #   编排脚本（可选）
│       ├── docs/images/   #   00-effect.png 效果对比图 / 08-demo-run.gif|mp4 录播
│       └── docs/          #   工作流级文档 + 使用示例
├── kb/                    # 知识库 RAG 契约
├── connectors/            # 连接器绑定（只读为主，写操作人工确认）
│   └── bindings.yaml      #   连接器绑定清单
├── reports/               # 实测台账与最近校验报告
│   └── real-run/          #   真实执行记录（输入清单 + 逐步产出 + 质检）
├── docs/                  # 员工级发布文档（六类）
├── tests/                 # 自动化多场景测试
│   ├── run.py             #   六套件运行器（自包含，零第三方依赖）
│   └── cases.yaml         #   场景用例（期望 = 可执行约束）
└── tools/                 # 仓内工具
```

## 4. 硬性规则

1. **技能/工作流目录名 == frontmatter `name`**，structure 套件强制校验。
2. **不做重复物理存放**：同一技能要么在 `agent-skills-core`（共享），要么在本仓 `skills/`（私有），不允许两处都有——contract 套件强制校验。
3. **README 相对链接必须可达**：structure 套件逐条校验。
4. **效果证据必须真实**：README 的效果对比图取自主场景工作流的实测记录
   （`00-effect.png`，左输入 / 右产出），不得用环境探测截图充当效果。
   尚无实测记录时如实说明，不放示意图。
5. **演示录播与台账同源**：`08-demo-run.gif/mp4` 与 `reports/real-run/<slug>.md`
   来自同一次真实执行，台账不存在则录屏不应存在。
6. **文档里写出来的命令必须跑得通**：doc 套件照抄执行（路径可达 + cwd 明确）。

## 5. 机器契约（employee.yaml）最小示例

```yaml
apiVersion: digital-employee/v1
kind: DigitalEmployee
metadata:
  id: de_indie_coldstart_growth_officer
  name: 出海冷启动获客官
  alias: Build-in-Public 内容引擎
  repo: de-indie-coldstart-growth-officer
  card_no: 4.4.1
  group: indie
  group_label: 独立开发者
  phase: P0
  complexity: M
  score: 23
  price: ¥99-249/月
  channels: [GitHub, 小红书, Coze扣子商店, 腾讯WorkBuddy Skill市场]
  license: Apache-2.0
  version: 1.0.0
spec:
  positioning: 产品冷启动期的"增长合伙人"
  users: 冷启动期 3-6 个月的 C 端/PLG 产品独立开发者
  boundary: "做：选题挖掘、多平台改写、排期与复盘；不做：代刷量、付费投放"
  kpi: 周均内容产出 ≥5 条
  roi_summary: 月省约 45h × ¥80 ≈ ¥3600 vs 订阅成本，回报比 >14 倍
  demand_refs: [D28, D01, D11, D22]
  demand_support: 充分
  demand_evidence: 中
runtime:
  system_prompt: persona/system-prompt.md
  boundary: persona/boundary.md
  kpi: persona/kpi.md
deliverables:
  skills:
    - slug: community-hotpost-digest
      name: 社区热帖摘要
      kind: private
      path: skills/community-hotpost-digest
      version: 1.0.0
      maturity: implemented
  workflows:
    - slug: wf-indie-coldstart-topic-mining
      name: 选题挖掘
      path: workflows/wf-indie-coldstart-topic-mining
      version: 1.0.0
      phase: P0
      calls: [community-hotpost-digest, painpoint-sellingpoint-match]
  knowledge_base: kb/
  connectors: connectors/bindings.yaml
external_dependencies:
  - repo: agent-skills-core
    version: 1.0.0
    skills: [brief-generator, platform-format-adapter, scheduled-dispatch, title-generator]
    resolution: external
spec_only_dependencies: []
```

完整 Schema：[`schemas/employee.schema.json`](../schemas/employee.schema.json)。
