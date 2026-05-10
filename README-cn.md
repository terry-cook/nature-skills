# nature-skills
## 技能索引

| 技能 | 状态 | 用途 | 触发关键词 |
|------|------|------|-----------|
| [`nature-figure`](skills/nature-figure/README.md) | 稳定 | 可发表的 matplotlib 图表 | "Nature figure"、"publication plot"、"scientific figure" |
| [`nature-polishing`](skills/nature-polishing/README.md) | 稳定 | 将学术文本润色为 *Nature* 风格 | "Nature style"、"polish"、"academic writing" |
| [`nature-citation`](skills/nature-citation/README.md) | 测试版 | 严格的 Nature / CNS 系列文献检索，支持 ENW、RIS 和 Zotero RDF 导出 | "Nature citation"、"CNS citation"、"text citation"、"supporting references"、"Zotero RDF" |
| [`nature-data`](skills/nature-data/README.md) | 草稿 | Nature 数据可用性声明、数据库计划与 FAIR 检查 | "Data Availability"、"repository"、"FAIR metadata"、"data availability statement" |
| [`nature-response`](skills/nature-response/README.md) | 测试版 | 逐条审稿意见回复信，含意见分类、行动映射与风险检查 | "response to reviewers"、"rebuttal letter"、"major revision"、"审稿意见回复" |
| [`nature-paper2ppt`](skills/nature-paper2ppt/README.md) | 测试版 | 将科学论文转换为中文 PPTX 幻灯片 | "paper PPT"、"journal club"、"paper to slides"、"paper presentation" |

