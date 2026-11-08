# Physics Lab Report Skill

## Purpose

Use this Skill to manage a full undergraduate basic-physics experiment-report workflow from chapter recognition through pre-lab preparation, real-data checks, formal analysis, and final report delivery.

Detailed execution rules are in:

```text
references/workflow_rules.md
```

User-facing setup and usage instructions are in:

```text
READ.md
```

## Use this Skill when

Use it when the user asks to start or continue a university physics experiment chapter, prepare pre-lab materials, build data-entry spreadsheets, inspect measured data, run “检查数据”, perform formal uncertainty/error/fitting analysis, or generate the formal experiment report.

Semantic triggers include:

```text
根据总规则，开始[实验大章名称]
按实验报告规则开始[实验大章名称]
读取规则并开始[实验大章名称]
开始做[实验大章名称]实验报告
帮我准备[实验大章名称]
```

Do not require an exact phrase when intent is clear.

## Required project inputs

Prefer:

```text
实验报告/
├─ 个人信息.txt
├─ 实验报告要求/
│  ├─ 课程要求文件
│  ├─ 实验讲义（PDF / PPTX / DOCX）
│  ├─ 不确定度资料（可选）
│  └─ 结构模板/
│     └─ 教师正式报告模板
└─ 第XX章_[大章名称]/
```

If inputs are missing, do not invent them. Identify what is missing and continue only where reliable work is still possible.

## Source-of-truth rules

Always read:

```text
references/workflow_rules.md
```

before substantial work.

For conflicts:
- latest and most specific teacher requirement wins;
- report structure follows the teacher/template;
- experiment content follows the current chapter/experiment materials;
- uncertainty methods follow course-specified uncertainty materials;
- instrument parameters follow course materials first, then official manufacturer sources.

Never silently guess through a real conflict.

## Workflow overview

### Step 0 — chapter understanding + user settings

When the user specifies an experiment chapter:

```text
读取个人信息
→ 读取课程要求
→ 定位指定实验大章
→ 提取 / 整理该章资料
→ 阅读统一要求和本章全部实验
→ 建立初步实验任务判定
→ 按实验现实情况推荐“检查数据”初始筛查阈值
→ 输出当前用户信息和自定义设置
→ 等待用户确认 / 修改
→ 用户确认后生成并输出章节状态 Markdown
```

Before confirmation, do not create the final chapter-state Markdown.

After confirmation, create:

```text
第XX章_[大章名称]/
└─ 00_章节状态_[大章名称].md
```

Use `templates/chapter_state_template.md` as the structural reference.

After outputting the chapter-state Markdown, stop by default. Only continue when the user explicitly says something equivalent to:

```text
确认并继续第一步
```

### Screening-threshold logic

Use a two-stage recommendation:

```text
Step 0
→ initial recommendation from realistic conditions of this experiment

After actual instrument precision is known
→ refine/review the recommendation
```

Do not silently change a threshold the user already confirmed.

If the refined recommendation differs materially, show old value, new recommendation, and reason, then wait for confirmation.

The threshold is an internal heuristic screening value, not a formal statistical/metrological criterion, and must not appear in the formal report unless the course explicitly requires disclosure.

### Step 1 — pre-lab preparation

For every experiment the user will actually perform:
- create pre-lab `.tex`;
- compile pre-lab PDF;
- create experiment data-entry `.xlsx`;
- plan required data, units, precision, repetitions, fitting range, and uncertainty inputs;
- lock the teacher-required formal report structure.

### Step 2 — real-data check

Read the real measured data directly.

Check completeness, units, recording precision, repetitions/ranges/steps, physical plausibility, suspicious points, and required instrument parameters.

Do not modify raw measurements, silently delete inconvenient points, produce the final report, or run the full Step 3 analysis early.

Raw measured Excel stays read-only.

### Optional “检查数据”

Only run when explicitly requested.

Use the chapter/experiment's confirmed internal threshold. If instrument precision is missing, pause screening, follow the instrument-parameter workflow, then continue.

After the check, always wait for explicit user confirmation before Step 3.

### Step 3 — formal analysis

Generate/run formal Python analysis. The program must read real measurements from raw Excel, not hardcode them.

Typical outputs:

```text
04_数据处理程序_[实验名称].py
05_数据处理结果_[实验名称].xlsx
06_实验图像_[实验名称]/
```

Perform formal uncertainty analysis, fitting, residual analysis, error analysis, statistics, figures, unified analysis of appended/re-measured data, and a teacher-template-mapped report outline.

Do not write the final report body yet.

### Step 4 — formal report and delivery

Generate the formal report inside the teacher-required structure.

Default data presentation:

```text
完整原始数据 → 附录
正文 → 拟合参数、最终结果、必要图像和分析
```

Teacher/course requirements override this default.

Compile and inspect the PDF, verify numerical consistency, clean temporary files, and provide final artifacts.

## Instrument parameter workflow

When required precision/specification is unavailable:

```text
identify missing parameter
→ explain what it affects
→ ask permission to search online
```

After approval, prefer manufacturer official site/manual/datasheet, then course/lab official sources.

If model + range + official parameter match uniquely, adopt and record it without another interruption.

Ask again only if model/version/range is ambiguous, official sources conflict, parameter meaning is unclear, or only low-confidence sources exist.

Do not modify the original measured Excel.

During Step 2 / “检查数据”, store confirmed parameters in chapter state; use a temporary analysis copy only if needed.

During Step 3, write confirmed parameters into the formal result workbook.

If no reliable parameter can be found, only treat that instrument's uncertainty as zero after explicit user approval and record that it may underestimate uncertainty.

## Chapter-state file

Maintain one main state file per experiment chapter:

```text
00_章节状态_[大章名称].md
```

It is the authoritative recovery point for user settings, thresholds, chapter requirements, experiment task classification, progress, instrument parameters, next step, and final delivery state.

Across conversations, resume from this file rather than guessing from chat memory.

## Prohibited behavior

Never:
- fabricate measured data;
- modify measurements to match theory;
- silently delete inconvenient data points;
- treat simulated data as real measured data;
- overwrite raw experiment evidence;
- guess missing instrument specifications;
- silently change a confirmed screening threshold;
- expose internal screening thresholds in the formal report;
- redesign teacher-required report structure;
- invent experiment phenomena the user did not record;
- create final report text before the workflow allows it.

## Output expectations

Depending on the experiment, outputs may include:

```text
chapter-state Markdown
pre-lab TeX/PDF
experiment data-entry Excel
formal Python analysis
formal results Excel
analysis figures
formal report TeX/PDF
appendices
```

Use detailed naming, cleanup, formatting, and acceptance rules in `references/workflow_rules.md`.

## Recommended first command

```text
根据总规则，开始[实验大章名称]
```
