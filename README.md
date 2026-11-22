# Chat routine

A Claude Code skill by Nikhil Daftary.

If you run lots of Claude chats at once, the sidebar fills up with titles that tell you nothing. This skill gives every chat a title you can scan, like `+ · 11-21-26 · Pricing page`, and keeps that title honest:

- `+` means the chat is still open.
- `✓` means you and Claude agreed the work is done.
- `✗` means you decided to drop it.

Claude also gives you a short note at the start of each chat (what it found and what it plans to do) and a closing note at the end (what changed, what it checked, and the next steps for you to pick).

## Install

1. Copy the `chat-routine` folder into `~/.claude/skills/`.
2. Add this line to `~/.claude/CLAUDE.md` so Claude runs the skill in every chat:

```
Before your first reply in every Claude Code chat, run the chat-routine skill.
```

Claude only renames chats on its own in the Claude desktop app. In a terminal, Claude puts the title at the top of its first reply so you can copy it.

## Make it yours

- **Date order.** The title uses month-day-year. Change `MM-DD-YY` and `date +%m-%d-%y` in `SKILL.md` if you prefer another order.
- **Project tags.** To tag a project's chats, such as with a client's initials, describe the tag in that project's CLAUDE.md. The skill puts the tag between the date and the outcome.

## License

MIT. You can use, change and share this skill, as long as you keep the copyright notice in `LICENSE`.
