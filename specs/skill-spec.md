# 原子技能规范（skill-spec）

> 原子技能 = `layer: L1-skill`。技能即执行体：一个目录、一份 SKILL.md、
> 可选的确定性脚本。本规范定义 frontmatter 必需字段与正文必需段落。

## 1. 目录与命名

```
skills/<skill-slug>/
├── SKILL.md        # 技能定义（必需）
├── scripts/        # 确定性脚本（可选）
│   └── *.py        #   每个脚本必须支持 --selftest
└── docs/           # 技能级文档（含实测记录）
```

- 目录名 == frontmatter `name`（structure 套件强制）。
- slug 用小写连字符（`community-hotpost-digest`）。

## 2. SKILL.md frontmatter 必需字段

```yaml
---
name: community-hotpost-digest        # == 目录名
description: >-                       # 触发词齐全：做什么 + 何时触发（中英关键词）
  聚合社区热帖并摘要为选题与洞察素材。当用户提到热帖摘要、社区热点、
  hotspot digest 时使用——即使用户没有直接说出上述关键词，只要意图吻合也应触发。
license: Proprietary                  # 或 MIT / Apache-2.0
compatibility: >-                     # 跨平台声明：需要什么环境、降级路径
  需要能读取工作区文件。若环境支持执行代码，将额外启用 scripts/ 路径。
  跨 WorkBuddy / ChatGPT / DeepSeek Harness / Kimi Work / 豆包工作。
metadata:
  repo: de-indie-coldstart-growth-officer   # 归属仓
  owner: de_indie_coldstart_growth_officer  # 归属员工 ID（共享技能为 PLATFORM）
  delivery: employee                        # employee | core
  visibility: private_to_employee           # private_to_employee | platform_shared
  author: PLATFORM
  version: "1.0.0"
  standard: agent-skills/v1
  library: 本平台资产库
  layer: L1-skill
  complexity: M                             # S | M | L
  phase: P0
---
```

## 3. 正文必需段落

| 段落 | 必需 | 说明 |
|------|------|------|
| `# 标题` + 一句话说明 | ✅ | 用 `description` 首句 |
| `## 输入` | ✅ | 必需 / 可选输入；**缺必需输入必须「明确告诉用户缺什么，不要猜」** |
| `## 环境探测与降级` | ✅ | 能力探测表（首选路径 / 降级路径）；不假设平台专有工具名 |
| `## 输出` | ✅ | 产出物与格式 |
| `## 禁止事项` | ✅ | 红线（❌ 列表），必须非空 |
| `## 合规` | ✅ | AI 标识说明、人工审核要求 |

结构套件与 `audit_skill_spec.py` 逐条校验段落齐备。

## 4. 提示词纪律

1. **可执行约束**：每条场景期望（`tests/cases.yaml`）必须对应正文里一条
   可找到的约束文本——这是 scenario 套件不开 LLM 就能跑的原理。
2. **数值纪律**：输出里每个数字必须能在输入材料里找到出处；缺的写 `<待填>`
   并给出获取方式。**禁止自己算术推导后当作原始数据输出**（行为级验证
   的 derived 申报机制专查这类漂移）。
3. **降级路径显式声明**：无代码执行环境时怎么走，必须写成表格，不能只写在
   口径说明里。

## 5. 脚本规范

- 每个脚本必须内嵌 `--selftest`（自测随脚本分发，副本漂移当场发现）。
- 脚本只依赖标准库 + 声明过的包；不得内嵌 API Key（compliance 套件全仓扫描）。
- 脚本输出落盘到明确的相对路径；不得写绝对路径（compliance 套件校验）。

## 6. 与共享底座的关系

技能要么在 `agent-skills-core`（`visibility: platform_shared`，多个员工挂载），
要么在本仓 `skills/`（`private_to_employee`）。**不允许两处物理存放**——
contract 套件强制校验。私有技能若被 ≥3 名员工需要，应晋升为共享技能并
走 `agent-skills-core` 的版本发布流程。
