# AGENTS.md

For this repository, follow these rules strictly:

## Current task focus

Fix Financial PDF_NON_MC Question Sheet output only.

## Hard constraints

* Do not change MC parsing
* Do not change Managerial logic
* Do not change Answer Sheet behavior
* Do not change worksheet truncation behavior
* Do not refactor unrelated code
* Prefer the smallest safe change

## Strong implementation preference

* Prefer detecting and preserving compact setup-table structure in the parser layer, not by guessing layout during final rendering
* Reuse existing parsed structure whenever possible
* Avoid heuristic formatting-only fixes in `generateQuestionSheet()`
* When a compact setup table is detected, store it as structured row/column data for later rendering

## Table rendering rule

For compact question-side setup tables like CH8-12:

* Do not output them as plain paragraphs
* Do not align them with spaces
* They are only considered fixed if rendered with a real `docx.Table`

## Detection rule

* First isolate question-only content using existing worksheet truncation logic
* Only then detect compact setup tables inside question-only content
* Never render worksheet / answer-entry / solution tables
* Only detect small, clearly tabular blocks with consistent row structure
* Do not convert narrative lines into tables
* Only render compact setup tables that belong to the question content before `Required:` or before worksheet UI begins

## Narrow-case rule

* For now, support only clearly structured compact setup tables such as CH8-12
* Do not introduce a generic PDF table reconstruction system

## Fallback rule

* If no clear compact setup table is detected, keep the existing paragraph rendering path unchanged

## Scope guard

* Limit changes to the Financial PDF_NON_MC flow only
* Do not modify shared helpers unless absolutely necessary
* If a shared helper must change, keep the change minimal and explain why it does not affect other paths

## Change review rule

After editing:

* show exact changed function blocks
* identify where structured table data is created
* identify where `new docx.Table(...)` is created
* identify where the structured table data is passed into Question Sheet rendering
* explain risks
* explain why the change does not affect MC, Managerial, or Answer Sheet paths

## Output format requirement

When proposing or applying a fix:

* provide exact edited function blocks, not summaries
* provide a minimal diff when possible
* do not describe a change without showing the changed code
* changes must be applied to the actual file on disk, not only described in prose
