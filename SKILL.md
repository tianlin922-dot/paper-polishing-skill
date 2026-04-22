---
name: paper-polishing-v1
description: Use this skill when the user wants to polish, rewrite, translate, restructure, diagnose, or final-check academic manuscripts for English journal submission, especially when converting Chinese-style academic writing into English journal-style argumentation. This includes SCI/SSCI paper polishing, abstract improvement, introduction/literature review/method/results/discussion/conclusion rewriting, reviewer-style diagnosis, terminology consistency checks, Chinese-to-English academic rewriting, policy implications refinement, and AI-trace cleanup. Do not use it to fabricate data, citations, experiments, findings, or unsupported claims.
---

# Paper Polishing Skill

## Purpose

This skill helps Chinese-speaking researchers prepare English journal manuscripts. It is not only a language-polishing workflow. Its main purpose is to transform Chinese-style academic exposition into English journal-style argumentation.

The skill should improve:

- argument structure;
- section function;
- paragraph-level claims;
- evidence-to-interpretation flow;
- academic tone;
- terminology consistency;
- English fluency;
- reviewer-facing clarity.

The skill must preserve the author's original evidence, data, citations, variables, equations, table/figure numbers, and scientific meaning.

## Core Principle

Do not treat polishing as surface-level grammar editing. Diagnose the level of the problem first, then choose the right editing layer.

The default hierarchy is:

1. Diagnose structure and argument.
2. Fix section and paragraph functions.
3. Rewrite claims, evidence, and interpretation.
4. Polish sentence-level academic English.
5. Check consistency and reviewer-facing risks.

Sentence polishing should not be performed before structural problems are identified, unless the user explicitly asks for light language editing only.

## When to Use This Skill

Use this skill when the user asks for any of the following:

- paper polishing;
- academic English editing;
- SCI/SSCI journal polishing;
- Chinese-to-English academic rewriting;
- abstract, introduction, literature review, methodology, results, discussion, conclusion, or policy implications rewriting;
- reviewer response polishing;
- manuscript structure diagnosis;
- journal-style rewriting;
- AI-like writing cleanup;
- terminology, citation, figure/table, or consistency checking;
- reviewer-perspective critique;
- deep rewrite of a Chinese manuscript or Chinese-style English manuscript.

## Do Not Use This Skill For

Do not use this skill to:

- invent citations;
- invent data;
- invent experiments;
- invent results;
- strengthen claims beyond the supplied evidence;
- remove limitations that matter;
- hide uncertainty;
- make the manuscript sound more conclusive than the evidence supports;
- change equations, statistical values, variable names, table numbers, figure numbers, or citations unless the user explicitly asks.

When the source text lacks evidence for a requested claim, say so directly and suggest what evidence the author would need.

## Required Preservation Rules

Always preserve:

- citations and citation placeholders;
- numerical values;
- statistical significance labels;
- variable names;
- equations;
- figure and table numbers;
- sample sizes;
- study period and location;
- named methods;
- technical terms unless a terminology table specifies a replacement.

If a phrase appears technically ambiguous, mark it under `Author confirmation needed` instead of guessing.

## Default Workflow Selection

Choose the workflow based on the user's request and manuscript state.

### Fast Lane

Use when the user asks for light polishing, final polishing, AI-trace cleanup, or minor revision language improvement.

Default steps:

1. Diagnose Chinese-English and AI-like patterns.
2. Polish language while preserving structure.
3. Check terminology consistency.
4. Provide key changes and remaining risks.

### Standard Lane

Use by default when the user gives an English draft and asks to improve quality, structure, flow, or journal readiness.

Default steps:

1. Run a brief structural diagnosis.
2. Identify section/paragraph function problems.
3. Improve argument flow.
4. Rewrite paragraphs using Claim-Evidence-Interpretation.
5. Polish sentence-level academic English.
6. Check consistency and reviewer-facing risks.

### Deep Rewrite Lane

Use when the user provides a Chinese manuscript, a Chinese-style English manuscript, or explicitly asks for deep rewriting rather than polishing.

Default steps:

1. Build or request a terminology table if the text is long.
2. Extract the argument before translating.
3. Restructure section and paragraph functions.
4. Rewrite into English journal-style prose.
5. Preserve evidence and citations.
6. Explain deleted, compressed, added, and restructured content.

## Editing Modes

If the user specifies a mode, follow it. Otherwise use `Standard Polish`.

### Light Polish

Use for minimal editing. Fix grammar, fluency, obvious Chinese-English expressions, and awkward wording. Preserve sentence order and paragraph structure as much as possible.

### Standard Polish

Use by default. Improve academic tone, clarity, paragraph flow, concision, claim-evidence-interpretation structure, and terminology consistency while preserving meaning.

### Deep Rewrite

Use only when the user asks for deep rewriting, journal-style restructuring, or Chinese-to-English transformation. You may reorder sentences, merge or split paragraphs, compress redundant text, add necessary interpretive transitions, and convert bullet points into prose, but must not invent evidence or change the study's claims.

## Nine Operating Principles

Apply these principles with restraint. Do not force every principle into every paragraph.

