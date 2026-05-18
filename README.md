# spec-why-commenter

Adds spec-backed WHY comments to source code, linking implementation details to their specification sections. Also identifies **spec gaps** — code with no spec backing — and reports them with SPEC GAP comments and a summary table as discussion starters.

## Installation

### Per-agent quick reference

| Agent | CLI | Command |
|-------|-----|---------|
| **opencode** | `apm` | `apm install hiono/spec-why-commenter --target opencode` |
| **opencode** | `npx skills` | `npx skills add hiono/spec-why-commenter` |
| **Claude Code** | `apm` | `apm install hiono/spec-why-commenter --target claude` |
| **Cursor** | `apm` | `apm install hiono/spec-why-commenter --target cursor` |
| **Cursor** | `npx skills` | `npx skills add hiono/spec-why-commenter` |
| **Windsurf** | `npx skills` | `npx skills add hiono/spec-why-commenter` |
| **Copilot** | `apm` | `apm install hiono/spec-why-commenter --target copilot` |
| **Gemini Code Assist** | `apm` | `apm install hiono/spec-why-commenter --target gemini` |
| **Codex CLI** | `apm` | `apm install hiono/spec-why-commenter --target codex` |

### npx skills (Vercel Labs)

Works with opencode, Cursor, Windsurf, and other Vercel-harness agents:

```bash
npx skills add hiono/spec-why-commenter
```

### APM (Agent Package Manager)

Works with all targets defined in `apm.yml`:

```bash
# Install without target flag (uses all targets from apm.yml)
apm install hiono/spec-why-commenter

# Or specify your agent explicitly
apm install hiono/spec-why-commenter --target opencode
```

## Usage

Ask your AI coding assistant to annotate source code with spec references. The skill supports:

- **Pattern 1** — Direct spec citation: `// SPEC §X: description`
- **Pattern 2** — WHY comment: `// SPEC §X: description` + `// WHY: rationale`
- **Pattern 3** — Multiple spec references: `// SPEC_A §X / SPEC_B §Y: ...`
- **Pattern 4** — Spec gap: `// SPEC GAP: undocumented behavior`

See [SKILL.md](./SKILL.md) for the full workflow and examples.
