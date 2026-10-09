# 工程技能库

本仓库的核心技能源自 [Matt Pocock 的 skills 仓库](https://github.com/mattpocock/skills)，维护可组合的工程技能、项目配置样板与 Agent Notes 工具。

## 工程推进

当前入口是 [AGENTS.md](./AGENTS.md) 的“工程工作流”。默认 Discuss，讨论收敛后按 [GATES.md](./docs/agents/GATES.md) 的 Steps 推进，由用户明确发出命令：

| 命令 | 作用 | 结束位置 |
| --- | --- | --- |
| `/planning` | 加载 planning，在工作项、按需独立 Spec 与 Agent Notes 中收敛范围和决定，并完成规划评审 | 回到 Discuss，等待下一条命令 |
| `/implement` | 工作充分定义后加载 implement，完成实施、code-review 和 verify | 已授权范围验证完成，或明确阻塞 |

“确认”“可以”“同意”不是推进命令。命令出现在引用、示例或文档中不构成授权。`/implement` 遇到关键合同缺口时报告 blocker 并建议 `/planning`，不能自动规划。实施中改变范围、合同或重大设计时暂停受影响工作，回到 Discuss，由用户重新决定如何推进。

这些命令是用户授权约定，不是本仓库提供的命令解析器。安装环境负责发现技能和接收用户输入。

GATES 集中维护授权、阶段准入和返回规则，技能维护具体工作方法。模型仍需判断合同是否充分，命令不会填补缺失需求。旧 [engineering-v2.md](./docs/agents/engineering-v2.md) 仅保留为独立方法论参考，不是本仓库当前入口，也不与当前门控规则叠加。

## 接入项目

1. 从 [技能目录](./skills/README.md) 选择完整技能目录，安装到使用环境支持的位置，保留支持文件。跨技能调用使用技能名，不绑定本仓库路径。
2. 在目标项目 AGENTS.md 或 CLAUDE.md 中采用本仓库的讨论规则，并引用部署到项目中的 GATES.md；保留已有适用规则，授权与门控集中维护在该文件中。
3. 按需调用 [setup-matt-pocock-skills](./skills/user-invoked/setup-matt-pocock-skills/SKILL.md)，配置跟踪器、工件归属和领域资料。已有配置优先复用，setup 不授予规划或实施权限。新项目、已有项目补配置与历史归档的提示词见 [setup 使用指南](./docs/setup-matt-pocock-skills.md)。

技能仍按 user-invoked 与 model-invoked 分组存储；目录分组不授予执行权限。planning 和 implement 保留现有目录位置，其调用策略设置为显式选择，具体阶段授权由 GATES.md 的命令规则决定。

| 配置或工件 | 默认位置与用途 |
| --- | --- |
| 讨论与门控入口 | 根级 AGENTS.md 或 CLAUDE.md，讨论收敛后引用 GATES.md |
| 推进授权与门控 | docs/agents/GATES.md，按 Steps 编排工作项推进 |
| 跟踪器 | docs/agents/issue-tracker.md，登记工作项位置、状态和操作 |
| 文档归属 | 目标项目既有文档入口（通常 docs/AGENTS.md），写清当前位置和历史属主 |
| 领域资料 | 根 GLOSSARY-MAP.md 或 GLOSSARY.md，各上下文资料按映射定位 |
| Agent Notes | Planning 中长期技术设计与决定的属主；根 .agents/notes/ 内按已选上下文分目录，只读命令导航目录树 |

多项目配置时，setup 先提出上下文边界、名称和路径，由用户选择需要建立的范围；再分别选择哪些上下文启用 Notes、是否需要公共记录区。已有选择直接复用，新发现项目不会自动加入。记录目录按需创建，原有记录保持原路径。

遵循“一个事实一个家”：Proposal 或 Ticket 拥有本次范围、产品行为与验收、状态和评审/批准事实；跨工作项持续生效的合同按需使用独立 Spec。Planning 的长期技术提案、设计与决定由 Agent Notes 承载。通用技能读取项目配置指定的适用输入；setup 负责当前及历史工件归属的配置。详见 [注册规则](./skills/user-invoked/setup-matt-pocock-skills/artifact-registration.md) 和 [记录部署](./skills/user-invoked/setup-matt-pocock-skills/decision-records.md)。

## 使用示例

讨论时可以提出“比较这两种方案，指出需要我决定的差异”。想正式规划时发出：

```text
/planning 将已选方向整理为必要的合同与验收条件，并完成规划评审。
```

规划完成后不会自动实施。准备实施时，另行发出：

```text
/implement 按已确认工作项实现导出功能，完成审查与验证。
```

需求已充分定义时可以从 Discuss 直接 `/implement`，无需先创建规划产物。只想验收交付物时选择 to-verify；要修复实现缺陷时，先明确修复范围，再用 `/implement` 授权，可同时指定 fix-bug 的专项方法。普通文档治理依 AGENTS.md 的例外处理。

## 选配与依赖

| 技能 | 关联职责 |
| --- | --- |
| planning | 使用自带规划资料与 Plan Review；必要时使用 domain-modeling；交付后回 Discuss |
| implement | code-review 负责实现审查，verify 负责最终验收；一次授权覆盖范围内修正与重验 |
| verify | 合同或重大设计变化回 Discuss，不自动调用 planning |
| tdd | 测试优先执行方式，关联 codebase-design 与 code-review，不另行授予实施权限 |
| playwright-e2e | 按需为浏览器项目配置 Playwright、项目测试规则和首个 E2E spec |
| fix-bug | 使用 diagnosing-bugs 与 code-review，仍遵循项目实施授权 |
| to-verify | 用户手动发起的交付验收；报告结果，不自动修复或切换到实施 |

只选择所需能力，保留相应依赖及支持资源。[技能目录](./skills/README.md) 提供各技能场景和用法。

## 文档维护

AGENTS.md 拥有讨论规则与门控入口；GATES.md 拥有阶段授权、准入和返回规则；技能拥有实际工作方法及完成条件；本 README 说明接入与使用方式。Agent Note 格式与生命周期见 [通用记录规则](./skills/user-invoked/setup-matt-pocock-skills/resources/decision-records/.agents/notes/README.md)。setup 的 resources/decision-records 保存通用资源，不复制本仓库项目历史。
