---
name: incus-doc
description: Ground AI responses and scripts in offline authoritative Incus CLI help and documentation. Must be loaded before using incus, writing scripts that invoke incus CLI commands, invoking incus CLI commands directly, or answering questions about incus. Reduces hallucinations by routing all claims through verified reference files.
license: Apache-2.0
compatibility: Requires incus-doc/references/cli/ (per-command help files), incus-doc/references/man/ (per-command man pages), incus-doc/references/docs/ (markdown documentation), incus-doc/references/cli-help.md (command index), incus-doc/references/cli-man.md (man page index), and incus-doc/references/docs/index.md (doc index) in the skill folder.
allowed-tools: read glob grep bash
metadata:
  status: active
  version: 1.2.0
  updated: 2026-07-06
  author: David HEURTEVENT
  source: https://github.com/fruafr/skills/tree/main/skills/incus-doc
  owner: David HEURTEVENT
  tags: incus, container, virtual-machine, virtualization, cli, documentation, hallucination-reduction, lxc
---

# Skill: incus-doc

- **Purpose:** Ground every Incus-related answer, command, and script in authoritative CLI help and documentation to eliminate hallucinations.
- **Use when:** The user asks a question about Incus, asks to write a script using incus CLI commands, asks to run incus commands, or any context where Incus is mentioned.
- **Don't use when:** The question is purely about general Linux/container concepts unrelated to Incus syntax or behavior.
- **Success looks like:** Every incus CLI invocation, flag, subcommand, and configuration option is verified against the skill's reference files before being emitted; no fabricated flags, subcommands, or behaviors.

## Inputs

This skill does not require explicit inputs — it activates on context (incus mentions). The skill's own reference files serve as the input.

**Assumptions:**
- The `references/` directory within the skill folder (`incus-doc/references/`) exists and is up to date.
- `incus-doc/references/cli-help.md` contains the authoritative list of all incus commands and subcommands with links to per-command help in `incus-doc/references/cli/`.
- `incus-doc/references/cli-man.md` contains the authoritative list of all incus man pages with links to per-command man pages in `incus-doc/references/man/`.
- `incus-doc/references/docs/index.md` contains the authoritative documentation tree with links to markdown files in `incus-doc/references/docs/`.

## Output

**Format:** Grounded answer / corrected script / verified command

Every incus command, flag, or configuration option **must** be traceable to a reference file. If the reference does not contain the claim, the claim must not be emitted.

## Reasoning

1. **Understand** — Parse the user request; identify all incus commands, subcommands, flags, and configuration options.
2. **Plan** — Determine which reference files to consult:
   - Command/subcommand existence and flags → `incus-doc/references/cli-help.md` then `incus-doc/references/cli/<command>.txt`
   - Man page lookup (man-page-style options, descriptions) → `incus-doc/references/cli-man.md` then `incus-doc/references/man/<command>.txt`
   - Conceptual/configuration/documentation/REST API → `incus-doc/references/docs/index.md` then relevant markdown file in `incus-doc/references/docs/`
3. **Generate** — Produce answer with every incus-specific element verified.
4. **Verify** — Re-read relevant reference lines to confirm every flag, subcommand, and option exists.

**When uncertain:** Consult reference files. If not covered, state "Not found in the Incus reference documentation."

## Decision Rules

| If | Then |
|----|------|
| User asks about incus commands | Open `incus-doc/references/cli-help.md`, find command, open linked `incus-doc/references/cli/<file>.txt`, answer from it |
| User asks about incus concepts/config | Open `incus-doc/references/docs/index.md`, find relevant doc page, open linked `incus-doc/references/docs/<file>.md`, answer from it |
| User asks about incus man page details | Open `incus-doc/references/cli-man.md`, find command, open linked `incus-doc/references/man/<file>.txt`, answer from it |
| User asks to write a script with incus | Verify every incus command/flags against its `incus-doc/references/cli/<file>.txt` |
| User invokes incus command directly | Verify command+flags against `incus-doc/references/cli/` before running |
| Not in references | "This is not covered in the bundled Incus reference documentation." |
| Subcommand lookup | Drill into `incus-doc/references/cli/<parent>_<subcommand>.txt` |

## Guardrails

**Must:** Consult references before every incus claim · cite reference file path · verify every flag and subcommand exists before using · open specific help/doc file for exact command/page.

**Must not:** Invent flags, subcommands, or config options · guess incus CLI syntax · output unverified incus command · reference non-existent documentation paths.

**Priority:** [1] Exact help file text > [2] Documentation markdown > [3] No answer

## Edge Cases

| Situation | Response |
|-----------|----------|
| Missing reference files | Refuse: "Incus reference files not found." |
| Command exists but flag not in help | Don't use the flag |
| Feature not in docs | "Not covered in bundled Incus reference documentation." |
| User asks about LXD (not incus) | Clarify: "This skill covers Incus, not LXD." |
| Conflicting CLI help vs docs | Trust CLI help file over documentation |
| Script with many commands | Verify each command+flags independently |

## Self-Check (run before returning)
- [ ] Correctness — every incus command/flag/subcommand verified against a reference file
- [ ] Format — output contains reference file paths for traceability
- [ ] Safety — no fabricated Incus features
- [ ] Completeness — all incus-related parts addressed

**If a check fails after 2 attempts:** Refuse with "Unable to ground the answer in the Incus reference documentation."

## Examples

**✅ Good:** User asks "How do I create an incus container?" → Agent opens `incus-doc/references/cli-help.md`, finds `incus create` → `incus-doc/references/cli/incus_create.txt`, reads flags, answers with verified command.

**❌ Bad:** User asks "Run `incus init mycontainer`" → Agent guesses `init` is valid, but it doesn't exist in the reference files. Correct: use `incus create` or `incus launch`.

## Change log
- v1.0.0 — initial: Ground all Incus-related AI output in bundled CLI help and documentation references to eliminate hallucinations.
- v1.1.0 — Updated all reference paths to use absolute paths pointing to the `references/` directory within the skill folder (`incus-doc/references/`).
- v1.2.0 — Added `cli-man.md` (man page index) alongside `cli-help.md` and `docs/index.md`.
