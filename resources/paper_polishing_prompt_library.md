# paper-polishing-v1

> 一套面向中文作者投英文期刊的论文润色 Prompt 体系。

本框架不止用于“把英语改顺”，而是帮助作者把**中文式论证结构**系统性地重写为更接近英文期刊的论证结构。

**适用场景**：SCI/SSCI 期刊投稿、中文稿深度英文重写。
**占位符约定**：统一使用 `{TOPIC}`、`{METHOD}`、`{JOURNAL}`、`{CONTRIBUTION}` 等，便于跨领域复用。

---

## 目录

* [0. 为什么要做框架](#0-为什么要做框架)
* [1. 九大核心原则](#1-九大核心原则)
* [2. 工作流速查](#2-工作流速查workflow-playbooks)
* [Layer 0 — 诊断层](#layer-0--诊断层)
* [Layer 1 — 结构层](#layer-1--结构层)
* [Layer 2 — 段落层](#layer-2--段落层)
* [Layer 3 — 句子层](#layer-3--句子层)
* [Layer 4 — 章节专项 Prompt](#layer-4--章节专项-prompt)
* [Layer 5 — 一致性与终审](#layer-5--一致性与终审)
* [Layer 6 — 迭代与反思](#layer-6--迭代与反思)
* [Layer 7 — 中文到英文深度重写](#layer-7--中文到英文深度重写)
* [附录](#附录)
* [License & 使用建议](#license--使用建议)


## 0. 为什么要做框架

常见润色 Prompt 的问题不是“写得不好”，而是**粒度太粗**：一个“请把这段润色一下”的 Prompt 同时混合了句法、结构、论证、术语、语气五件事，模型只能挑容易的做——句子更顺了，但文章的论证骨架没变。

中文作者的英文论文读起来奇怪，根源往往不在语法：

* 段落是“材料堆”，而不是“论证单元”；
* 章节标题按“主题”命名，而不是按“功能”命名；
* 结果章在报告系数，而不是在**做解释**；
* 政策建议与前文证据脱钩，像一张独立的 to-do list；
* 引用是“A 研究了 X, B 研究了 Y”的点名式罗列，而不是综合判断式组织；
* 文献综述罗列了“谁研究了什么”，但没有说出“领域到目前为止知道什么、不知道什么、本文补什么”。

这些是**结构层和论证层**的问题，不是语言层能修掉的。本框架把润色拆成**七个层级**，每层解决一类问题。

---

## 1. 九大核心原则

### 原则一：论证先于描述（Argument over Description）

每一段都应该在**做一个判断**（claim），而不是在报告事实。

**判断标准**：把段落第一句遮住，能不能猜出这段“想说什么观点”？如果只能猜出“在讲什么话题”，就是描述，需要改写。

### 原则二：功能先于主题（Function over Theme）

章节标题应回答“这节在论证中**承担什么功能**”。

| ❌ 主题式               | ✅ 功能式                                               |
| ------------------- | --------------------------------------------------- |
| 2.1 `{TOPIC}` 的影响因素 | 2.1 Evidence on `{TOPIC}` and gaps in explanation   |
| 2.2 方法的演进           | 2.2 Methods for `{TOPIC}` analysis and their limits |
| 2.3 小结              | 2.3 Study positioning and contribution              |

### 原则三：证据—主张—解释三段式（Claim–Evidence–Interpretation）

结果与讨论章节的默认段落模板：**先判断 → 再举证 → 后解释**。中文论文最常见的毛病是停在证据，英文期刊期待你完成解释。解释要回答 “So what?”。

### 原则四：引入 Complication，而非平铺结论

好的讨论章**主动引入复杂性**：反常识、内部矛盾、非显著结果、边界条件、与文献冲突——把这些转化为洞察是升级讨论章质量的最短路径。

### 原则五：Gap–Position–Contribution 三元结构

文献综述收尾必须清楚回答：已有研究不能解决什么？本文处在什么位置？本文做了什么独特的事？

### 原则六：每条政策建议都要有证据锚点（Traceable Policy）

自检：“如果审稿人问：这条建议基于哪张图/哪个系数？我能一句话回答吗？”回答不了就不要写。

### 原则七：Voice 一致性（Consistent Authorial Voice）

全文应有统一的作者声音：一致的 `we` / `this study` 用法、一致的 hedging 强度、一致的段落节奏。不能前三节像严谨实证，后三节突然变成政策文件语气。

### 原则八：节制（Restraint）

本框架最大的失败模式不是“不够深”，而是**处处用力**——把每条原则都塞到每个段落里，结果产生一种“所有句子都在做论证”的密集文本，反而失去顶刊那种有节奏的松紧感。

**三条节制法则**：

1. **最合适的位置用一次**：一个论证动作应该只在最合适的位置用一次。
2. **章节之间有密度梯度**：Intro 轻论证 / LR 重论证 / Method 中等 / Results 重证据 / Discussion 重复杂化 / Conclusion 轻收束。
3. **有些段落就是“连接组织”**：背景段、过渡段、说明段可以轻一点，让真正的论证段落突出。

**典型症状**：

* Intro 里出现 “The field is stronger at X than at Y”；
* 每段都以判断句开头，一段都没有背景段；
* 每段都有 Complication，读者疲劳；
* Policy Implications 每条建议都是 “leverage point” 级别的论证。

**自检**：写完一节后倒过来读，感到“喘不过气”就是密度过高；感到“像综述”就是密度过低。

### 原则九：编辑式翻译（Editorial Translation）

深度翻译不是“把中文内容换成英文”，而是一种**编辑行为**。它涉及五类操作，缺一不可：

| 操作                                   | 典型场景                                                   | 框架工具                   |
| ------------------------------------ | ------------------------------------------------------ | ---------------------- |
| **压缩**（Compress）                     | 方法论 12 子节 → 4 子节；公式后“其中...”段 → `where X denotes...` 从句 | Prompt 3.3、4.11        |
| **增补**（Add）                          | 中文量表研究没写 KMO/Bartlett → 英文必须补 Measurement Assessment   | 原则九本身                  |
| **重构**（Restructure）                  | 中文主题式标题 → 英文功能式；中文 bullet 贡献 → 英文流动散文                  | Prompt 1.1、1.2、4.8、3.6 |
| **保留**（Preserve）                     | 数据、引用、变量名、图表编号                                         | Layer 7                |
| **保留结构回声**（Preserve Structural Echo） | 中文三段式路径 “A → B → C” 如果直接打散会丢节奏                         | Prompt 3.6             |

**关键判断**：不是每句中文都应该有对应英文，也不是每条英文需求都要在中文里找源头。作为编辑，要判断：

* 中文说得太多 → **压缩**；
* 中文说得不够 → **增补**；
* 中文说得错位 → **重构**；
* 中文说得正好 → **保留**；
* 中文用了富于节奏的结构性表达 → **保留结构回声**。

**投稿视角自检**：

* 审稿人会问 “为什么没有 KMO 测试？” → 应**增补**；
* 审稿人会问 “为什么方法论有 12 个小节？” → 应**压缩**；
* 审稿人会问 “政策建议和结果怎么对应？” → 应**重构**；
* 读者感到英文版“读起来散”，但中文“有节奏” → 可能丢了**结构回声**。

> 这是整个框架最深的一条原则：**你不是译者，你是为中文稿担任英文编辑。**

---

## 2. 工作流速查（Workflow Playbooks）

根据稿件当前状态选择路径。每条路径给出 Prompt 组合建议与时间估算（以一篇 8000 词论文计）。

### 路径 A：Fast Lane（1–2 小时）

**适用场景**：英文稿已经定型，只差最后一轮语言打磨；或审稿人意见是 minor revision。

```text
0.1 体检 → 3.1 中式英语诊断 → 3.2 AI 痕迹清除 → 5.1 术语一致性 → 5.3 审稿人视角
```

### 路径 B：Standard Lane（半天–1 天）

**适用场景**：英文稿已经成型但读起来“平”；或自己写完之后对结构没信心。

```text
0.1 体检 → 0.2 段落功能标注 → 0.3 术语表前置
   ↓
1.1 章节功能重命名 + 1.2 GPC 重构
   ↓
按章节跑 Layer 4 的对应 Prompt
   ↓
2.2 段落重组 + 2.3 Complication 注入
   ↓
3.2 AI 痕迹清除 + 3.3 语体压实 + 3.6 bullet 转散文
   ↓
5.1 + 5.2 一致性 → 5.3 审稿人视角
```

### 路径 C：Deep Rewrite Lane（2–3 天）

**适用场景**：你有中文稿（或中文式英文稿），要做的是深度结构重写，不是润色。

```text
0.1 体检 → 0.3 术语表前置
   ↓
7.2 整节中文→英文深度重构
   ↓
得到一份结构正确的英文初稿
   ↓
然后按 Path B 继续
```

### 使用建议

* 不要跳过 Layer 0 的诊断。不诊断直接改，80% 的工作会做在错误的位置。
* 不要一次跑完所有 Prompt。每层之间让作者自己判断是否改得合理。
* 不要把 Layer 3 放在 Layer 1/2 之前。句子层改好了，结构一调整就全部作废。
* 中文稿→英文稿的重写，Layer 0.3 的术语表前置会节省大量后期一致性返工成本。

---

## Layer 0 — 诊断层

> **用途**：在动笔改之前，先让模型做体检报告。不要跳过。

### 0.1 全文结构与论证体检 Prompt

```text
你是一位 {JOURNAL} 的资深审稿人。请对下面论文做结构与论证层面的体检，
不要做任何文字润色。请严格按下面格式输出。

═══ PART 1：六维评分（每项 1–5）═══

1. 章节功能清晰度
   分数：
   证据（直接引用原文 1–2 句）：
   改进方向（一句话）：

2. Gap–Position–Contribution 清晰度
   分数：
   证据：
   改进方向：

3. 段落功能清晰度（话题句是判断还是主题）
   分数：
   证据：
   改进方向：

4. 政策—证据可追溯性
   分数：
   证据：
   改进方向：

5. 内部一致性（术语、变量、摘要-结论对齐）
   分数：
   证据：
   改进方向：

6. 期刊契合度（与 {JOURNAL} 风格比对）
   分数：
   证据：
   改进方向：

═══ PART 2：三大优先修改项（按投入产出比排序）═══

P0（必须改）：
P1（建议改）：
P2（可选）：

═══ PART 3：修完 P0/P1 后的预期水平 ═══

一句话预测。

═══ PART 4：本诊断不涉及但作者需注意的风险 ═══

（如：图表质量、统计检验选择、伦理审查声明等）
```

### 0.2 段落功能标注 Prompt

```text
请对下面这一节的每个自然段做功能标注。每段输出：

- 段号：
- 第一句（原样引用）：
- 推测的段落功能（从下面七类中选或自定义）：
  ① 背景铺设  ② 文献综合  ③ 判断提出（claim）
  ④ 证据支持（evidence）  ⑤ 机制解释
  ⑥ 反例/复杂化  ⑦ 引申/政策含义
- 功能完成度：完整 / 半完成 / 错位
- 若错位，简述问题（话题句和内容不符 / 功能缺失 / 功能重复等）

标注完成后回答：

1. 哪些段落话题句和内容不匹配？
2. 本节缺失了哪个功能？（如只有 background+evidence，缺 claim 和 interpretation）
3. 哪些相邻段落功能重复、可合并？
4. 本节整体论证路径是什么？（用 → 表示）是否存在断裂？
```

### 0.3 术语与记号一致性表前置生成 Prompt

```text
在开始翻译或深度润色之前，请扫描全文（或主要章节），生成一份
术语-变量-符号一致性对照表，作为后续所有写作的锁定词表。

═══ 输入 ═══
{中文全稿 或 主要章节}

═══ 输出：五类一致性表 ═══

## 1. 核心概念术语表
| 中文 | 英文（锁定）| 首次全称（若有缩写）| 同义词（不允许混用）|
|---|---|---|---|
| {TOPIC} | {EN_TERM} | full name (ABBR) | synonym A, synonym B |

重点锁定：
- 研究对象（如 eVTOL / 自动驾驶 / MaaS）
- 研究方法核心概念（如 causal discovery / knowledge graph）
- 领域专有术语（如 hypothetical bias / leverage point）
- 人群指称（如 user / traveler / respondent —— 三者语境不同）

## 2. 变量与符号表
| 变量名 | 符号 | 斜体 / 粗体 | 下标惯例 | 首次出现位置 |

## 3. 单位与格式表
- 数字：小数位、千分位
- 百分比：数字% 还是 “X percent”
- 货币：CNY / USD / RMB 哪个？全文一致
- 时间：min / minutes 哪个？
- 距离：km / kilometers 哪个？

## 4. 引用格式锁定
- Author et al., Year 还是 Author, Year
- 多个引用：; 分隔 还是 , 分隔
- 期刊名缩写规则

## 5. 图表命名规范
- 图：Fig. X 还是 Figure X（全文一致）
- 表：Table X 大小写
- 首次出现位置在哪里？

═══ 使用方法 ═══
把这份表放在每次 Prompt 的附录中，作为“术语锁定参考”。后续所有 Prompt
的输出都应符合此表。如果某 Prompt 输出与此表冲突，优先以此表为准。
```

---

## Layer 1 — 结构层

### 1.1 章节功能重命名 Prompt

```text
请把下面章节与子章节标题从“主题式”改为“功能式”。

判断方法：
- 主题式：告诉读者“在讨论什么”
- 功能式：告诉读者“在论证中做什么”

要求：
1. 保留章节原有内容范围，不扩不缩；
2. 避免 “Overview”、“Analysis”、“Related Work”、“Background” 等功能空洞词；
3. 每个新标题配一句话说明该节承担的论证功能（作为注释）；
4. 标题间应形成逻辑递进，而不是并列罗列。

Before/After 示例（供参考，不要照抄）：
- Before: “3.2 Model Design”
- After:  “3.2 A three-layer network to separate within- and between-layer effects”
  功能说明：用层级化模型分离层内机制与层间传导，回应 Research Q2。

输出：
A. 原标题 → 新标题（附功能说明）
B. 标题间的论证链条（用 → 连接）
```

### 1.2 Gap–Position–Contribution 论证重构 Prompt

```text
请按 Gap–Position–Contribution 结构重写下面这段（文献综述末尾或引言末尾）。

Gap（2–3 句）：
- 不要空话 “尚未充分研究”；
- 要具体：已有研究在哪一步停下来、为什么停下来、造成什么后果；
- 允许用对比句式“强于 X，弱于 Y”。

Position（1–2 句）：
- 本文不试图推翻什么，而是在哪条具体路径上推进一步；
- 若是跨方法融合，必须说明为何这两类方法值得融合，而不是“把它们放一起”。

Contribution（通常 3 条，每条 1 句）：
- 每条必须说明“本文具体做了什么”，禁止 “propose a novel framework” 这种空话；
- 第一条：问题重新界定层面；
- 第二条：方法创新层面；
- 第三条：经验外推或实践价值层面。

禁用词：
“to the best of our knowledge, the first”, “novel”, “systematic”,
“comprehensive”, “structured” 单独出现时必须替换为具体动作。

Before/After 示例：
- Before: “This paper proposes a novel framework for {TOPIC}.”
- After:  “This paper links adoption barriers identified from survey data to
          policy instruments drawn from analogous mobility systems, using a
          knowledge graph to make the mapping explicit.”
```

### 1.3 从主题式到功能式文献综述 Prompt

```text
请把下面文献综述从“主题式组织”重构为“功能式组织”。

旧结构常见形式：
- 2.1 关于 X 的研究  / 2.2 关于 Y 的研究  / 2.3 关于 Z 的研究

目标结构（推荐三段）：
- A. Evidence on {TOPIC}
     领域就 {TOPIC} 积累了什么稳定证据？
     写作重点：用 “the literature suggests/shows” 汇聚多篇，而非逐篇介绍。
- B. Methods for {TOPIC} and their limits
     现有方法能做什么、不能做什么？按方法家族分组，每组 1–2 句概括。
- C. Study positioning
     综合 A、B，本文处于什么位置？结尾对接 Contribution。

改写要求：
1. 所有原有引用必须保留，不新增虚构文献；
2. 把 “A 等研究发现……B 等研究发现……” 改为综合判断句；
3. 对不显著/矛盾/分散证据，明确写出分歧，不要掩盖；
4. 同一篇文献只在最合适的那一段出现；
5. 结尾必须直接引出 Gap–Position–Contribution。
```

### 1.4 Subsection 设计 Prompt

```text
请判断下面这节内容是否需要拆分为子节（如 5.1.1、5.1.2），
或是否需要把散乱的多个子节合并。

═══ 拆分子节的三个合法理由 ═══
1. 概念对比：如 within-layer vs between-layer、short-term vs long-term。
2. 对象分组：如 for policymakers vs for operators、quantitative vs qualitative evidence。
3. 结果维度：如直接效应 vs 间接效应、全样本 vs 子样本。

不合法的拆分理由：
- “这节太长”（长 ≠ 需要拆，先看是否能删）；
- “每个变量一个小节”（应按研究问题而非按变量组织）；
- “按图序号一图一小节”（图应服务于论证，不是论证的组织依据）。

═══ 合并子节的信号 ═══
中文稿常见的“一个算法一节”碎片化结构在英文期刊往往需要合并：

合并信号：
- 多个子节都在做同一件事的不同技术步骤
- 子节长度都在 1–2 段且独立性弱
- 子节之间缺乏论证对比，只是技术序列
- 标题形式如 “3.2.1 / 3.2.2 / 3.2.3” 这种深嵌套

合并方法：
- 把技术步骤折叠到段内用 “First, ... Then, ... Finally, ...” 组织
- 保留核心公式与关键数字，删除冗余解释
- 合并后的新子节用功能式标题

═══ 新增子节的信号 ═══
有时中文用一段讲完的内容，英文反而要拆开：

新增信号：
- 段落内部有明显对比结构却没有视觉分隔
- 段落跨越了两种分析对象
- 段落讨论了两组建议
- 段落超过 300 词且读者难以一口气读完

═══ 输出 ═══
1. 本节是否应该拆分、合并、或保持现状？为什么？
2. 若拆分/合并，建议的新子节结构与每个子节的论证功能。
3. 若保持现状，建议保留哪些内部逻辑标记来帮助读者识别子段落。
```

---

## Layer 2 — 段落层

### 2.1 段落话题句升级 Prompt

```text
请逐段检查下面文本的话题句（每段第一句）。

判断原则：
- 合格话题句是判断（claim），不是主题（topic）。
- 看完第一句，读者应知道“这段要证明什么”，而不只是“这段要讨论什么”。

处理流程：
1. 抽取每段第一句；
2. 判断是“判断”还是“主题”；
3. 对主题型话题句，根据段落内容改写为判断型。

Before/After 对照：

❌ 主题型：
“This section discusses the role of income in eVTOL adoption.”
→ 只告诉读者话题，没有论点。

✅ 判断型：
“Income is not just a willingness-to-pay variable; it reshapes risk perception
in ways that challenge the premium-user assumption.”
→ 给出明确论点，读者可以带着这个假设读下去检验。

严格约束：
- 不要为了“更具体”而扭曲原意；
- 若段落内部确实没有判断，请标注“需要补充判断或拆分合并”，不要硬编造；
- 避免用 “In this section we discuss/present/examine” 开头。
```

### 2.2 Claim–Evidence–Interpretation 重组 Prompt

```text
请把下面这段按 Claim–Evidence–Interpretation 三段式重组。

三部分结构：
1. Claim（1 句）：对现象的总体判断。禁止 “This section shows...”。
2. Evidence（1–3 句）：具体系数、显著性、对比、图表编号。
3. Interpretation（2–4 句），包含三层：
   - 机制层：为什么会出现这个结果？
   - 意义层：对理论/实践意味着什么？
   - 边界层：在什么条件下成立？什么条件下失效？

改写要求：
1. 若原文只停在 Evidence，请根据证据方向补齐 Interpretation，
   但不得编造原文没有的数据；
2. Interpretation 禁用模板句 “This suggests that...”、“This indicates...”，
   改用具体动词：complicates, qualifies, reverses, constrains,
   explains, undermines, reframes, challenges；
3. 每段不超过 6 句，超过就拆；
4. 禁用 “as mentioned above”、“as we all know”、“it is well known that”。
```

### 2.3 Complication 注入 Prompt

```text
请在下面分析中主动引入至少一处 complication，以提升论证深度。

Complication 类型（选最合适的一种）：
- 反常识型：结果与领域常见假设相悖；
- 内部矛盾型：两个结果互相牵制（A 促进 Y，但通过 B 又抑制 Y）；
- 非显著型：本该显著的结果不显著，这本身说明什么；
- 边界型：结果仅在某个子群/条件下成立；
- 文献冲突型：结果与已有某条结论不一致，需要调和。

写作要求：
1. 用以下开头之一：
   - “Although X, ...”
   - “This finding complicates the common assumption that...”
   - “One likely reason is...”
   - “Contrary to what might be expected, ...”
   - “At the same time, this result should be read with caution, because...”
2. 不要削弱原结论，而是通过 complication 让原结论显得更站得住；
3. 禁止编造数据，只在原文数据范围内做解释；
4. 若原文数据实在无法支撑任何 complication，诚实说明理由，不硬加。

非显著结果处理范例：
❌ 忽略：不提，或一句话带过
✅ 转化：“The non-significant improvement in the safety dimension likely
reflects the current scarcity of structured safety evidence in the field,
rather than a limitation of the method itself.”
```

### 2.4 Leverage-Point 深度解释 Prompt

```text
在讨论“某变量在系统中重要”时，按三级阶梯拔高，不要停在 Level 1。

Level 1（弱）：X 显著影响 Y。
Level 2（中）：X 通过 M 影响 Y，因此干预 X 可通过 M 改变 Y。
Level 3（强）：X 是决策系统的 leverage point，它连接多条路径，
对 X 的干预沿多条路径传导；相反，直接干预 Y 成本更高、效果更短。

要求：
1. 只有在网络拓扑指标（介数、强度）或多路径证据支持时才可到 Level 3；
2. Level 3 必须说明：为什么别的变量不是 leverage point；
3. leverage point 讨论必须直接对接政策设计，不要写成封闭学术观察。
```

---

## Layer 3 — 句子层

### 3.1 中式英语深度诊断 Prompt

```text
请对下面文本做中式英语深度诊断。不要只挑语法错，请按六类找出结构性问题：

1. 抽象名词堆叠
   “the construction of X” / “the realization of Y” / “the improvement of Z”
   → 改为动词化表达 construct / realize / improve

2. 虚动词 + 抽象名词
   “make an improvement to” / “carry out an analysis of”
   → 改为 improve / analyze

3. 机械逻辑连接词高频
   “therefore / thus / at the same time / meanwhile” 反复出现
   → 英文期刊更常用句子结构表达逻辑

4. 时间/空间框架前置
   “In recent years,...” / “From the perspective of...” / “With the development of...”
   → 英文倾向开门见山

5. 形式主语与被动语态过度
   “It can be seen that...” / “It should be noted that...”
   → 主动陈述

6. 范围/程度词滥用
   “various / a variety of / certain / relevant / corresponding / appropriate”
   → 多数可直接删

输出格式：
- 原句 → 问题类型 → 改写
- 结尾给出“可直接替换”的词表（原词 → 替换建议）
```

### 3.2 AI 痕迹识别与清除 Prompt

```text
下面文字逻辑基本对，但读起来像 AI 改过。请重写，使其更像本领域作者写的英文。

典型 AI 痕迹（必须减少）：
- 模板句：This suggests that... / This indicates that... /
          Taken together, these results... / This finding highlights...
- 抽象功能性表达：provides a structured pathway to... /
                  has important implications for... / plays a crucial role in...
- 三拍子并列：A, B, and C 反复出现，每段一个“三件套”；
- 结构过于整齐：每段等长，都以 Overall/In summary 结尾；
- “结果—意义—政策” 三句套。

重写要求：
1. 句长变化：长短句混合；
2. 开头变化：不要连续几段都以 This 开头；
3. 具体动词替代抽象名词短语；
4. 允许适度不对称（破折号、插入语），反而更像作者；
5. 保留所有事实、数据、引用、图表编号、变量名。

自检：全文搜索 “This suggests that” 与 “This indicates that”，
合计出现次数应不超过 2 次。
```

### 3.3 学术语体压实 Prompt

```text
请在不丢失核心信息的前提下对下面文本做语体压实。

压实方法：
1. 删除元语言铺陈
   “In order to better understand this problem, we first ...” → 直接陈述做了什么；
2. 合并同义句：连续两句表达相同意思只保留更具体的；
3. 名词化瘦身：
   “the identification of the influence of A on B” → “how A affects B”；
4. 精简限定：
   “to some extent / to a certain degree / relatively / quite / very” 多数可删；
5. 去除结论性空话：
   “This is of great significance” 直接删或替换为具体含义。

6. 公式后文压缩（方法论章节高频问题）
   中文方程后常跟 80–150 词的“其中...”解释段，解释每个符号。
   英文紧凑模式：将所有变量含义折叠到一个 “where X denotes..., Y is...,
   and Z represents...” 的从句中（30–50 词）。

约束：
- 不要压成“技术报告式短句”；
- 保留学术必需的 hedging：“may / could / is likely to / tends to”；
- 目标压缩率：不丢信息前提下减少 15%–25% 词数；
- 方法论章节的公式后文压缩可更激进，目标 30%–50% 词数。
```

### 3.4 Hedging 强度校准 Prompt

```text
请检查下面文本中的断言强度，校准 hedging 到合适位置。

三类常见问题：
1. 过度自信（overclaiming）：
   “X causes Y” / “This proves that...” / “Always / Never”
   → 弱化为 “X contributes to Y” / “This supports the view that...” /
     “Typically / Rarely”

2. 过度 hedging（underclaiming）：
   “This might possibly potentially perhaps suggest that may...”
   → 一个句子只保留一个 hedge，不要堆叠

3. hedge 强度不匹配证据强度：
   相关系数显著但仅中等 → 用 “associated with” 而非 “drives”；
   实验显著 + 效应大 → 用 “increases” 而非 “may be associated with”。

请输出：
- 原句 → hedge 问题类型 → 改写（附一句说明 hedge 强度的依据）
- 整体 hedge 风格诊断：本文目前偏保守还是偏激进？

参考附录 F 的 Hedging Toolkit 选择具体词汇。
```

### 3.5 密度校准 Prompt

```text
请检查下面一节文字是否存在“论证密度过高”的问题——这是本框架
本身可能造成的 over-engineering 风险。

密度过高的六个症状（每发现一条，标出具体位置）：

1. 每段都以“判断句”开头
2. Intro 中出现 LR 结尾的 GPC 句式
3. 每段都有 Complication
4. Policy Implications 每条都是 “leverage point” 级论证
5. 连接性段落被改成判断段落
6. hedge 强度前后不一致

输出：
A. 本节密度诊断：整体密度偏高 / 合适 / 偏低
B. 六类症状命中清单（具体到句）
C. 对症状 1–5 的具体“降密度”改写建议（挑最紧迫的 3 条）
D. 本节与相邻章节的密度梯度是否合理？
```

### 3.6 Bullet-to-Prose 转换 Prompt

```text
中文论文常见的 (1)(2)(3) 清单型表达在英文顶刊中几乎从不使用（除了
systematic review 等特定文类）。请把下面的 bullet/编号清单转换为流动散文。

═══ 适用场景 ═══
A. Introduction 末尾的贡献列表
B. Conclusion 中的政策建议（若未按行动者分层，见 Prompt 4.13）
C. Limitation 的三条
D. Future Research 的三方向
E. Method 开头的“本研究步骤：第一步... 第二步...”

═══ 三档转换策略 ═══

档位 1：完全散文化（推荐用于贡献和 Limitation）
- 用 “First..., Second..., Third...” 或 “The study makes three contributions.
  We first... We then... Finally...”
- 不使用任何 bullet 或编号
- 段落呈现为一段完整流动的散文

档位 2：轻度结构化（推荐用于复杂政策建议）
- 用子段首句的 “For policymakers, ...” / “For operators, ...” 区分
- 不用 bullet，但用段首引导词帮助读者识别
- 整体仍是散文段落

档位 3：保留视觉结构（仅在极少数情况下使用）
- 例如 Method 中严格的算法步骤
- 用斜体或粗体的引导词而非 bullet
- “**Step 1.** ... **Step 2.** ...”

═══ 结构回声保留规则 ═══
如果中文 bullet 本身蕴含节奏感，转散文时应保留这份节奏：
- 使用 italics 标注三个关键短语
- 使用 em dash 标识三段式
- 使用 parallel structure 复现节奏

═══ 输出 ═══
A. 转换档位选择 + 理由
B. 散文化后的段落
C. 是否保留了结构回声？若保留，说明用了什么英文修辞手段复现节奏
D. 删除了哪些内容（重复、元语言、“propose a novel” 等空话）
```

---

## Layer 4 — 章节专项 Prompt

### 4.1 Abstract：六要素结构 Prompt

```text
请按六要素结构撰写或润色 Abstract，适合 {JOURNAL}。

六要素（顺序可微调但不可缺失）：
1. Context（1–2 句）：{TOPIC} 的背景与现实意义，不要宣传式语言。
2. Tension/Gap（1–2 句）：本文回应的具体矛盾，不要 “尚未充分研究”。
3. Approach（2–3 句）：方法设计核心逻辑，不是技术堆砌；说明为什么这么做。
4. Data（1 句）：样本来源、规模、时空范围。
5. Findings（2–4 句）：
   - 1–2 句给主要影响方向（正/负）；
   - 1–2 句给方法比较结果（若有）；
   - 禁止 “results show that the proposed method outperforms baselines”。
6. Implications + Contribution（1–2 句）：政策/实践含义 + 方法学或理论贡献。

语言约束：
- 总长 180–280 词；
- 避免 “To the best of our knowledge”、“novel framework”、“comprehensive study”；
- 缩写只在全称后紧跟括号，后文统一用缩写；
- Keywords 5–7 个，避免与标题完全重合。
```

### 4.2 Introduction：四段漏斗 Prompt

```text
请按四段漏斗结构重写 Introduction。

【重要】Gap 密度：Intro 用“轻 Gap”，不要把 LR 结尾的 GPC 放在这里。

段 1｜现实问题层（Why this matters）：
- {TOPIC} 的紧迫性或转折点；
- 避免 “近年来……随着……发展” 式陈述，直接给具体现象或数据；
- 可以用一个弱元研究开场词缓冲，避免一上来就硬判断。

段 2｜研究进展层（What the field knows）：
- 已有研究的主要共识；
- 不堆引用，用概括性判断带出 3–5 个核心引用；
- 段尾可以用一个“问题转向句”做自然过渡。

段 3｜缺口层（What the field cannot yet answer）：
- 具体化 gap：领域在哪一步断了？
- 必须写得准确但不过度强硬；
- 不要在这段给出 “stronger at X than at Y” 的完整 GPC 判断。

段 4｜方案与贡献层（What this paper does）：
- 一句话描述方案核心逻辑；
- 列出 3 条具体贡献，用流动散文而非 bullet；
- 每条贡献必须说明“具体做了什么”。

段 5｜章节组织（可选独立段）：
- 3–5 句，用加粗的 Section 号；
- 避免 “The structure of this paper is as follows” 这种元语言过长。

禁止：
- 在 Introduction 中提前给结果；
- 展开方法细节；
- 用自谦式表达；
- 把 LR 结尾的 GPC 对比句前置到 Intro。
```

### 4.3 Literature Review：三段功能式 Prompt

参见 Layer 1 的 1.3。此处补充章节内约束：

```text
补充约束：
1. 禁止无指向引用：
   “some scholars / many researchers / a lot of studies” → 删除或具体化；
2. 引入相邻领域时必须说明理由；
3. 避免把多个相邻领域揉成一大段，每次引入一个要明确目的。
```

### 4.4 Citation Integration 专项 Prompt

```text
请检查下面文本中引用的整合方式，按三条标准改写。

三条标准：
1. 引用应服务于判断，不是判断的替代品。
2. 同一判断的多个来源应合并引用。
3. 引用前后应有作者的声音。

处理流程：
1. 找出所有“点名式”引用：“A found..., B found...”；
2. 按标准 1–3 改写；
3. 检查每 200 词范围内是否至少有一个引用（文献综述要求）或每 300 词（其他章节）。

输出：
A. 改写后的段落；
B. 被合并/删除/重新定位的引用清单；
C. 标注 “可能需要补充引用” 的位置。
```

### 4.5 Methodology：设计逻辑优先 Prompt

```text
请润色下面的 Methodology，使其更像英文期刊的方法章节。

每个子节的推荐写作顺序：
1. 设计逻辑（Design logic, 1 段）：为什么采用这个方法？它处理本研究的哪个具体难点？
2. 关键组件（Components）：每个组件先给一句“它承担什么功能”，再给技术细节。
3. 假设与约束（Assumptions & constraints, 1 段）：方法成立的前提、适用条件、失效条件。
4. 实现细节（Implementation, 1 段）：参数、迭代次数、硬件、数据划分。

语言约束：
- 避免 “This process has three stages: First, ...; Second, ...; Third, ...”；
- 避免把方法写成工具使用说明书；
- 每个子节结尾不写“综上所述”；
- 融合型方法必须强调本文中的功能分工。
```

### 4.6 Results：Claim–Evidence–Interpretation 重写 Prompt

```text
请将 Results 按 Claim–Evidence–Interpretation 结构重写。

每段必须满足：
- Claim：一句话判断（不是话题）；
- Evidence：具体系数 / 显著性 / 图表编号 / 对比；
- Interpretation：机制 + 意义 + 边界。

升级手法：
1. 小节分组按“研究问题”而不是按“变量”。
2. 报告 vs 解释黄金比例：每段纯报告系数不超过 30%，其余至少 70% 是 Interpretation。
3. 把非显著结果当作洞察。
4. 每个 Fig./Tab. 必须被“解读”，不能只是 “As shown in Fig. X, ...”。
```

### 4.7 Discussion：复杂化 + 可信度边界 Prompt

```text
请把 Discussion 改写为以“复杂化 + 可信度边界”为核心的章节。

结构建议：
1. 总体判断（1 段）：结果对 {TOPIC} 的总体理解意味着什么？
2. 与文献对话（1–2 段）：验证了什么？偏离了什么？为什么偏离？
3. 机制解释（1–2 段）：为什么数据呈现这个模式？
4. 可信度与边界（1 段，必须有）：样本、方法、情境的外部有效性边界。
5. 政策或实践含义（见 4.8）。

必须避免：
- 把 Results 复述一遍；
- Limitation 当礼节性收尾；
- “Further research is needed to...” 空转结尾。
```

### 4.8 Policy Implications：行动者分层 Prompt

```text
请把 Policy Implications 按行动者分层改写，而不是按建议清单平铺。

分层模板：
- 5.x.1 For policymakers / regulators
- 5.x.2 For operators / service providers
- 5.x.3 For users / community（若适用）

每层内部要求：
1. 先给一句统领判断：这类行动者在 {TOPIC} 中承担什么角色？
2. 然后给 2–3 条建议，每条满足：
   - 明确行动（do what），不是 “应该加强 / 关注”；
   - 证据锚点：说明 “based on the result that ...”；
   - 适用条件：什么情境下有效？什么情境下要谨慎？

避免：
- “should / must / need to” 连续重复；
- 一条建议超过 3 行；
- 与前文结果无关的额外建议。

末尾加一句 caveat：
“These implications are based on {DATA_SCOPE} and should be interpreted
with attention to local institutional and market conditions.”
```

### 4.9 Conclusion：四功能模块 Prompt

```text
请把 Conclusion 重组为四个功能模块，严格限制长度。

四模块：
1. What this study did（2–3 句）：概括研究任务与方案核心。
2. How the method performed（2–4 句）：方法有效性证据。
3. Key substantive findings（3–5 句）：这项研究加到领域认知中的新东西。
4. Future research（2–4 句）：每条方向必须具体。

禁止：
- 再重复 Policy Implications；
- 写 “In conclusion, this paper proposes...” 自指开场；
- “This paper is the first to...” 引战句式。
```

### 4.10 表格与图注深度润色 Prompt

```text
请润色下面的图注、表题、表头和表中语言。

表题（Table caption）：
- 完整描述短语，不是主题词；
- 包含：内容类型 + 样本或单位 + 关键限定。

图注（Figure caption）：
- 结构：主体 + 方法 + 重点信息；
- 必要时附图中符号/颜色说明；
- 避免一个词的图注 “Flowchart.”。

表头（Column headers）：
- 统一大小写；
- 变量名和单位都要出现；
- 避免中式直译。

表中语言：
- 用简短名词短语，避免完整句；
- 数字格式一致；
- 空值用统一符号。

正文对图表的引用：
- 每个图表必须被“解读”；
- 全文引用格式统一；
- 在首次提及一个图表的段落之后再讨论该图表。

输出：
A. 改写后的所有表题 / 图注 / 表头；
B. 建议合并的行/列；
C. 术语与正文的一致性检查结果。
```

### 4.11 Methodology Consolidation Prompt

```text
中文论文的方法论常见“一个算法/一个步骤 = 一个子节”的碎片化结构。
请把下面方法论按“概念集群”合并，目标 3–5 个子节，对应论证任务而非技术序列。

═══ Step 1：识别概念集群 ═══
请先分组：把所有现有子节按“它们共同完成什么方法论任务”聚类。

典型集群：
- 集群 A：数据建模与结构
- 集群 B：估计与检验
- 集群 C：领域知识组织
- 集群 D：生成与集成

═══ Step 2：合并规则 ═══
每个集群：
1. 用一句话说明承担的方法论任务（变成新子节标题）。
2. 把该集群内原有的多个算法/步骤折叠为段内序列。
3. 每个技术组件保留：核心公式 + 关键数值 + 一句功能说明。
4. 删除：冗余解释、通用技术背景、步骤之间的元语言。

═══ Step 3：标题功能化 ═══
旧子节标题（主题式）：
- “2.1 DirectLiNGAM”
- “2.2 Bootstrap 显著性检验”
- “2.3 网络拓扑指标”
- “2.4 因果链挖掘”

合并后功能式标题：
- “4.2 Causal discovery, network analysis, and chain mining”

═══ 输出 ═══
A. 旧 → 新子节映射表
B. 每个新子节的功能式标题 + 一句功能说明
C. 合并后的完整方法论 Markdown
D. 删除了哪些内容

═══ 禁止 ═══
- 禁止“保守一对一翻译”；
- 禁止删除核心公式或关键参数；
- 禁止把合并后的子节变成难以消化的“大段落怪兽”。
```

### 4.12 Dual-Gap 结构处理 Prompt

```text
方法论创新型论文常有两个相关但独立的 gap。
例如：
- Gap 1：传统 SP 问卷有假设性偏差
- Gap 2：通用 LLM 缺少领域知识约束
- 本文通过 KG 同时回应这两个 gap

═══ 三种结构选择 ═══

选择 A：合并为一段
- 用一个总判断引入：“The methodological challenge is therefore twofold.”
- 然后用 “On the one hand, ...; on the other hand, ...” 展开
- 最后给一句桥接：“Addressing both requires ...”

选择 B：保留两段但用过渡句承接
- 第一段完整展开 Gap 1
- 第二段开头用过渡句：“{METHOD_B} opens an additional methodological space”
- 第二段结尾给桥接句

选择 C：一段主 gap + 一个子段侧翼
- 主 gap 展开为一段
- 侧翼 gap 用一句话附在 LR 末尾或 Method 开头

═══ 判断规则 ═══
Q1：两个 gap 是否都是本文的“主驱动力”？
Q2：两个 gap 能否用同一句判断话概括？
Q3：读者是否需要单独理解 gap 2 才能理解本文贡献？

═══ 输出 ═══
A. 选择的结构（A/B/C）+ 理由
B. 改写后的段落
C. 过渡句 / 桥接句的设计说明
D. Contribution 段是否需要调整？
```

### 4.13 Policy Implications 三分层填空模板

```text
Policy Implications 最常见的三分层模板（可直接填空使用）：

5.4 Policy implications

[Lead paragraph, 2–3 sentences]
We draw three groups of implications from the preceding results. These
build on the causal pathways identified in Section 5.1 and the leverage
points identified in Section 5.2, and are intended as entry points for
discussion rather than prescriptive guidance given the {DATA_SCOPE} of
this study.

5.4.1 For policymakers and regulators
[One leading claim, 1 sentence]
Policymakers sit at the point where {KEY_LEVERAGE_1} translates into
system-level conditions for adoption.

[2–3 specific recommendations]
First, {ACTION_1A}, since the results show {RESULT_1A} (§5.x), particularly
{CONDITION_1A}. Second, {ACTION_1B}, given that {RESULT_1B}. Third,
{ACTION_1C}.

5.4.2 For {OPERATORS} and service providers
[One leading claim]
{OPERATORS} have the most direct influence on {KEY_LEVERAGE_2}.

5.4.3 For users and community [optional, only if applicable]
[One leading claim]
[1–2 recommendations]

[Caveat paragraph, 1–2 sentences]
These implications rest on {DATA_SCOPE} and should be interpreted with
attention to local institutional and market conditions.

═══ 每条建议的质量检查 ═══
1. 具体性：能否从 “should improve X” 改为 “should do Y to improve X”？
2. 证据锚点：括号内能否写出 “based on §5.x's result that ...”？
3. 条件性：在什么情境下有效？什么情境下可能失效？
4. 行动者匹配：这条建议指向的行动者真的是这一小节的主体吗？

═══ 禁止 ═══
- 同一行动者下 > 3 条建议
- “should / must / need to” 连续重复
- 建议之间没有优先级暗示
- 和前文结果无关的“补充建议”

═══ 输出 ═══
A. 填空完成的三分层 policy implications
B. 每条建议背后的证据锚点编号
C. 是否有建议因缺乏证据支持而应删除？
```

---

## Layer 5 — 一致性与终审

### 5.1 术语与记号一致性 Prompt

```text
请对全文做术语与记号一致性审查。

检查项：
1. 关键术语统一
2. 缩写首次出现
3. 变量记号一致
4. 数字与单位
5. 引用格式

若已通过 Prompt 0.3 生成术语一致性表，把该表作为本 Prompt 的参考标准。

输出：
A. 不一致项目清单（原文 → 建议统一为）
B. 建议的术语对照表（glossary）
```

### 5.2 图表—引文—正文三向对齐 Prompt

```text
请检查图、表、引用、正文四者之间的对齐。

四项对齐：
1. 正文 ↔ 图表
2. 正文 ↔ 引用
3. 正文 ↔ 数据
4. 摘要 ↔ 结论

输出：
A. 不对齐项目清单（具体到句）；
B. 建议修复方案；
C. 若存在数据矛盾，标注需要作者核对。
```

### 5.3 审稿人视角重审 Prompt

```text
请扮演 {JOURNAL} 的挑剔审稿人，给本文写 major revision 意见。

请给出：
1. 三条最有可能导致拒稿的质疑（按审稿人真实逻辑）；
2. 五条 major comments（方法、结果、外推、政策、文献定位）；
3. 十条 minor comments（术语、记号、图表、引用、语言）；
4. 若要 major revision 后重投，作者需补什么实验、文献、论证？

语气：严厉但专业，不要礼貌化敷衍。直接引用原文作为证据。
```

### 5.4 期刊编辑视角终审 Prompt

```text
请扮演 {JOURNAL} 的 Associate Editor，基于审稿人意见做决策分析。

回答：
1. 稿件最大优势是什么？
2. 稿件最大风险是什么？
3. 与 {JOURNAL} 过去一年发表文章的契合度（1–5 评分）
4. 话题是否符合 {JOURNAL} 主要读者群？
5. 投稿前建议作者修订哪三件事？

决策建议：
- 推荐 desk accept for review / major revision before review / desk reject
- 给作者一个 3 点清单。
```

---

## Layer 6 — 迭代与反思

### 6.1 三轮自动迭代 Prompt

```text
请对下面文本做三轮自动迭代。

第一轮｜诊断（不修改）
按 Layer 0 的两个 Prompt 标准，列出结构、段落、语言三层级问题；
给出优先级 P0 必须改 / P1 建议改 / P2 可选。

第二轮｜结构与论证重构
针对 P0 和 P1 的结构层问题修改；
保留 Evidence 不变，只改 Claim 和 Interpretation；
输出修改对照表（原段 → 新段 → 修改理由）。

第三轮｜语言精修
在第二轮基础上按 Layer 3 做句子层优化；
同时清理 AI 痕迹。

最终输出：
A. 三轮叠加后的定稿；
B. 关键修改追踪表；
C. 仍需作者确认的条目（数据、引用、因果断言）。
```

### 6.2 高亮修订版 Prompt

```text
请在原文基础上润色，并按不同修订类型用不同颜色标注（用于 Word）：

- 黄色：句子重写（保留原意但改变表达）
- 绿色：新增的解释、边界、复杂化内容
- 蓝色：结构调整（段落移动、拆分、合并）
- 红色删除线：删除的冗余或错误内容

要求：
1. 纯粹小语法修正不必高亮；
2. 保留所有数据、引用、公式、图表编号；
3. 段落移动在新位置开头注明 “[moved from Section X.Y]”；
4. 末尾附修改摘要。
```

### 6.3 对比审校 Prompt

```text
下面给出两个版本：
- 版本 A：{中文稿 / 旧英文稿}
- 版本 B：{新英文稿 / 新版本}

请做“版本跃迁分析”，不做语言润色。

回答：
1. 版本 B 相较 A，在八个维度各自提升多少（1–5 分）；
2. 哪些改动最大程度提升了论证？给 3 条代表性改动；
3. 哪些地方 B 没有比 A 更好甚至变差？为什么？
4. 从 A 到 B 的改动中哪些是“可迁移的写作技巧”？
5. 若还要做版本 C，你会优先改什么？
```

---

## Layer 7 — 中文到英文深度重写

> 处理真实场景：你有一份中文稿（或中文式英文稿），不是要“把英语改顺”，是要做一次**深度结构重写**。

### 7.1 段落级中文→英文深度重写 Prompt

```text
下面是一段中文学术写作。请不要做逐句翻译，而是按下面四步重写：

步骤 1｜论证抽取
先用 3–5 句中文，概括这段在论证什么判断、用什么证据、要导向什么结论。

步骤 2｜结构重组
按英文期刊段落结构重排：
- 第一句：判断（claim）
- 中间：证据（evidence）
- 结尾：解释 + 引申（interpretation）

步骤 3｜英文输出
按步骤 2 的骨架用英文重写：
- 话题句要是判断而非话题；
- 每句只做一件事；
- 避免中式连接词堆叠；
- 允许改变句子数量；
- 引用、数据、变量名保留。

步骤 4｜自检
检查：
- 新英文段的第一句遮住后，读者能否猜出论点？
- 新英文段是否补上了原中文未写的 interpretation？
- 新英文段是否删除了原中文的元语言？

输出格式：
A. 步骤 1 的论证抽取；
B. 步骤 2 的结构大纲；
C. 步骤 3 的英文段；
D. 步骤 4 的自检回答；
E. 删除了哪些内容、为什么删。
```

### 7.2 整节中文→英文深度重构 Prompt

```text
下面是一整节中文学术写作。请按下面五步做深度重构，
不要做逐段翻译。

步骤 1｜识别本节论证任务
用一句话回答：这节在全文论证中承担什么功能？

步骤 2｜重命名子节
按功能式命名。如果原中文有“一、二、三”混用“2.1, 2.2”，统一为英文期刊编号方式。

步骤 3｜段落功能重排
为本节画一张段落功能图：每个段落的功能是什么？
功能之间是否有递进？是否有缺失？是否有重复？

步骤 4｜按新段落结构用英文重写
每段按 Claim–Evidence–Interpretation 组织。

步骤 5｜审稿人视角检查
跑一次 Layer 5.3 的审稿人视角 Prompt，看新重构版有没有明显被攻击点。

输出格式：
A. 步骤 1 的论证任务陈述；
B. 步骤 2 的新子节标题（附功能说明）；
C. 步骤 3 的段落功能图（文字版）；
D. 步骤 4 的英文重写；
E. 步骤 5 的审稿人模拟回应与对策。
```

### 7.3 使用建议

* **不要一次重构整篇**：一节一节来，每节重构后让作者看一遍再进下一节。
* **不要在 7.1/7.2 结束后立刻做语言精修**：先让重构结果沉淀半天，再跑 Layer 3。
* **保留中文原稿**：重构后的英文段可能比中文原段短很多，这是正常的，但要确认没有丢失关键证据或引用。
* **引用保留**：重构允许删内容但不允许删引用。删引用前先问自己：这个引用在新结构里是不是真的没位置？
* **做 Layer 7 之前先跑 Prompt 0.3 生成术语表**。所有 Layer 7 输出都参照此表。

---

## 附录

### A. 中式英语常见模式清单

| 类别     | 中式表达                                        | 英文改写                       |
| ------ | ------------------------------------------- | -------------------------- |
| 抽象名词堆叠 | the construction of the platform            | building the platform      |
| 抽象名词堆叠 | the realization of the goal                 | achieving the goal         |
| 虚动词+名词 | make an analysis of                         | analyze                    |
| 虚动词+名词 | carry out a discussion about                | discuss                    |
| 虚动词+名词 | conduct a comparison between A and B        | compare A and B            |
| 时间框架前置 | In recent years, with the development of... | 直接进入内容                     |
| 时间框架前置 | Nowadays, ...                               | 删                          |
| 形式主语   | It can be seen that...                      | 直接陈述判断                     |
| 形式主语   | It should be noted that...                  | 直接说要点                      |
| 形式主语   | It is well known that...                    | 删，或给具体来源                   |
| 范围词滥用  | various factors such as A, B, and C         | A, B, and C                |
| 范围词滥用  | the corresponding results                   | the results                |
| 程度词模糊  | to some extent                              | 删或具体化                      |
| 机械连接   | at the same time / meanwhile                | 用从句或省略                     |
| 机械连接   | Therefore, ...; Therefore, ...              | 其中一个改为 As a result 或删      |
| 过度泛化   | This is of great significance               | 具体说意义在哪                    |
| 过度承诺   | This paper comprehensively studies...       | This paper examines...     |
| 过度礼节   | We humbly propose...                        | We propose...              |
| 直译     | Information on the respondents              | Respondent characteristics |
| 直译     | The situation of traffic congestion         | Congestion conditions      |

### B. AI 模板句黑名单与替换库

**黑名单**：全文出现超过 2 次即判定“AI 痕迹偏重”。

| AI 模板句                                | 替换方向                                                     |
| ------------------------------------- | -------------------------------------------------------- |
| This suggests that...                 | 具体动词：X complicates / qualifies / reverses / constrains Y |
| This indicates that...                | 把间接推断直接写出来                                               |
| This finding highlights...            | 直接说 finding 的含义                                          |
| Taken together, these results...      | 精简为一句综合判断                                                |
| Overall, ...                          | 删，或改为具体总结                                                |
| In summary, ...                       | 删，或改为具体总结                                                |
| This paper provides a novel framework | 描述框架做了什么具体动作                                             |
| plays a crucial role in               | 用具体关系动词：shapes / constrains / enables / mediates         |
| has important implications for        | 直接说 implications 是什么                                     |
| provides a structured pathway to      | 删或改为 “links X to Y”                                      |
| It is worth noting that               | 删                                                        |
| A comprehensive framework that        | 说清楚框架的构成要素                                               |
| To the best of our knowledge          | 删                                                        |

**结构性 AI 痕迹**：

* 每段 150±20 词 → 段落长度过于整齐；
* 每段都有三件套 “A, B, and C” → 模板化并列；
* 连续 3 段以 This 开头 → 开头句式单一；
* 每个子节都以 “In summary,” 结尾 → 结构僵硬；
* 每个 Finding 都以 “The results show that...” 开头 → 报告性过重。

### C. 章节功能速查表

| 章节                  | 核心功能                | 一句话自测                            |
| ------------------- | ------------------- | -------------------------------- |
| Abstract            | 让读者在 15 秒决定读不读      | 不看正文能否知道 gap、做了什么、发现了什么？         |
| Introduction        | 让读者相信“这个 gap 值得填”   | gap 是否具体？贡献是否可追溯？                |
| Literature Review   | 让读者相信“你理解领域且找到合适位置” | 结尾有 Gap–Position–Contribution 吗？ |
| Methodology         | 让读者相信“方法选择有理、可复现”   | 先讲设计逻辑再讲细节了吗？                    |
| Results             | 让读者相信“证据稳健”         | 每段有 Claim 还是只有 Evidence？         |
| Discussion          | 让读者相信“你理解结果的复杂性”    | 有没有 Complication？有没有边界段落？        |
| Policy Implications | 让读者相信“结论有实际用途”      | 每条建议能否追溯到具体证据？                   |
| Conclusion          | 让读者带走一个清晰记忆         | 是摘要 2.0 还是真正的 take-away？         |

### D. 投稿前 5 分钟自检 Checklist

* [ ] 标题是功能式还是主题式？
* [ ] 摘要能独立表达核心发现？
* [ ] Introduction 的 gap 是否具体到可被挑战？
* [ ] Literature Review 结尾有 Gap–Position–Contribution？
* [ ] Methodology 每个子节开头是否说了“为什么这么做”？
* [ ] Results 每段第一句是判断还是话题？
* [ ] Discussion 中有至少一处 Complication？有可信度边界段落？
* [ ] Policy Implications 每条建议都能追溯到具体结果？
* [ ] Policy Implications 是否按行动者分层？
* [ ] Conclusion 避免了复述摘要和 Policy？
* [ ] 贡献段已从 bullet 转为散文？
* [ ] 图/表/引用/数据四向一致？
* [ ] 术语、缩写、变量记号全文一致？
* [ ] AI 模板句出现次数 < 2？
* [ ] 中式英语模式已清理？
* [ ] Hedging 强度与证据强度匹配？
* [ ] Keywords 与标题无过度重复？
* [ ] References 格式统一？每条引用都在正文中出现？

### E. Writing Pattern Library

**1. 重新框定结果的意义（Reframing findings）**

* `Rather than X, Y.`
* `What this finding really shows is not X, but Y.`
* `This is better understood as Y than as X.`

**2. 引入 Complication**

* `Although X, Y.`
* `Yet this reading is complicated by Z.`
* `This finding complicates the common assumption that...`
* `One likely reason is...`
* `At the same time, the result should be read with caution, because...`

**3. 对比论证（Structural contrast）**

* `By contrast, ...`
* `Unlike X, which assumes..., Y does not require...`
* `Where prior studies focus on X, the present study foregrounds Y.`

**4. 限定外推（Scoping generalization）**

* `This pattern is likely specific to X and may not generalize to Y.`
* `The result holds under X; whether it extends to Y remains open.`
* `These implications are specific to {DATA_SCOPE} and require further testing in other contexts.`

**5. 与文献对话**

* `This is consistent with A (2020) but contrasts with B (2022).`
* `A (2020) reports similar findings in X; the present study extends this to Y by...`
* `Whereas earlier work has focused on A, we show that B is equally central.`

**6. 机制层语言**

* `A likely mechanism is...`
* `One plausible channel through which X influences Y is...`
* `The effect is likely driven not by X itself but by...`

**7. 处理非显著结果**

* `The non-significant improvement in the {dimension} likely reflects...`
* `The absence of an effect is itself informative: it suggests...`
* `While directionally consistent, the result does not reach statistical significance, which may indicate limited evidence density in this sub-domain.`

**8. 政策—证据连接**

* `Based on the result that..., {actor} should...`
* `The implication for {actor} is therefore not X but Y: ...`
* `This follows from {specific finding in Section X.Y}.`

**9. 多来源综合引用**

* `Consistent evidence across X, Y, and Z (A; B; C) shows that...`
* `The literature converges on X, although the conditions under which it holds remain disputed (A; B).`

**10. 小节开头的 framing 句**

* `{Section/Subsection} considers whether...`
* `We now turn to the question of whether...`
* `The remainder of this section develops this argument in three steps.`

**11. 结构回声保留句式**

* `...a research pathway from *identifying* X to *organizing* Y and, finally, to *generating* Z.`
* `...the concerns are threefold—safety, cost, and accessibility—each shaping adoption through a distinct channel.`
* `The question has shifted from *who* uses X to *which* concerns constrain them and *how* policy can respond.`

**12. Dual-Gap 承接句**

* `{METHOD_B} opens an additional methodological space.`
* `A related but distinct limitation is that...`
* `The methodological challenge is therefore twofold: on the one hand, ...; on the other hand, ...`
* `Addressing both requires ...`

### F. Hedging Toolkit

**断言动词（Claim verbs）**

| 强度 | 动词                                    | 适用条件           |
| -- | ------------------------------------- | -------------- |
| 强  | shows, demonstrates, establishes      | 强实验证据、效应量大、复制过 |
| 中强 | indicates, reveals, confirms          | 显著性 + 理论一致     |
| 中  | suggests, points to                   | 方向明确但效应小或单研究   |
| 中弱 | is consistent with, aligns with       | 相关性而非因果性       |
| 弱  | may indicate, could be interpreted as | 证据较弱           |

**因果关系动词**

| 强度    | 动词                                  |
| ----- | ----------------------------------- |
| 强（因果） | causes, drives, determines          |
| 中（效应） | affects, shapes, influences         |
| 中（贡献） | contributes to, supports            |
| 弱（相关） | is associated with, correlates with |

**程度副词**

| 强度 | 词汇                                     |
| -- | -------------------------------------- |
| 强  | strongly, substantially, markedly      |
| 中  | considerably, meaningfully, noticeably |
| 弱  | modestly, somewhat, slightly           |

**情态 hedging**

| 强度  | 表达                        |
| --- | ------------------------- |
| 高概率 | will likely, is likely to |
| 中概率 | may, can, could           |
| 低概率 | might, may possibly       |

**使用规则**：

1. 同一断言中不要堆叠 hedges。
2. hedge 强度必须匹配证据强度。
3. 同一篇论文中，对同类结果应使用同类 hedge。
4. Policy Implications 中的动词强度应与 Results 中的动词强度匹配。

### G. Worked Example

#### 原中文段落

> 近年来，随着新兴交通方式的快速发展，其推广过程中的政策设计问题引起了学术界广泛关注。大量研究已经表明，影响公众采纳新型交通方式的因素是多方面的，包括价格、安全、便利性、社会影响等。值得注意的是，尽管既有研究已识别了诸多影响因素，但如何将这些因素转化为具体的推广政策，仍然缺乏系统性研究。本文认为，有必要在行为证据与政策设计之间建立更紧密的联系，从而为新型交通方式的推广提供更坚实的政策支撑。

#### Level 1：Naive Polish

```text
In recent years, with the rapid development of emerging transport modes,
the issue of policy design in their promotion has attracted wide attention
from academia. A large number of studies have shown that the factors
influencing public adoption of emerging transport modes are multi-dimensional,
including price, safety, convenience, and social influence. It is worth
noting that, although existing studies have identified various factors,
how to transform these factors into specific promotion policies is still
lacking systematic research. This paper believes that it is necessary to
establish a closer connection between behavioral evidence and policy design,
so as to provide a more solid policy support for the promotion of emerging
transport modes.
```

**问题诊断**：

* 时间框架前置 “In recent years...”；
* 抽象名词堆叠；
* AI 模板句；
* 话题句是主题而非判断；
* 缺少证据；
* 结尾空洞。

#### Level 2：Surface Polish

```text
Emerging transport modes have attracted growing attention, and their
adoption has been studied in terms of price, safety, convenience, and
social influence (Author A, Year; Author B, Year). Although these factors
are well identified, less is known about how to translate them into
concrete promotion policies. This study argues that behavioral evidence
and policy design should be more closely linked.
```

**仍然存在的问题**：

* 开头仍然是“话题陈述”而非“判断”；
* Gap 仍然空泛；
* 没有说明 “more closely linked” 具体指什么；
* 缺少 Position 和 Contribution 的对接。

#### Level 3：Deep Reframing

```text
The adoption of emerging transport modes depends on a relatively stable
set of factors—price, safety, convenience, and social influence (A, Year;
B, Year)—yet the literature remains stronger at identifying what shapes
adoption than at showing how these factors can be turned into policy
design. This asymmetry matters: without an explicit mapping between
adoption barriers and policy instruments, survey-based evidence tends
to generate recommendations that are either too generic to implement or
too specific to transfer. The present study addresses this asymmetry by
linking survey-identified barriers to policy instruments drawn from
analogous mobility systems, using a knowledge graph as the explicit
mapping layer.
```


---

## License & 使用建议

本框架为 Prompt 模板集合，可自由修改使用。

建议：

1. 先跑 Layer 0 的三个 Prompt：0.1 诊断 + 0.2 段落标注 + 0.3 术语表。
2. 根据诊断和附录 D 的 checklist 选取对应层级的 Prompt，而不是每次都跑全套。
3. 每次 Prompt 调用保留模型输出的“修改说明”部分，便于复盘。
4. 每完成一篇论文，用 Prompt 6.3 做一次版本跃迁分析，积累你自己的写作模式表。
5. 中文稿→英文稿的真实工作流走 Layer 7 → Layer 3–5，不要误走 Layer 0–6。

> 最好的 Prompt，是那个能让你写到第三篇论文时觉得“其实不太需要 Prompt 了”的 Prompt。
