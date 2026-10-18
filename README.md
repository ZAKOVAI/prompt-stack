# Prompt Stack

A skill that turns a rough request into a four-layer, paste-ready prompt package.

## What it does

Prompt Stack writes the prompt and stops there. It returns one package you can paste into the tool that will do the work: the instruction, the context that tool should see, the constraints that bound it, and the checks that decide when it is done.

Invoke it with `/prompt-stack` plus the raw request, or by asking for a prompt to paste into another agent. If the target tool is missing, the skill asks for that and stops.

## The four layers

1. **Prompt.** The instruction the target model receives. Hard constraints sit in the first 30%. The shape matches the tool: file anchors and a done-check for Cursor, start state and stop conditions for Claude Code or Devin, comma-separated descriptors for Midjourney, a short goal for reasoning models.
2. **Context pack.** Only what this task needs, in a fixed order: always-on rules, the relevant spec slice, the files to read, the live error or screenshot, and a short handoff. Stable rules come first. The active task comes last.
3. **Constraints.** Four surfaces. Locked artifacts (rubric, tests, merge policy) can be read and cannot be rewritten to pass. Editable paths are listed. The result log is append-only. Deletes, dependencies, schema, credentials, deploy, merge, spend, and gated releases stop for a human.
4. **Eval loop.** Deterministic checks run before any rubric. Dimensions are scored separately. A model judge is used only when the check cannot be mechanical. The loop keeps a better result, rolls back a worse one, and stops on a binary pass, a budget, or a human gate.

Agentic targets also get **Proof required** (what must exist before anyone says done) and **Stop-and-ask** (what must halt for the user). One-shot tools still receive layers 3 and 4, marked `one-shot`.

Worked packages are in [`examples/`](examples/).

## Install

The folder that contains `SKILL.md` must be named `prompt-stack`. The `name` in the frontmatter has to match that directory.

**Claude Code** (user skills):

```bash
git clone https://github.com/ZAKOVAI/prompt-stack.git ~/.claude/skills/prompt-stack
```

**Cursor** (project skills):

```bash
mkdir -p .cursor/skills
git clone https://github.com/ZAKOVAI/prompt-stack.git .cursor/skills/prompt-stack
```

**Cursor** (user skills): copy the same folder to `~/.cursor/skills/prompt-stack/`.

Any other agent that loads a skills directory uses the same layout: a `prompt-stack` folder with this `SKILL.md` inside. `examples/` is optional. The skill runs from `SKILL.md` alone.

## Before and after

Request: "write a prompt so Cursor fixes the flaky checkout test."

Before, a typical reply is one paragraph: fix the test, make it pass, and do not break anything. No file, no locked surface, no proof, no stop condition.

After, the package names the test file as the only editable path, points at the failing assertion and one trusted pattern file, locks the test helper, requires the test command output before "done," and stops if the fix would need a new dependency or a schema change. The full package is [`examples/agentic-test-fix.md`](examples/agentic-test-fix.md). A one-shot image brief is [`examples/one-shot-product-still.md`](examples/one-shot-product-still.md).

## Credits

Prompt Stack distills public methods. Read the sources for the theory.

- Prompt: [prompt-master 1.8.0](https://github.com/nidhinjs/prompt-master) by nidhinjs. That version is listed in the repository history. `main` may have moved on.
- Context: [context-engineering](https://github.com/addyosmani/agent-skills/blob/main/skills/context-engineering/SKILL.md) by Addy Osmani, in [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills).
- Constraints: [harness-engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering/blob/main/skills/harness-engineering/SKILL.md) by muratcankoylan, in [Agent-Skills-for-Context-Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering).
- Loops: [evaluation](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering/blob/main/skills/evaluation/SKILL.md) and [advanced-evaluation](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering/blob/main/skills/advanced-evaluation/SKILL.md), from the same collection. The judge skill applies only when a judge is required.

## Author

Zach Benhamou ([ZAKOVAI](https://github.com/ZAKOVAI))

## License

[MIT](LICENSE). Copyright 2026 Zach Benhamou.