> **新增技能？** 请参阅本文件底部的[贡献指南](#新增技能)。

---

## nature-figure

**功能说明** — 生成符合 *Nature* 期刊视觉标准的多面板 matplotlib 图表：正确的字体排版、语义化调色板、可编辑的 SVG 输出，以及无冗余的面板信息架构。

**示例输出展示** — [`nature-figure` 示例库](skills/nature-figure/README.md#example-output-gallery) 中包含五张密集的模拟 *Nature* 风格结果图，涵盖材料/机制、空间成像、体内疗效、单细胞系统和扰动验证。

**图表类型图谱** — [`nature-figure` 图表图谱](skills/nature-figure/README.md#chart-type-atlas) 对 10 种支持的图表类型进行了分类，包括柱状图、折线图、热图、散点/气泡图、雷达/极坐标图、分布图、森林/区间图、面积/堆叠图、图像板和网络/矩阵布局。

| ![材料设计与物理验证](skills/nature-figure/assets/gallery/fig1-material-mechanism-rich.png) | ![空间成像与摄取](skills/nature-figure/assets/gallery/fig2-spatial-imaging-rich.png) | ![体内疗效与耐受性](skills/nature-figure/assets/gallery/fig3-in-vivo-efficacy-rich.png) | ![单细胞系统图](skills/nature-figure/assets/gallery/fig4-single-cell-systems-rich.png) | ![扰动验证](skills/nature-figure/assets/gallery/fig5-validation-perturbation-rich.png) |
|---|---|---|---|---|

**构建来源** — 基于发表于 *Nature Machine Intelligence* 及顶级机器学习/生物信息学期刊的论文生产脚本（[figures4papers](https://github.com/ChenLiu-1996/figures4papers)）。

**核心规则**

- 三项必填 rcParams 必须始终置于首位：
  ```python
  plt.rcParams['font.family'] = 'sans-serif'
  plt.rcParams['font.sans-serif'] = ['Arial', 'DejaVu Sans', 'Liberation Sans']
  plt.rcParams['svg.fonttype'] = 'none'   # 文本保留为 <text> 节点，而非路径
  ```
- 主输出格式始终为 `.svg`；300 dpi 的 `.png` 作为辅助栅格预览。
- 多面板图遵循三级信息层次：**概览 → 偏差 → 关系**。任意两个面板不得回答相同的科学问题。

**参考文件**

```
skills/nature-figure/
├── README.md
├── SKILL.md
└── references/
    ├── api.md            调色板、辅助函数签名、验证规则
    ├── design-theory.md  排版、布局、导出策略、反冗余规则
    ├── common-patterns.md 超宽面板、图例轴、印刷安全柱状图
    ├── tutorials.md      端到端演练（柱状图、趋势图、热图）
    └── chart-types.md    雷达图、3D 球体、散点图、fill_between、对数刻度
```

**支持的图表类型** — 堆叠柱状图、分组柱状图、水平消融柱状图、趋势/折线图、顺序热图、发散 z 分数热图、气泡散点图、雷达/极坐标图、3D 球体示意图、fill-between 面积图、对数刻度柱状图、GridSpec 多面板图。

---

## nature-polishing

**功能说明** — 将学术草稿（包括中文→英文翻译）转换为符合 *Nature* 期刊规范的文本：句子不超过 30 个词、章节感知时态与语气对冲、精确词汇、正确引用规范，以及英式英语。

**构建来源** — 基于五篇 *Nature* s41586 论文（2026 年）的深入阅读及研究生级科学英语写作课程，提炼出涵盖句子结构、论文框架、词汇、引用完整性、期刊风格和 AI 伦理共 25 条规则。

**核心规则**

| 领域 | 核心规则 |
|------|---------|
| 句子长度 | 每句不超过 30 词；逐句计数；最后一句最易超限 |
| 语气对冲校准 | 依据证据强度匹配主张力度：*demonstrate* → *suggest* → *may reflect* |
| 章节时态 | 结果部分 = 过去时 + 量化细节；讨论部分 = 对冲语气 + 机制阐述 |
| 引用完整性 | 仅引用本人亲自阅读并核实的文献；四种归因类型 |
| 过度主张检测 | 标记绝对化表述、无根据的因果关系、范围扩大、未经证实的"首次"主张 |
| 英式英语 | signalling、colour、analyse、programme、modelling、behaviour |

**12 步润色流程**

句子拆分 → 章节识别 → 沙漏结构检查 → 时态审查 → 句子修改 →
词汇升级 → 模板检查 → 引用审查 → 期刊风格 → 过度主张检查 →
校对 → 纯文本输出

**参考文件**

```
skills/nature-polishing/
├── README.md
└── SKILL.md    25 条规则 + 12 步工作流（由 Claude 自动加载）
```

---

## nature-citation

**功能说明** — 将手稿文本或独立主张转换为严格符合 Nature / CNS 系列的参考文献候选，并导出为参考文献管理器就绪的 `ENW`、`RIS` 或 Zotero `RDF` 格式文件。还可生成用于年份筛选、文献选择和格式专项下载的 HTML 筛选页面。

**构建来源** — 基于 Crossref 元数据检索、DOI 记录导出，以及针对 Nature Portfolio、AAAS Science 系列和 Cell Press 的期刊家族过滤逻辑。

**核心规则**

| 领域 | 核心规则 |
|------|---------|
| 范围过滤 | 限定于 Nature Portfolio、Science 系列、Cell Press 或旗舰期刊 |
| 分段 | 将长文本拆分为可引用的主张单元，分配稳定的段落 ID |
| 检索规范 | 将中文主张翻译为英文科学概念；精准优先于广泛 |
| 支持等级 | 区分强支持、部分支持、背景支持、限制性支持和仅元数据支持 |
| 导出完整性 | 不虚构 DOI、页码、卷号、期号或期刊元数据 |
| 下载选项 | 支持以 `ENW`、`RIS` 或 Zotero `RDF` 单文件导出 |

**参考文件**

```text
skills/nature-citation/
├── README.md
├── SKILL.md
├── references/
│   ├── journal-scope.md
│   ├── ris-endnote.md
│   └── search-strategy.md
└── scripts/
    └── nature_citation.py
```

**示例工作流** — 对段落进行分段，检索范围内文献，在 HTML 浏览器中审查候选文献，然后仅将选定记录下载为 `ENW`、`RIS` 或 Zotero `RDF`。

---

## nature-data

**功能说明** — 为 Nature 系列及 Springer Nature 投稿准备和审核数据可用性声明、数据库计划、数据集引用和 FAIR 元数据检查。支持双语处理：中文作者备注（如"数据可用性声明"、"通讯作者处申请"、"原始数据"、"受限数据"、"公共数据库"）将被转换为精确的投稿就绪英文，并附中文操作备注。

**构建来源** — 基于 Springer Nature 研究数据政策、Nature Portfolio 报告标准、Scientific Data 数据库与引用规范、FAIR 指导原则和 DataCite 元数据规范。

**核心规则**

| 领域 | 核心规则 |
|------|---------|
| 数据可用性 | 将每个支持结果的数据集映射至持久访问途径 |
| 数据库策略 | 优先使用具有持久标识符的强制性或学科专用数据库 |
| 受限数据 | 说明限制原因、控制方、审查途径和访问条件 |
| 数据集引用 | 以 DataCite 风格引用公开数据集，包含创建者、标题、数据库、年份和标识符元数据 |
| FAIR 元数据 | 检查标识符、许可证、README/数据字典、来源、版本和再利用条件 |
| 中文对齐 | 翻译意图而非字面措辞；标记模糊的"合理申请"表述 |

**参考文件**

```
skills/nature-data/
├── README.md
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── chinese-author-alignment.md
    ├── fair-metadata-checklist.md
    ├── policy-principles.md
    ├── repository-and-identifiers.md
    ├── source-basis.md
    └── statement-patterns.md
```

---

## nature-response

**功能说明** — 为 Nature 系列及高影响力期刊手稿修改起草、审核和修订逐条审稿意见回复信。该技能将回复信视为面向编辑的核实文件：每条审稿意见被分配稳定 ID、分类、映射至行动，并关联手稿证据、修改位置或未解决的作者输入标记。

**构建来源** — 基于 Nature 编辑流程指南、Nature 系列修改包说明、Springer Nature 反驳建议和透明同行评审考量。

**核心规则**

| 领域 | 核心规则 |
|------|---------|
| 完整性 | 每条审稿意见均须分配 ID，并给出回复、交叉引用或未解决标记 |
| 行动映射 | 每条回复映射至具体手稿操作，如 `ACCEPT_TEXT`、`ACCEPT_ANALYSIS`、`SOFTEN_CLAIM` 或 `AUTHOR_INPUT_NEEDED` |
| 可追溯性 | 所声称的修改须注明章节、页码、行号、图表、表格、补充材料、引用或可见占位符 |
| 真实性 | 不虚构实验、分析、引用、行号、图表面板、编辑指令或手稿修改 |
| 语气 | 使用协作、以证据为导向的语言；仅以科学或范围为据提出异议 |
| 中文对齐 | 将中文作者备注转换为英文回复文本，并在需要时附中文确认事项 |

**参考文件**

```
skills/nature-response/
├── README.md
├── SKILL.md
├── references/
│   ├── action-mapping.md
│   ├── chinese-author-alignment.md
│   ├── comment-taxonomy.md
│   ├── difficult-cases.md
│   ├── intake-and-routing.md
│   ├── qa-checklist.md
│   ├── response-structure.md
│   ├── source-basis.md
│   └── tone-and-stance.md
├── tests/
    ├── conflicting-reviewers.md
    ├── defensive-draft-audit.md
    ├── evaluation-summary.md
    ├── impossible-experiment.md
    ├── major-revision-missing-evidence.md
    ├── minor-revision.md
    └── rubric.md
└── examples/
    ├── conflicting-reviewers.md
    ├── major-revision-with-missing-evidence.md
    └── minor-revision.md
```

---

## nature-paper2ppt

**功能说明** — 将科学论文、预印本、PDF、文章正文、摘要、图注或阅读笔记转换为简洁的中文 `.pptx` 演示文稿，适用于期刊俱乐部、组会、实验室会议、论文分享或学位研讨会。

该技能识别论文类型与核心论点，仅选取支持证据链的图表，撰写中文幻灯片标题、要点、图注、结论和演讲备注，生成实际 PPTX 文件，并进行轻量级包质量检查。

**核心规则**

| 领域 | 核心规则 |
|------|---------|
| 叙事逻辑 | 以论文的科学论点作为幻灯片主线，而非手稿章节顺序 |
| 论文类型 | 先对论文分类，再选择"主张优先"、"问题到解决方案"、"流程到验证"或"证据映射"逻辑 |
| 图表 | 将图表作为证据使用；裁剪或拆分密集面板，而非将其压缩至无法阅读的位置 |
| 输出 | 以真实 `.pptx` 为主要交付物，包含中文文本和演讲备注 |
| 质量检查 | 重新打开或检查 PPTX 包，记录幻灯片数量、嵌入媒体、备注及任何渲染限制 |
| 真实性 | 不虚构结果、方法、数字、数据集、机制或图表细节 |

**参考文件**

```
skills/nature-paper2ppt/
├── README.md
└── SKILL.md
```

---

## 共同设计原则

本系列所有技能均遵循以下原则：

1. **仅使用一手来源** — 规则基于已发表的 *Nature* 内容或官方期刊指南，而非一般风格偏好。
2. **显式优于隐式** — 每条规则均附有理由说明，而非仅作断言。
3. **章节感知** — 学术写作和图表均需要上下文敏感性；每个技能根据所处理的论文部分应用不同逻辑。
4. **输出优先** — 每个技能立即返回可直接使用的成果：可复制粘贴的文本、`.svg` 文件、`.pptx` 幻灯片或具体建议。不生成中间规划文档。
5. **可扩展设计** — 每个技能在自己的目录中自成体系；新增技能无需修改现有技能。

---

## 新增技能

向本系列添加技能的步骤：

**1. 创建目录**
```
nature-<topic>/
```

**2. 最少必需文件**

| 文件 | 是否必需 | 用途 |
|------|---------|------|
| `SKILL.md` | 是 | 前置元数据（`name`、`description`）+ 规则 + 工作流；触发后由代理自动加载 |
| `README.md` | 是 | 完整英文人类可读参考文档 |
| `references/*.md` | 复杂技能建议添加 | 模块化规则文件（API、设计理论、教程、图表类型等） |

**3. SKILL.md 前置元数据模板**
```yaml
---
name: nature-<topic>
description: >-
  一句话描述该技能的功能及触发时机。
  包含输出格式和主要用例。
---
```

**4. 更新本索引**

在上方[技能索引](#技能索引)表格中添加一行：
```markdown
| [`nature-<topic>`](nature-<topic>/README.md) | 草稿 / 稳定 | 单行用途说明 | 触发关键词 |
```

**5. 状态标签**

| 标签 | 含义 |
|------|------|
| `草稿` | 规则已定义；尚未在真实示例上测试 |
| `测试版` | 已在示例上测试；可能存在边缘情况 |
| `稳定` | 已在真实学术内容上验证；规则已确定 |

---

## 候选技能（尚未构建）

以下为已记录的空白领域，欢迎贡献。

| 候选技能 | 范围 | 优先级 |
|---------|------|-------|
| `nature-stats` | *Nature* 统计报告规范（效应量、置信区间、p 值格式、样本量说明） | 高 |
| `nature-methods` | 深度方法写作助手——可重复性检查清单、禁用短语、伦理审批模板、补充材料组织 | 中 |
| `nature-cover` | 投稿信起草——钩子段落、重要性框架、适合期刊论证、≤ 500 词限制 | 中 |
| `nature-review` | 以 *Nature Reviews* 风格撰写文献综述或综述文章——综合 vs. 总结、论点主导结构 | 低 |
