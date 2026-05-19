# spec-why-commenter

Adds spec-backed WHY comments to source code, linking implementation details to their specification sections. Also identifies **spec gaps** — code with no spec backing — and reports them with SPEC GAP comments and a summary table as discussion starters.

## Installation

Choose your package manager:

### npx skills

```bash
npx skills add hiono/spec-why-commenter
```

Compatible with opencode, Cursor, Windsurf, and other Vercel-harness agents.

### apm

```bash
apm install hiono/spec-why-commenter --target opencode
```

Replace `opencode` with your agent (`claude`, `cursor`, `copilot`, `gemini`, `codex`, `windsurf`). Omit `--target` to install for all configured targets.

## Usage

Ask your AI coding assistant to annotate source code with spec references. The skill supports:

- **Pattern 1** — Direct spec citation: `// SPEC §X: description`
- **Pattern 2** — WHY comment: `// SPEC §X: description` + `// WHY: rationale`
- **Pattern 3** — Multiple spec references: `// SPEC_A §X / SPEC_B §Y: ...`
- **Pattern 4** — Spec gap: `// SPEC GAP: undocumented behavior`

See [SKILL.md](./SKILL.md) for the full workflow and examples.
