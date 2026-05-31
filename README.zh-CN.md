# Skill Evaluation

[English](README.md) | [中文](README.zh-CN.md)

`skill-evaluation` 是一个用于生成 Skill / Agent 评估报告的 Codex / OpenSkills skill。

它会根据目标 `SKILL.md`、本地仓库、代码、日志、trace、metrics、测试产物和参考工作法，完成场景与对象理解、三层拆解、指标设计、测试执行或复盘、失败归因，并输出一份结构化、可复用的评估报告。也可以对多个候选 skill 做同题对比，回答哪个更好用、适合什么场景、差距在哪里。

正式执行前，它会先产出评测设计草案，包括三层拆解、目标/指标、数据方案、评判方式与评判规则、数据记录表和测试流程；确认后再进入测试执行和最终报告生成。

## 适用场景

- 评测一个新的 skill 是否可用、是否值得保留。
- 对两个或多个候选 skill 做同题对比评测。
- 评测一个多环节 skill 的能力链路。
- 对接近 agent 的工作流做分层评估。
- 根据真实运行日志、测试数据或历史产物生成评估报告。
- 构建可复用测试数据集，管理样本来源、reference output 和数据集拆分。
- 设计体系化评测场景，区分核心测试场景和语言、技术主题、行业、风险点等覆盖标签。
- 设计并保存评测运行数据表，沉淀评测运行记录、测试样本记录、指标结果记录。
- 在测试流程中判断是否需要 multi-agent / sub-agent 协同执行，并给出编排方案。
- 评估 skill 的可唤起性、存在必要性、无 skill 基线和失败回流。
- 生成候选运行矩阵、候选维度胜负表、场景适配建议和样本级成对比较。
- 固定使用 `规则评判 / 模型评判 / 人工评判` 三类评判方式，并记录评判规则、模型版本、评分细则和 fallback 原因。
- 通过结果复用和增量补跑节约 token 与运行资源，例如存在必要性评测中复用已有有效“有 skill”产物，只补跑无 skill 基线。
- 在修改 skill 后补充命中优化点的数据样本，并重跑受影响优化集和相关保留集，验证优化是否符合预期。
- 输出 Markdown 或飞书文档可用的评测报告。

## 核心方法

评估报告固定使用三层架构：

| 层级 | 关注点 |
| --- | --- |
| 运行框架层 | 环境、凭证、权限、API、日志、可用事件、输出文件、可唤起性；成本或耗时仅在实际采集到时纳入 |
| 能力链路层 | 数据准备、工具链、状态交接、Schema、字幕/正文/中间产物、流程串联 |
| 业务效果层 | 最终任务质量、相关性、准确性、误判/漏判、用户价值、存在必要性 |

每份报告都必须包含：

- 评测对象和证据范围
- 三层拆解
- 评测目标与指标
- 评测设计确认记录
- 评测场景体系表、场景级拆分矩阵和标签覆盖矩阵
- 可唤起性评估
- 存在必要性 / 无 skill 基线评估
- 测试流程中的 multi-agent / sub-agent 协同编排判断
- 评测数据记录表和落盘位置
- 测试流程与执行结果
- 失败点、分歧样本和优化方向
- 可复现产物与参考来源
- 本轮使用的大语言模型及版本
- 评判规则、评分细则、模型评判提示词和 fallback 原因记录
- 资源效率与结果复用记录，包括复用来源、补跑范围和是否计入质量结论

多候选对比报告还必须包含：

- 候选 Skill 清单和版本证据
- 共同数据集与候选运行矩阵
- 候选维度胜负表
- 场景适配建议
- 样本级成对比较
- 各候选 trace 与失败归因

## 安装

将仓库克隆到 Codex / OpenSkills 可发现的 skills 目录中：

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/Scofy0123/skill-evaluation.git ~/.codex/skills/skill-evaluation
```

如果你的环境使用 `.agents/skills`：

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/Scofy0123/skill-evaluation.git ~/.agents/skills/skill-evaluation
```

重启 Codex 后即可使用。

## 使用示例

```text
使用 $skill-evaluation 评测这个本地 skill：
/path/to/your-skill/SKILL.md
请实际运行可安全执行的测试，并输出 Markdown 评估报告。
```

