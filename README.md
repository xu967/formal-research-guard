# Formal Research Guard

Formal Research Guard 是一个通用 Codex skill，用于科研项目中的正式实验、论文结果、代码变动、数据变动、环境变动、时间戳和回滚记录管理。

这个 skill 的目标是防止把临时实验、调试实验或不完整结果误当作正式论文结果。它不绑定具体项目、数据集、模型、论文模板、实验框架或日志文件名，适用于不同科研代码仓库。

## 核心作用

该 skill 会要求 Codex 默认把科研输出当作正式记录处理。

它会约束 Codex：

- 在执行任务前先读取项目本地规则文件。
- 默认不运行只能用于调试的部分实验。
- 在正式实验前确认论文表格、方法行、数据集、checkpoint、协议、指标、seed、日志、输出路径和回滚方式。
- 为正式结果和项目变动保留时间戳。
- 不把不完整、诊断性或短训练结果写入论文。
- 在项目存在变更日志时，记录代码、论文、数据、环境、实验和回滚变动。

## 适用场景

当 Codex 处理以下任务时，适合使用这个 skill：

- 学术科研代码仓库。
- 机器学习训练或评估。
- 论文结果表格和实验数字。
- LaTeX 论文修改。
- baseline、ablation、fine-tuning 或 evaluation 脚本。
- checkpoint、数据集或环境变更。
- 实验跟踪和回滚记录。

## 不适用内容

这个 skill 不提供训练框架、数据加载器、论文模板或实验运行器。

它是一个流程约束工具，用于帮助 Codex 判断某个实验、结果或变动是否具备正式记录条件，是否可以写入论文，是否需要补充日志、时间戳和回滚说明。

## 安装方式

将仓库 clone 到 Codex skills 目录：

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
git clone git@github.com:xu967/formal-research-guard.git "${CODEX_HOME:-$HOME/.codex}/skills/formal-research-guard"
```

如果使用 HTTPS：

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
git clone https://github.com/xu967/formal-research-guard.git "${CODEX_HOME:-$HOME/.codex}/skills/formal-research-guard"
```

安装后，重新启动 Codex 会话，使 Codex 重新发现 skill 元数据。

## 使用规则

安装后，在科研项目中使用 Codex 时，应遵循以下规则：

1. **先写项目本地规则。** 在目标项目中维护 `Init.md`、`AGENTS.md`、`EXPERIMENT_PLAN.md`、`EXPERIMENT_TRACKER.md`、`CHANGELOG.md` 或项目自己的变更与回滚记录。这个 skill 会优先要求 Codex 读取这些文件。
2. **正式实验要说清楚论文目标。** 如果你要跑正式实验，请在需求中说明目标论文表格、方法行、baseline、数据集、checkpoint、训练协议、评估协议、seed、指标、输出路径和日志路径。
3. **调试实验要明确说明。** 如果你只是想做 smoke、debug、sanity check、pilot 或短训练，请在需求中明确写出“只做调试”或“只做 smoke”。否则 Codex 会默认按正式实验标准检查。
4. **论文结果必须来自完整协议。** 不要要求 Codex 把单 seed、短训练、临时 checkpoint、synthetic substitute 或缺少 baseline 的结果写入论文正式表格。
5. **所有正式记录必须带时间戳。** 实验结果、代码变动、论文变动、数据变动、环境变动和回滚记录都应使用 `YYYYMMDD-HHMM` 形式的时间戳。
6. **每次变动都要有回滚方式。** 代码、论文、数据、环境和实验输出的变更都应记录如何撤销或恢复。
7. **项目本地规则优先。** 如果本 skill 的通用规则和项目内部规则冲突，以项目内部规则为准。

## 推荐提问方式

正式实验请求可以这样写：

```text
请使用 formal-research-guard。我要启动正式实验，用于论文 Table 1 的 Student-MAE baseline 行。
数据集是 XXX，checkpoint 是 XXX，训练协议是 XXX，评估协议是 XXX。
请使用 seed 0/1/2，输出到带时间戳的目录，并记录命令、日志、指标和回滚方式。
```

论文结果写入请求可以这样写：

```text
请使用 formal-research-guard。请先检查这些结果是否满足正式论文写入条件。
如果满足，请写入论文对应表格；如果不满足，请说明缺少哪些条件，不要直接写入。
```

调试请求可以这样写：

```text
请使用 formal-research-guard。这次只做 smoke/debug，不作为正式论文结果。
请仍然保留日志和时间戳，但不要把结果写入论文。
```

## 仓库结构

```text
formal-research-guard/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── formal-records.md
```

`SKILL.md` 包含 skill 的触发描述和核心工作流。

`agents/openai.yaml` 包含界面展示用的 skill 元数据。

`references/formal-records.md` 包含正式实验检查清单、变更日志模板和论文结果写入门槛。

## 预期工作流

当该 skill 触发时，Codex 应先在当前项目中查找本地规则文件，例如：

```text
Init.md
AGENTS.md
README.md
EXPERIMENT_PLAN.md
EXPERIMENT_TRACKER.md
CHANGELOG.md
*_变更与回滚记录.md
```

如果项目本地规则和本 skill 的通用规则冲突，以项目本地规则为准。

在启动正式实验前，Codex 应确认：

```text
论文表格或章节
方法行
baseline 覆盖范围
数据集
checkpoint
训练协议
微调协议
评估协议
seed 列表
指标
输出路径
日志路径
时间戳
回滚方式
```

如果这些正式条件缺失，Codex 应说明缺口，而不是直接启动实验。

## 非正式实验边界

除非用户明确要求调试，否则以下实验不能作为正式论文结果：

```text
smoke
pilot
diagnostic
short schedule
single seed only
temporary checkpoint
synthetic substitute
partial baseline set
```

这些实验可以用于工程排查，但不应写入论文表格或正式结果段落。

## 时间戳规则

每个正式结果和每次项目变动都必须带时间戳。

默认格式：

```text
YYYYMMDD-HHMM
```

时间戳应至少出现在一个持久位置中：

```text
变更日志小节标题
实验日志文件名
输出目录
结果 JSON、CSV 或 JSONL 文件名
checkpoint 目录
论文编译记录
回滚记录
```

没有时间戳的结果或变动不应被视为正式记录。

## 验证方式

如果本机有 Codex skill-creator 的验证脚本，可以执行：

```bash
python /path/to/skill-creator/scripts/quick_validate.py /path/to/formal-research-guard
```

例如，在内置 skill-creator 位于 `~/.codex/skills/.system` 的环境中：

```bash
python ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py "${CODEX_HOME:-$HOME/.codex}/skills/formal-research-guard"
```

期望输出：

```text
Skill is valid!
```

## 更新方式

更新已安装的 skill：

```bash
cd "${CODEX_HOME:-$HOME/.codex}/skills/formal-research-guard"
git pull
```

## 设计原则

这个 skill 保持通用，不写死任何具体项目规则。

项目专属规则应放在目标项目内部，例如：

- `Init.md`
- `AGENTS.md`
- `EXPERIMENT_PLAN.md`
- `EXPERIMENT_TRACKER.md`
- `CHANGELOG.md`
- 项目自己的变更与回滚记录

本 skill 的职责是提醒 Codex 读取这些项目规则，并围绕正式实验、论文结果、时间戳、复现性和回滚记录执行约束。
