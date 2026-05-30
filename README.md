# Skill Evaluation

[English](README.md) | [中文](README.zh-CN.md)

`skill-evaluation` is a Codex / OpenSkills skill for creating evidence-based Skill and Agent evaluation reports.

It reads a target `SKILL.md`, local repositories, code, logs, traces, metrics, test artifacts, and reference workflows to understand the evaluation object, build a three-layer model, define metrics, run or review tests, attribute failures, and produce a structured reusable report. It can also compare multiple candidate skills on the same task and explain which one works better, where each one fits, and what gaps remain.

Before formal execution, it first produces an evaluation design draft that covers the three-layer breakdown, goals and metrics, data plan, judging methods and rules, data records, and test flow. Only after confirmation should it proceed to test execution and final report generation.

## Use Cases

- Evaluate whether a new skill is usable and worth keeping.
- Compare two or more candidate skills on the same task.
- Evaluate the capability chain of a multi-step skill.
- Evaluate an agent-like workflow with layered evidence.
- Generate an evaluation report from real logs, test data, or historical artifacts.
- Build reusable datasets with sample sources, reference outputs, and dataset splits.
- Design systematic evaluation scenarios with core scenarios and coverage tags.
- Save evaluation run records, sample records, metric results, and artifact paths.
- Decide whether multi-agent or sub-agent collaboration is needed during testing.
- Evaluate skill routing, invocation readiness, necessity, no-skill baselines, and failure feedback loops.
- Generate candidate run matrices, dimension-by-dimension comparisons, scenario fit recommendations, and sample-level pairwise comparisons.
- Use only three judging modes: rule-based judging, model-based judging, and human judging.
- Reuse valid prior results and rerun only missing or invalid evidence units to reduce token and runtime cost.
- After skill changes, add samples that target the intended improvement and rerun affected optimization samples plus related holdout samples.
- Output reports suitable for Markdown or Feishu / Lark documents.

## Core Method

Evaluation reports use a fixed three-layer architecture:

| Layer | Focus |
| --- | --- |
| Runtime framework | Environment, credentials, permissions, APIs, logs, available events, output files, routing and invocation. Cost and duration are included only when actually collected. |
| Capability chain | Data preparation, toolchain, state handoff, schemas, intermediate artifacts, and process orchestration. |
| Business outcome | Final output quality, relevance, accuracy, false positives / false negatives, user value, and skill necessity. |

Every report should include:

- Evaluation object and evidence scope
- Three-layer breakdown
- Evaluation goals and metrics
- Evaluation design confirmation record
- Scenario catalog, scenario-level split matrix, and coverage tag matrix
- Invocation readiness evaluation
- Necessity / no-skill baseline evaluation
- Multi-agent / sub-agent orchestration decision
- Evaluation data records and storage location
- Test flow and execution results
- Failure points, disagreement samples, and improvement directions
- Reproducible artifacts and references
- LLM model names and versions used in the run
- Judging rules, scoring rubrics, model-judge prompts, and fallback reason records
- Resource efficiency and result reuse records

Multi-candidate comparison reports should also include:

- Candidate skill list and version evidence
- Shared dataset and candidate run matrix
- Dimension-by-dimension winner table
- Scenario fit recommendations
- Sample-level pairwise comparison
- Candidate traces and failure attribution

## Installation

Clone this repository into a skills directory discoverable by Codex / OpenSkills:

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/Scofy0123/skill-evaluation.git ~/.codex/skills/skill-evaluation
```

If your environment uses `.agents/skills`:

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/Scofy0123/skill-evaluation.git ~/.agents/skills/skill-evaluation
```

Restart Codex after installation.

## Usage Examples

```text
Use $skill-evaluation to evaluate this local skill:
/path/to/your-skill/SKILL.md
Run every safe executable test and output a Markdown evaluation report.
```

```text
Use $skill-evaluation with this workflow document and historical logs to evaluate a multi-step skill.
The report should include the three-layer breakdown, invocation readiness, necessity, test results, and improvement directions.
```

```text
Use $skill-evaluation to compare these two PRD-generation skills.
Use a shared dataset, a no-skill baseline, and sample-level pairwise comparison. Explain which one works better, what scenarios each fits, and where the gaps are.
```

```text
Use $skill-evaluation to update this Feishu evaluation report.
Preserve comments and update only the relevant sections instead of replacing the whole document.
```

## Repository Structure

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

## Reference Files

- `evaluation-runbook.md`: Runbook for `RUN-xx` execution steps, including the three-layer architecture, dataset splitting, evidence reuse, and feedback loops.
- `metrics-and-judging.md`: Metric design, judging modes and rules, static evaluation tags, and metadata compatibility checks.
- `report-template.md`: Evaluation report template.
- `feishu-output.md`: Feishu / Lark document creation and partial-update guidance.
- `comparative-evaluation.md`: Same-task comparison method for multiple candidate skills.
- `scenario-design-examples.md`: Scenario catalog examples, counterexamples, and common mistakes.
- `report-examples/`: Finished report examples for single-skill evaluation and multi-skill comparison.

## Design Principles

- Do not fabricate test data. Untested items must be marked as `待补测` or `间接证据`.
- Before marking an item as `待补测`, first check whether it can be measured with available tools, scripts, rule-based judging, historical artifact review, isolated no-skill baselines, Feishu / Lark CLI, or APIs.
- Do not rename untested items as `engineering gap`, `not applicable`, or `not directly measurable` to make them look complete. When something truly cannot be measured, document attempted paths, blockers, and the next step to fill the evidence gap.
- Read the target skill and related code before designing metrics.
- Clarify key evaluation decisions with the user: goals and metrics, three-layer breakdown, data sources, dataset construction, human judging criteria, side-effect permissions, and output format.
- Do not run bulk tests, perform online writes, or generate the final report before the evaluation design is confirmed.
- Core test scenarios must come from one coherent taxonomy. Define the scenario catalog name and primary classification dimension first; use language, region, technical topic, industry, and risk as coverage tags.
- Run real tests when an executable entry point exists. If not, use static evaluation, artifact review, or actively constructed substitute judging.
- Inventory existing valid evidence before running new tests. Reuse same-sample, same-input, same-version, same-model results when artifacts and logs are traceable.
- Save script-generated data to reusable table files or databases.
- The judging mode field may contain only `规则评判`, `模型评判`, and `人工评判`.
- Scored metrics must have scoring rubrics. Model-based judging must record prompt files and model versions. Human judging must record concrete review steps.
- Record fallback reason category, trigger point, handling action, and whether the fallback counts toward the quality conclusion.
- Do not create a weighted aggregate score unless the user explicitly asks for one.
- Prefer partial updates for online documents so existing comments are preserved.
