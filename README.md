# aiclaudelib

Заметки про Claude Code плагины. Грабли, фиксы, workaround-ы.

## `context: fork` + `agent:` — скилл не форкается

Скиллы с `context: fork` и `agent:` иногда загружаются inline в основную сессию вместо форка. Модель пытается выполнить инструкции агента прямо в чате — ломается всё.

**Фикс** — добавить в `CLAUDE.md`:

```markdown
# Plugin skill execution

When a plugin skill with `context: fork` and `agent:` is loaded inline instead of being forked automatically, you MUST manually spawn the specified agent using the Task tool. Pass the skill content as the prompt and use the agent specified in the frontmatter. Never process forked skill instructions inline — always delegate to the correct subagent.
```

Инструкции из `CLAUDE.md` грузятся в system prompt и переопределяют дефолтное поведение — модель будет делегировать через Task tool даже когда автофорк не сработал.
