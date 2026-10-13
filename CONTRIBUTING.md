# 贡献指南

## 修改规范

规范变更 = 生态级破坏性变更。流程：

1. 先在本仓库开 Issue 说明动机与影响面（30 名员工仓是否需要同步改动）
2. 重大变更升版本号：语义化（新增字段 = minor；改字段语义 = major）
3. 平台工具（`tools/build_repos.py` / `tools/upgrade_facade.py` /
   `tools/run_all_tests.py`）与规范同步更新，不允许规范先行、工具滞后

## 新增数字员工

1. 按 [employee-package-spec](specs/employee-package-spec.md) 建仓骨架
2. `employee.yaml` 填全 12 主字段，过 [`schemas/employee.schema.json`](schemas/employee.schema.json)
3. 用 `tools/upgrade_facade.py --only <repo>` 生成定义卡
4. `tests/run.py` 六套件全绿；主场景工作流补真实执行台账与效果证据
5. 提交到 `content/registry/`（employees / skills / workflows 三表登记）

## 修改技能 / 工作流

- 目录名、frontmatter、正文必需段落的约束见 [skill-spec](specs/skill-spec.md)
  与 [workflow-spec](specs/workflow-spec.md)
- 改动后必须重跑 `tests/run.py`；影响产出契约的改动必须写 CHANGELOG

## 红线（不可协商）

- 效果证据必须真实：不得用环境探测截图充当效果对比图
- 数字必须有出处：输出里的数字必须能回指输入材料，缺的写 `<待填>`
- 对外发布动作保留人工确认环节
- 不做代刷量、水军互动、伪装用户口吻
- 仓库内不得出现 API Key 与真实 PII
