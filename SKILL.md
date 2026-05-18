---
name: spec-why-commenter
description: >
  Adds spec-backed WHY comments to source code, linking implementation
  details to their specification sections. Also identifies "spec gaps" —
  code with no spec backing — and reports them with SPEC GAP comments and
  a summary table as discussion starters.

  Use this skill when the user wants to annotate source code with spec
  section references, explain WHY code behaves a certain way via a spec,
  map implementation to hardware/protocol/design specs, or surface
  undocumented behavior. Always use this skill when the user mentions:
  仕様書コメント, 仕様書未記載, spec gap, WHYコメント追加,
  仕様書との対応, 仕様書のどのセクション, コードと仕様書をマッピング.
  Also trigger on: "add spec comments", "WHY comments", "link code to spec",
  "spec gap", "annotate with spec", "仕様書に根拠を".

  Works with any readable spec format and any language with comment syntax.
---

# Spec WHY Commenter

Add spec-backed WHY comments to source code and extract undocumented behavior
(spec gaps) as discussion starters for the team.

## Before You Start

Confirm three things before writing any comment:

1. **Spec location** — ask the user, or look under `docs/`, `spec/`, `SPEC/`
2. **Target files** — if unspecified, prioritize files with register access,
   state machines, or math operations (highest spec-reference density)
3. **Spec short name** — decide the abbreviation to use in comments
   (e.g. `SPEC`, `RFC`, `UM`, `DATASHEET`)

## Comment Language

Match the language of the surrounding comments in the file:
- **English comments in the file → write new comments in English**
- **Japanese comments in the file → write new comments in Japanese**
- If the file has no comments yet, default to English.

## Comment Format

Write comments in order: spec citation first, then WHY. Place them **above**
the relevant line(s). Never change code logic — only add comments.

### Pattern 1: Direct spec citation

Use when the spec directly describes what the code does.

```
// <SPEC_NAME> §<section>: <what the spec says>.
<code>
```

Example (C++):
```cpp
// SPEC §3.4: MODE[1:0] selects the operating mode; bit2 and above are reserved.
current_mode_ = static_cast<Mode>(value & 0x3U);
```

Example (Python):
```python
# RFC 7159 §6: Numbers that cannot be represented as IEEE 754 double MUST be rejected.
if math.isinf(value) or math.isnan(value):
    raise ValueError("Non-finite number")
```

### Pattern 2: WHY comment (rationale)

Use when the spec describes the behavior, but it's not obvious *why* the code
is written this particular way.

```
// <SPEC_NAME> §<section>: <brief spec description>.
// WHY: <reason this implementation choice was made, citing spec if possible>.
<code>
```

Example:
```cpp
// SPEC §3.20: CLR register is write-only, write-1-to-clear.
// WHY: OVF (bit1) is cleared by reading STATUS register, not by CLR (SPEC §3.19).
//      RDY (bit0) is a live status bit — not a latched flag — so no CLR bit exists.
write_mask_ = 0x02U;
```

### Pattern 3: Multiple spec references

When two or more spec documents govern the same behavior, cite both.

```
// <SPEC_A> §<X> / <SPEC_B> §<Y>: <combined description>.
<code>
```

### Pattern 4: Spec gap (undocumented behavior)

When no matching spec section exists, mark it as a SPEC GAP.

```
// SPEC GAP: This behavior is not documented in <SPEC_NAME(s)>.
// WHY: <hypothesis about why this code exists>
// ACTION: Confirm with <owner/team> whether this is intentional or a spec omission.
<code>
```

Common spec gap candidates:
- Error handling paths not mentioned in the spec
- Boundary/invalid-input behavior
- Hardware bug workarounds
- Performance optimizations with no spec basis
- Simulator-specific abstractions (e.g. TLM simplifications)

## Workflow

### Step 1: Read the spec

- Read the spec sections relevant to the target code first
- For long specs, scan the table of contents to identify relevant sections,
  then read only those in detail
- Keep the short name → section mapping in mind as you annotate

### Step 2: Map code to spec

Focus on these code patterns as you read the source:

| Code pattern | Why it matters |
|---|---|
| Register reads/writes | Field definitions and access constraints appear in specs |
| State machine transitions | Spec diagrams and conditions often prescribe these exactly |
| Arithmetic / rounding | Algorithm origin is typically in the spec |
| Bit masks / field extractions | Bit field meanings are spec-defined |
| Error handling | If spec-described → WHY; if not → SPEC GAP |

### Step 3: Add comments

1. Choose Pattern 1–4 based on the situation
2. Place comments **directly above** the relevant line (not inline at end-of-line)
3. Keep citations concise: section number + 1–2 sentences is enough
4. Write WHY as *why this implementation choice*, not *what it does*
5. **Never change code logic — comments only**

### Step 4: Report spec gaps

After annotating, if any spec gaps were found, output a summary:

```
## Spec Gap Summary

| File | Line | Description | Action |
|------|------|-------------|--------|
| foo.cpp | 42 | Error path not in SPEC | Confirm with HW team |
| bar.py  | 17 | Retry logic undocumented | Check if intentional |
```

## Tips

- Read the spec **before** writing. Guessing from code leads to false citations.
- If no spec section matches, use SPEC GAP — never force a citation.
- Repeat the citation at each usage site even if the same section was cited
  nearby — readers don't always read top to bottom.
- If a comment grows long, cut it to the single most important WHY sentence.
