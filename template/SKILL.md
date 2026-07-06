---
name: [Name. Required. Must be 1-64 characters. May only contain unicode lowercase alphanumeric characters (a-z, 0-9) and hyphens (-). Must not start or end with a hyphen (-). Must not contain consecutive hyphens (--). Must match the parent directory name]
description: [Description. Required. Must be 1-1024 characters. Should describe both what the skill does and when to use it. Should include specific keywords that help agents identify relevant tasks.
license: [Optional. Specifies the license applied to the skill. Name of a license (use SPDX e.g. 'Apache-2.0'). Path to a bundled license file if provided]
compatibility: [Optional. Must be 1-500 characters if provided. Must indicate skills required, tools, state. Should only be included if your skill has specific environment requirements. Can indicate intended product, required system packages, network access needs, etc.]
allowed-tools field: [Optional. A space-separated string of tools that are pre-approved to run]
metadata:
	status: draft / active / deprecated
	version: 1.0.0
	updated: YYYY-MM-DD
	author: [name/team]
	source: https://github.com/
	owner: [name/team]
	tags: [comma separated keywords]
---

# Skill: [Name]

- **Purpose:** [One sentence]
- **Use when:** [Trigger]
- **Don't use when:** [Out of scope]
- **Success looks like:** [Observable outcome]

## Inputs
| Name | Type | Required | Default | Constraints | Description |
|------|------|----------|---------|-------------|-------------|
| `[input]` | `string` | Yes/No | — | [format/length] | [What it is] |

**Assumptions:** [What must be true for this skill to work]

## Output
**Format:** [Markdown / JSON / code / plain text]
**Template:**
```
[Literal structure with placeholders]
```

## Reasoning
1. **Understand** — [verify/extract]
2. **Plan** — [decide/decompose]
3. **Generate** — [produce]
4. **Verify** — [check before submitting]

**Expose reasoning?** Yes / No
**When uncertain:** Ask / Proceed with assumption / Refuse

## Decision Rules
| If | Then |
|----|------|
| [condition] | [action] |
| [condition] | [action] |
| Otherwise | [default] |

## Guardrails

**Must:** [non-negotiable] · [non-negotiable]
**Must not:** [prohibited + why] · [prohibited + why]

**Priority order when conflict:** [1] > [2] > [3] > [4]

## Style
- **Voice/Formality:** [e.g., imperative, formal]
- **Audience:** [Who reads this]
- **Length:** [Target or "as needed, no longer"]
- **Format/Language:** [Markdown / specific terminology to use or avoid]

## Edge Cases
| Situation | Response |
|-----------|----------|
| [Missing/invalid input] | [action] |
| [Out of scope] | [refuse / redirect] |
| [Conflicting constraints] | [which wins] |
| [Adversarial input] | [refuse / sanitize] |

## Self-Check (run before returning)
- [ ] Correctness — [criterion]
- [ ] Format — [criterion]
- [ ] Style — [criterion]
- [ ] Safety — [no PII/fabrication/harm]
- [ ] Completeness — [all parts addressed]

**If a check fails after 2 attempts:** [fallback — partial result / ask / refuse]

## Examples

**✅ Good:** Input → Output (1–2 lines on why it works)

**❌ Bad:** Output → corrected version (1 line on what failed)

## Change log
- v1.0.0 — initial: [description]

