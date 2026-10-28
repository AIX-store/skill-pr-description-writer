# PR Description Writer

An AI skill that writes pull request descriptions from the diff, commits and linked issue. Each description opens with a short business-language summary, then covers scope and out-of-scope, motivation, how it was tested (and what was not), risk and rollback, a screenshots checklist for UI work, reviewer guidance and deploy notes.

## Install

Claude Code:

```
git clone https://github.com/AIX-store/skill-pr-description-writer ~/.claude/skills/pr-description-writer
```

Other agents (Codex, Cursor, Claude Desktop projects): copy the contents of `SKILL.md` into the agent's instructions or rules file.

## Example prompts

- "Write the PR description for my current branch against main."
- "This PR touches billing. Add a proper risk and rollback section."
- "Rewrite this PR body so our product manager can approve it."

## Licence

MIT, © 2026 CodeMeld LLC.

Listed on AiX: https://aixstore.ai/store
