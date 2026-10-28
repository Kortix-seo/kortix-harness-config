# kortix-harness-config

A working agent-harness configuration for an open-source Kortix project: two agents, the grants each one holds, a scheduled trigger, and the files the harness reads at boot. Copy it, cut it down, and run it on your own machine.

Kortix is the open-source AI Operating System. This repository is not a framework and it is not a demo: it is the project directory a Kortix session boots into, written the way a team would keep it — every file versioned, every grant reviewable as a diff.

## What a harness configuration actually contains

An agent harness is the runtime that turns a model into an agent that can do work: the loop, the tools, the context, and the rules about what the agent may touch. The Kortix manifest owns the last part, and this repo is arranged so the other parts stay swappable around it.

| Path | What it owns | Who reads it |
|---|---|---|
| `kortix.yaml` | The project's governance: which agents exist, what each may reach, which triggers run | The platform, at boot |
| `agents/<name>.md` | One agent's behaviour: description, mode, prompt body | The harness, on every turn |
| `skills/release-notes/SKILL.md` | A procedure an agent loads when the task matches | Every harness |
| `harnesses/` | Files only one harness implementation reads | That harness only |

The split is the point. `kortix.yaml` decides what an agent may touch; the agent file decides how it works; `harnesses/` holds anything loop-specific, so replacing the loop does not touch the grants.

OpenAI publishes the Codex loop, so [the Codex harness](https://agentharnessguide.com/codex-harness) can be read before anyone swaps it in. Its approval rules come from configuration on whatever machine runs it, and its session record stays there too. Anthropic's Claude Code takes the configured route instead: the loop is Anthropic's, and what a team changes sits in settings files. [The Claude Code harness](https://agentharnessguide.com/claude-code-harness) explains the four places those settings come from, including the local file the tool writes itself after a single approval. In neither setup does a permission change reach a reviewer as a diff by default, which is why the grants live in `kortix.yaml` here.

## The two agents here

`release-notes` drafts a changelog from the commits between two tags, writes it to `notes/`, and opens a change request. It reads the repository and runs `git`, and it holds no connectors and no project secrets, because a changelog needs neither.

`repo-auditor` answers "what changed in this repository last week, and who reviewed it" from `git log` and merged change requests. It is read-only on purpose: its prompt tells it never to edit, and its grant means it cannot.

Neither agent merges. Session work reaches the trunk through a change request a person approves, and merge is default-deny for agents.

## Run it

```bash
git clone https://github.com/kortix-ai/suna    # the Kortix source, for reference
```

Then create a Kortix project from this directory (Kortix CLI, or the project importer in Kortix Cloud), set the two secrets the `release-notes` agent needs in Kortix Secrets, and start a session with `release-notes` as its agent. The manifest's `env.required` list is what the platform checks before a session boots.

Every session runs in its own isolated sandbox, as an ephemeral branch of this repo — install, experiment, recover; only what you commit and push survives.

## The parts of a harness this repo does not settle

The loop itself. A harness compensates for what a model cannot do yet, so the loop is the half that moves as models improve, and the manifest is the half you keep. If your grants are in a git repo, a loop upgrade is a code change and a grant change is a review.

A harness can be open at the loop and closed in the team layer above it, and which of the two you own decides whether a swap is a code change or a migration. [The open-source agent harness guide](https://agentharnessguide.com/open-source-agent-harness) starts from that split. Where the record lands once a session closes is the difference [the harness comparison](https://agentharnessguide.com/agent-harness-comparison) draws across four runtimes: a local store, a service database, or a commit. Here it lands in a commit.

More on the four shapes of harness and where each one keeps its boundaries: [what an agent harness owns](https://agentharnessguide.com/).

## Project files

- [Kortix on GitHub](https://github.com/kortix-ai/suna) — the source of the runtime these files boot into
- [kortix.com](https://kortix.com) — the product, documentation and self-host instructions
