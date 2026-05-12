# One-pager protocol — Claude Code skill

A skill that automates building a practical HTML one-pager: ask 5 pre-flight questions, apply structural + visual conventions, produce a self-contained HTML file. Iterate.

## What's here

| File | Role |
|---|---|
| `SKILL.md` | The skill itself. Copy/symlink into `~/.claude/skills/one-pager-protocol/` to activate. |
| `practical_one_pager_protocol.html` | Canonical reference — the protocol applied to itself. Read for visual conventions, CSS skeleton, and a worked example of the pre-flight → final-page mapping. **[View rendered](https://arnaudbru.github.io/resources/skills/one-pager-protocol/practical_one_pager_protocol.html)**. |

## Install

Copy both files (or symlink the whole folder) into your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/one-pager-protocol
cp SKILL.md practical_one_pager_protocol.html ~/.claude/skills/one-pager-protocol/
```

Or symlink the whole directory:

```bash
ln -s "$PWD" ~/.claude/skills/one-pager-protocol
```

Claude Code discovers the skill automatically on next session.

## Use

In a Claude Code session, either:

- **Explicit invocation**: `/one-pager-protocol`
- **Natural language**: *"make me a one-pager about X"*, *"create a one-pager for choosing Y"*, *"write me a cheat-sheet on Z"* — the skill's `description` field matches these.

The skill walks through 5 pre-flight questions (audience, usage context, the one-sentence question the page answers, specificity, shape), then writes a v0, then iterates with you.

## Applied examples

Real one-pagers built with this skill live at [`../ml-training/`](../ml-training/) in this repo:

- [`choosing_an_instance.html`](https://arnaudbru.github.io/resources/ml-training/choosing_an_instance.html) — decision-guide for picking the right GPU instance for training (VRAM / sys RAM / compute regime / cost lenses)
- [`choosing_dataset_patterns.html`](https://arnaudbru.github.io/resources/ml-training/choosing_dataset_patterns.html) — decision-guide for picking among PyTorch dataset patterns (in-memory / lazy / pre-decoded / sharded / features / streaming)

## How the protocol stays alive

The skill is **inductive**: when a new build surfaces a generalisable rule that wasn't in the protocol, it gets back-ported into `SKILL.md` and the canonical HTML's iteration-rules table. **Methodology is the residue of iteration, not the seed.**
