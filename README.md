# frua.fr - Skills

This repository contains agent skills (folders of instructions, scripts, and resources) that your AI harness (Claude Code, OpenCode, Pi, etc) loads automatically to improve its performance on specialized tasks.

## Setup

The content of the [./skills](./skills) folder should be copied to the relevant skills folder: 

- Compatible Projet Claude: .claude/skills/<name>/SKILL.md
- Compatible Global Claude: ~/.claude/skills/<name>/SKILL.md
- Compatible avec l’agent de projet: .agents/skills/<name>/SKILL.md
- Compatible avec les agents globaux: ~/.agents/skills/<name>/SKILL.md

Each skill is self-contained in its own folder with a SKILL.md file containing the instructions and metadata that your harness uses.

This folder may contain subfolders (references, scripts, assets, etc) used by the skill.

## List of skills

| **Skill** | **Description** |
| - | - |
| [incus-doc](./skills/incus-doc) | Offline authoritative [Incus](https://linuxcontainers.org/incus/docs/main/) CLI help and documentation. Reduces hallucinations by routing all claims through verified reference files. (incus v. 6.0 as at 2026-07-06)|

## License

Each skill subfolder contains a LICENSE.txt file detailing the License for the specific skill. By default, these skills are released under the open source Apache-2.0 License. 

## Disclaimer

These skills are provided for demonstration and educational purposes only. Given the probabilistic nature of the LLM technology, the implementation and behavior of your harness may differ from what is shown in these skills.

Always test skills thoroughly in your own environment before relying on them for critical tasks.

## Author

David HEURTEVENT <david@heurtevent.org> for frua.fr

## Development

The template folder contains a skill template [SKILL.md](./template/SKILL.md) compatible with the [Agent Skills open format](https://agentskills.io/home). This skill template expands the open format with best practices : input, output, reasoning, decision rules, guardrails, style, edge cases, self check, examples, change log.
