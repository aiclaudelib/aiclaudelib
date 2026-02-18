# aiclaudelib

Community plugins for Claude Code.

<pre>
aiclaudelib marketplace
│
├─ analysis
│  ├─ <a href="https://github.com/aiclaudelib/reflect-plugin">reflect</a> ················· autonomous self-review: question → investigate → fix
│  └─ <a href="https://github.com/aiclaudelib/code-review-plugin">code-review</a> ············· multi-agent PR review with confidence scoring
│
├─ utility
│  ├─ <a href="https://github.com/aiclaudelib/ccode-plugin">ccode</a> ··················· plugin scaffolding, skills, agents, hooks, MCP
│  ├─ <a href="https://github.com/aiclaudelib/git-plugin">git</a> ····················· workflow commands, checkpoints, expert agent
│  └─ <a href="https://github.com/aiclaudelib/marketplace">aiclaudelib</a> ············· interactive marketplace browser & installer
│
├─ testing
│  └─ <a href="https://github.com/aiclaudelib/playwright-cli-plugin">playwright-cli</a> ·········· browser automation, screenshots, tracing, network mocking
│
└─ development
   └─ <a href="https://github.com/aiclaudelib/nodejs-plugin">nodejs-development</a> ······ architecture, testing, security, API, TypeScript
</pre>

> **Install:** `/plugin marketplace add aiclaudelib/marketplace` then `/aiclaudelib:install`
>
> **Direct:** `/plugin install <name>@aiclaudelib` — where name is any of the above

---

## Notes

Notes on Claude Code plugins. Gotchas, fixes, workarounds.

## `context: fork` + `agent:` — skill doesn't fork

Skills with `context: fork` and `agent:` sometimes load inline into the main session instead of forking. The model tries to execute agent instructions directly in the chat — everything breaks.

**Fix** — add to `CLAUDE.md`:

```markdown
# Plugin skill execution

When a plugin skill with `context: fork` and `agent:` is loaded inline instead of being forked automatically, you MUST manually spawn the specified agent using the Task tool. Pass the skill content as the prompt and use the agent specified in the frontmatter. Never process forked skill instructions inline — always delegate to the correct subagent.
```

`CLAUDE.md` instructions are loaded into the system prompt and override default behavior — the model will delegate via Task tool even when auto-fork doesn't trigger.
