# AI Claude Lib

Claude Code findings & tips collected from real-world plugin development.

---

## Plugin Skill Execution — Stable Auto-Invocation of Forked Skills

### The Problem

When a plugin skill uses `context: fork` with an `agent:` in its frontmatter, Claude Code sometimes loads the skill instructions inline in the main conversation instead of forking to the specified agent. When this happens, the main session tries to execute the skill instructions directly — which fails because the instructions are written for a subagent with different tools and an isolated context.

### The Fix

Add this instruction to your `~/.claude/CLAUDE.md` (or project-level `CLAUDE.md`):

```markdown
# Plugin skill execution

When a plugin skill with `context: fork` and `agent:` is loaded inline instead of being forked automatically, you MUST manually spawn the specified agent using the Task tool. Pass the skill content as the prompt and use the agent specified in the frontmatter. Never process forked skill instructions inline — always delegate to the correct subagent.
```

### Why It Works

Claude Code's `CLAUDE.md` instructions override default behavior and are always loaded into the system prompt. By explicitly telling the model to delegate forked skills to the Task tool, you guarantee the skill runs in the correct agent with the correct model and tools — even when the automatic fork mechanism doesn't trigger.

### When You Need This

- You're building plugins with skills that use `context: fork` + `agent:`
- Your skill's agent specifies a different `model:` (e.g. `model: opus` vs the default)
- The skill's instructions assume isolated context (file reads, writes, analysis) that shouldn't pollute the main session

---

*More findings coming as we dig deeper into Claude Code plugins, hooks, agents, and multi-model orchestration.*
