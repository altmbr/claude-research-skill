# /research — Multi-Agent Research Orchestrator for Claude Code

A Claude Code skill that decomposes any research question into parallel agent workstreams, launches them simultaneously, monitors progress, and synthesizes results into a single coherent document.

**One command. Multiple agents. Deeply sourced research in minutes.**

## What It Does

Type `/research [any question]` and the skill will:

1. **Decompose** your question into 2-4 non-overlapping research workstreams
2. **Present a plan** for your approval before doing anything
3. **Launch parallel agents** — each writes to its own markdown file in real-time
4. **Monitor progress** — checks in at escalating intervals, kills stuck agents, relaunches automatically
5. **Synthesize** — a final agent reads all research outputs and produces a unified document with cross-cutting insights, contradictions, and confidence assessments

## Example

```
> /research Help me understand the psychology of dating to find a life partner in your mid-30s

## Research Plan: Dating Psychology (Mid-30s)

### Agent 1: Decision Science & Optimal Stopping
Sections: Secretary problem, satisficing vs maximizing, paradox of choice...

### Agent 2: Relationship Science & Compatibility
Sections: Gottman's research, attachment theory, compatibility factors...

### Agent 3: Dating Channels & Strategy
Sections: Apps vs community, meeting method vs quality, cognitive biases...

### Synthesis Agent
Runs after all agents complete → unified playbook with actionable framework
```

**Output:** 4 markdown files, ~1,700+ lines of deeply sourced research with inline citations — produced in ~10 minutes.

## Install

Copy the skill file to your Claude Code commands directory:

```bash
# Clone the repo
git clone https://github.com/altmbr/claude-research-skill.git

# Copy to Claude Code
cp claude-research-skill/SKILL.md ~/.claude/commands/research.md
```

Or manually: download `SKILL.md` and save it as `~/.claude/commands/research.md`.

That's it. Type `/research` in Claude Code and it works.

## How It Works

The skill uses Claude Code's built-in Task tool to launch multiple `general-purpose` subagents in parallel. Each agent:

- Gets a **strict write protocol** — must write findings to its file after every single search (no two searches in a row without writing)
- Works through sections **in order**, citing every claim with inline source URLs
- Marks its file `Status: COMPLETE` when done

The orchestrator monitors all agents at escalating intervals (30s, 2min, 5min, then every 5min). If an agent gets stuck (line count unchanged between check-ins), it's automatically killed and relaunched with any data it already found pre-loaded.

After all research agents complete, a synthesis agent reads every output file and produces:
- **Executive summary** (3-5 bullets)
- **Key findings by theme** (cross-cutting, not per-agent)
- **Contradictions & tensions**
- **Confidence assessment** (well-supported vs. speculative)
- **Recommended next steps**

## Key Design Decisions

- **Write-after-every-search protocol** — Agents that research without writing get stuck in loops. The strict alternating pattern (search → write → search → write) prevents this entirely.
- **Stuck agent recovery** — Rather than waiting for agents to self-correct (they won't), the orchestrator kills and relaunches with pre-loaded data. No wasted time.
- **Skeleton files first** — Creating output files with section headers before launching agents gives them structure and makes progress monitoring trivial (just check line counts).
- **User approval before launch** — The plan is always shown first. No wasted compute on the wrong decomposition.

## Output Structure

```
research/
└── [topic]/
    ├── agent-1.md          # First research workstream
    ├── agent-2.md          # Second research workstream
    ├── agent-3.md          # Third research workstream (if needed)
    └── synthesis.md        # Cross-cutting synthesis
```

Each file is a standalone, deeply sourced markdown document with inline citations.

## Requirements

- [Claude Code](https://claude.com/claude-code) (CLI)
- No API keys or external dependencies — uses Claude Code's built-in tools

## License

MIT — see [LICENSE](LICENSE).

## Author

**Bryan Altman** — [GitHub](https://github.com/altmbr)