```text
使用 $skill-evaluation，根据这份工作法和历史日志，评测某个多环节 skill。
报告需要包含三层拆解、可唤起性、存在必要性、测试结果和优化方向。
```

```text
使用 $skill-evaluation 对比评测这两个 PRD 生成 skill。
请使用共同数据集、无 skill 基线和样本级成对比较，输出哪个更好用、适合什么场景、差距在哪里。
```

```text
使用 $skill-evaluation，更新这篇飞书评估报告。
请保留评论，做局部章节更新，不要整篇覆盖。
```

## 评测结果报告案例

本仓库 README 提供中英双语版本，以下评测报告案例可直接跳转查看最终报告形态：

- [单一 Skill 评估报告样例：backend-estimation](references/report-examples/单一Skill评估报告样例-backend-estimation.md)
- [多 Skill 对比评估报告样例：两个开源 PRD](references/report-examples/多Skill对比评估报告样例-两个开源PRD.md)

## 目录结构

```text
SKILL.md
agents/openai.yaml
references/
  evaluation-runbook.md
  metrics-and-judging.md
  report-template.md
  feishu-output.md
  comparative-evaluation.md
  scenario-design-examples.md
  report-examples/
    单一Skill评估报告样例-backend-estimation.md
    多Skill对比评估报告样例-两个开源PRD.md
```

## 参考文件

- `evaluation-runbook.md`：供 skill 执行时按 `RUN-xx` 步骤读取的运行手册，覆盖三层架构、数据集拆分、资源复用和闭环沉淀。
- `metrics-and-judging.md`：指标设计、评判方式与评判规则、静态评测标签、metadata 兼容性判断。
- `report-template.md`：评估报告结构模板。
- `feishu-output.md`：飞书文档创建和局部更新规范。
- `comparative-evaluation.md`：多个候选 skill 的同题对比评测方法。
- `scenario-design-examples.md`：评测场景体系示例、反例和常见误区。
- `report-examples/`：成品报告样例，分别覆盖[单一 skill 评测](references/report-examples/单一Skill评估报告样例-backend-estimation.md)和[多个 skill 对比评测](references/report-examples/多Skill对比评估报告样例-两个开源PRD.md)。

## 设计原则

- 不编造测试数据；未测项必须标记为 `待补测` 或 `间接证据`。
- 标记 `待补测` 前必须先主动判断能否用当前工具补测；能通过脚本、规则评判、历史产物复盘、独立会话基线、飞书 / CLI / API 拉取证据完成的项目，不应留空。
- 禁止把未测项改名成 `工程化缺口`、`不适用` 或 `不可直接测得` 来伪装完成；确实无法测量时，说明已尝试路径、阻塞原因和下一步补齐方式。
- 优先读取目标 skill 和相关代码，再设计指标。
- 关键节点必须和用户澄清：评测目标与指标、三层拆解、数据来源、数据集构建、人工评判口径、权限副作用和输出形态都要在确认包中列出已知信息、默认假设和待确认问题。
- 评测设计确认前，不进入批量测试、在线写入或最终报告生成。
- 核心测试场景必须来自同一套分类体系；先确定 `场景体系名称` 和 `主分类维度`，再列同维度、同粒度的核心场景。技术主题、语言地域、行业、风险点等横切因素写成覆盖标签。
- 有可执行入口时尽量真实运行；没有可执行入口时使用静态评测、复盘评测和可主动构造的替代评判。
- 评测前先盘点已有有效证据，优先复用同样本、同输入、同版本、同模型且产物和日志可追溯的结果；只补跑缺失或失效的无 skill 基线、候选结果或评判单元。
- 运行脚本产生的数据必须保存到可复用表文件或数据库。
- `评判方式` 字段只能使用 `规则评判`、`模型评判`、`人工评判`；脚本、Schema、文件、退出码、正则、阈值检查都属于 `规则评判`。
- 分数型指标必须有评分细则；模型评判必须记录提示词文件和模型版本；人工评判必须记录具体评审步骤。
- fallback / 兜底必须记录原因分类、触发环节、处理动作和是否计入质量结论。
- 不默认加权汇总总分，除非用户明确要求。
- 在线文档更新优先局部修改，尽量保留评论。