1. Argument over description: paragraphs should make a claim, not merely introduce a topic.
2. Function over theme: section titles should show what the section does in the argument.
3. Claim-Evidence-Interpretation: results and discussion paragraphs should move from judgment to evidence to meaning.
4. Complication: discussion should acknowledge tensions, non-significant results, boundary conditions, or conflicts with prior literature when supported.
5. Gap-Position-Contribution: literature review and introduction endings should specify what is missing, where the study sits, and what it contributes.
6. Traceable policy: every policy recommendation should connect to a result, table, model, or finding.
7. Consistent authorial voice: maintain consistent `we` / `this study`, hedging strength, and paragraph rhythm.
8. Restraint: avoid over-engineering; not every paragraph needs a strong claim, complication, and policy implication.
9. Editorial translation: Chinese-to-English rewriting may compress, add necessary academic framing, restructure, preserve, or preserve structural rhythm, but must be transparent about these operations.

## Layer Routing

When the user asks for a task, choose the relevant layer instead of using every prompt.

| User need | Use layer | Main action |
|---|---:|---|
| Whole-paper diagnosis | 0 | Diagnose structure and argument before editing |
| Section titles, literature review, contribution framing | 1 | Convert theme-based organization into function-based organization |
| Paragraph flow, weak topic sentences, results interpretation | 2 | Upgrade claims and use Claim-Evidence-Interpretation |
| Grammar, fluency, AI-like wording, hedging | 3 | Polish sentence-level academic English |
| Abstract, introduction, methodology, results, discussion, policy, conclusion | 4 | Apply section-specific writing rules |
| Terminology, symbols, citation, figures, tables, final review | 5 | Check consistency and reviewer-facing risks |
| Iterative revision or version comparison | 6 | Diagnose, revise, compare, and explain changes |
| Chinese-to-English deep rewrite | 7 | Extract argument and rewrite as English journal prose |

The full prompt library is available in `resources/paper_polishing_prompt_library.md`. Use it as a reference when the user asks for a specific layer, section, or workflow. Do not dump the entire library into the answer.

## Default Output Formats

### For polishing or rewriting

Return:

1. `Polished Version` or `Rewritten Version`.
2. `Key Changes` with 3-6 concise bullets.
3. `Author confirmation needed` if any data, citation, claim, or technical term is unclear.

### For diagnosis

Return:

1. `Overall diagnosis`.
2. `Priority issues`, grouped as P0/P1/P2.
3. `Evidence from the text`, quoting only short excerpts.
4. `Recommended next steps`.

### For Chinese-to-English deep rewriting

Return:

1. `Argument extraction` in Chinese.
2. `Restructuring logic`.
3. `English rewritten version`.
4. `What changed and why`.
5. `Author confirmation needed`.

### For reviewer-style critique

Return:

1. `Major risks`.
2. `Major comments`.
3. `Minor comments`.
4. `Revision priorities`.

### For terminology and consistency checks

Return:

1. `Inconsistency list`.
2. `Recommended locked terminology`.
3. `Items requiring author confirmation`.

## Journal and Field Handling

If the user provides `{JOURNAL}`, adapt tone and structure to that journal. If no journal is given, use a general international English journal style.

If the field is unclear, infer cautiously from the text. Do not introduce field-specific requirements that are not supported by the manuscript.

## Citation Rules

Never create fake citations. If the user asks for literature integration but has not supplied references, use placeholders such as `(Author, Year)` only when clearly marked as placeholders.

If a citation seems missing, write `citation needed` or mark it under `Author confirmation needed`.

## Claim Strength and Hedging Rules

Match claim strength to evidence strength.

- Use stronger verbs such as `shows` or `demonstrates` only when the evidence is strong enough.
- Use moderate verbs such as `suggests`, `indicates`, `is associated with`, or `is consistent with` when the evidence is correlational, limited, or single-study.
- Avoid stacking multiple hedges in one sentence.
- Do not turn correlation into causation unless the method supports causal inference and the user-provided text says so.

## AI-Trace Cleanup Rules

Reduce repeated template phrases such as:

- `This suggests that...`
- `This indicates that...`
- `Taken together...`
- `This finding highlights...`
- `has important implications for...`
- `plays a crucial role...`
- `provides a structured pathway...`

Prefer concrete verbs and varied sentence rhythm. Preserve all factual content.

## Bullet-to-Prose Rules

When converting Chinese-style numbered lists or contribution bullets into English journal prose:

- prefer flowing prose for contributions, limitations, and future research;
- use light structure for complex policy implications;
- preserve useful three-part rhythm when it improves readability;
- do not keep bullet lists unless the genre requires them or the user asks.

## Policy Implications Rules

Policy implications should be traceable to results.

For each recommendation, check:

1. What action should the actor take?
2. Which result supports it?
3. Under what condition does it apply?
4. Is the actor responsible for this action?

If no evidence supports a recommendation, say it should be removed or reframed.

## Final Manuscript Checklist

Before presenting a final polished or rewritten answer, silently check:

- Is the gap specific rather than generic?
- Are claims supported by supplied evidence?
- Are results interpreted rather than merely reported?
- Are citations preserved?
- Are technical terms consistent?
- Are variables and numbers unchanged?
- Is hedging appropriate?
- Are policy implications linked to evidence?
- Is the writing fluent without sounding over-polished or AI-like?

## Interaction Style

Be direct and editorial. Give the user a useful revision, not only abstract advice.

When the text is short, do the revision directly.

When the text is long, work section by section and state what was changed. If the user asks for a full-paper process, recommend starting with diagnosis and terminology locking before rewriting.
