# Digital Employee Spec · 小班底数字员工资产规范

> 一句话说明：小班底数字员工生态的总规范仓，定义**资产包怎么写、怎么校验、怎么发布**。
> 全部 30 名数字员工与公共底座 `agent-skills-core` 均对齐本规范。

## 这是什么

每个数字员工是一个**完整的资产包仓库**，包含：

- `employee.yaml` 机器契约（能力清单 + 依赖锁，可校验、可编排）
- `employee.md` 人读定义卡（12 主字段 + 2 扩展组，由 `employee.yaml` 生成）
- `skills/` 原子技能（SKILL.md + scripts + docs）
- `workflows/` 场景工作流（SKILL.md + 触发方式 + 调用链）
- `tests/` 六套件自动化校验（**不需要任何 LLM / API Key**）

## 快速开始

```bash
# 1. 挑一名员工克隆
git clone https://github.com/xiaobandi/de-indie-coldstart-growth-officer.git

# 2. 跑六套件自检，确认环境与契约
cd de-indie-coldstart-growth-officer && pip install pyyaml && python3 tests/run.py

# 3. 读 employee.md 定义卡了解能力边界，然后按 SKILL.md 使用
```

## 核心概念速查

| 概念 | ID 形态 | 说明 | 规范文档 |
|------|---------|------|---------|
| `DigitalEmployeePackage` | `de-<客群>-<能力>` | 完整数字员工资产包仓库 | [employee-package-spec](specs/employee-package-spec.md) |
| `DigitalEmployee` | `de_<客群>_<能力>` | 员工定义（employee.yaml / employee.md） | [employee-card-spec](specs/employee-card-spec.md) |
| `BusinessSkill` | `layer: L1-skill` | 原子业务技能（SKILL.md + scripts） | [skill-spec](specs/skill-spec.md) |
| `ScenarioWorkflow` | `layer: L2-workflow` | 场景工作流（触发方式 + 调用链） | [workflow-spec](specs/workflow-spec.md) |
| 校验体系 | 六套件 | structure / contract / scenario / script / doc / compliance | [test-suite-spec](specs/test-suite-spec.md) |

## 目录结构

```
digital-employee-spec/
├── README.md                            # 本文件
├── specs/                               # 核心规范文档
│   ├── employee-package-spec.md         # 资产包目录结构规范
│   ├── employee-card-spec.md            # 员工定义卡规范（12 主字段 + 2 扩展组）
│   ├── skill-spec.md                    # 原子技能 SKILL.md 规范
│   ├── workflow-spec.md                 # 场景工作流规范
│   └── test-suite-spec.md               # 六套件校验规范
├── schemas/
│   └── employee.schema.json             # employee.yaml 的 JSON Schema
├── examples/
│   └── employee-card.example.md         # 定义卡最小示例
└── CONTRIBUTING.md                      # 贡献规则
```

## 生态地图

| 位置 | 用途 |
|------|------|
| [digital-employees](https://github.com/xiaobandi/digital-employees) | 30 名数字员工总目录 + 依赖图谱 |
| [官网](https://xiaobandi.github.io) | 品牌首页与员工总览 |
| [agent-skills-core](https://github.com/xiaobandi/agent-skills-core) | 公共底座：跨员工共享技能（26 个） |
| 本仓库 | 资产格式总规范（你在这里） |

## 质量承诺

全部 30 名员工的资产包通过四层验证：

| 层 | 内容 | 最近结果 |
|----|------|---------|
| 静态结构 | 目录/链接/YAML/命名 | 7685/7685 |
| 三维校验 | structure × contract × scenario | 245/245 × 3 |
| 端到端 | 30 员工六套件真跑 | 30/30 |
| 行为级 | 固定输入真跑 + 机器复核（零编造/闸门/红线/数值纪律） | 150/150 |

## 贡献方式

见 [CONTRIBUTING.md](CONTRIBUTING.md)。
