# Chat routine

A Claude Code skill by Nikhil Daftary.

If you run many Claude chats at once, your sidebar soon fills with titles that don't tell you which chats are finished. This skill gives every chat a title you can scan at a glance, like `+ · 11-21-26 · Pricing page`, and keeps that title up to date:

- `+` means the chat is still open.
- `✓` means you and Claude agreed the work in the chat is done.
- `✗` means you decided to drop the work.

The date in the title is the day the chat started. The last part of the title names what the chat will produce.

Claude also sends you a short note when a chat starts and another when the work ends. The opening note says what Claude found in your project and what Claude plans to do. The closing note says what Claude changed, what Claude checked, and which next steps you can pick.

## Install the skill

1. Download this repository.
2. Copy the `chat-routine` folder into the `skills` folder inside your Claude settings folder. On a Mac or Linux computer, the full path is `~/.claude/skills/chat-routine/SKILL.md`. Create the `skills` folder if you don't have one yet.
3. Start a new Claude Code chat so Claude loads the new skill.

To confirm the install, type `/chat-routine` in a new chat. If Claude runs the routine, the skill is installed.

## Run the skill in every chat

Claude picks which skills to run based on what you ask, so Claude may skip this skill in some chats. To make Claude run this skill at the start of every chat, add this line to `~/.claude/CLAUDE.md` (create the file if you don't have one):

```
Before your first reply in every Claude Code chat, run the chat-routine skill.
```

If you'd rather choose which chats get the routine, skip that line and type `/chat-routine` at the start of a chat.

## Use the skill

**Start a chat as you normally would.** Ask Claude for what you need. Before Claude starts on your request, Claude names the chat and sends a two-to-four-line opening note. If the title misses the goal of the chat, tell Claude, and Claude will rename the chat.

**Close the chat when the work is done.** Claude often offers to close the chat once the work looks finished. You can also say "wrap up" or "close" at any time. Claude then checks the work and sends a closing note with numbered next steps, such as committing your changes or archiving the chat. Reply with the numbers you want. Claude changes `+` to `✓` only after you agree the work is done.

**Drop or pause the work.** Say "drop this" to stop for good, and Claude marks the chat `✗` and writes down why you stopped. Say "park this" or "hand this off" to stop for now, and Claude leaves the chat open with `+` and writes a note that lets you or a new chat pick up the work later.

**Keep one goal per chat.** If you raise a new, unrelated goal partway through a chat, Claude suggests a new chat and writes a starting prompt for that chat.

## Where the skill works

In the Claude desktop app, Claude renames each chat for you and can archive finished chats. In the terminal, Claude can't rename a chat, so Claude puts the title at the top of the first reply for you to copy.

## Change the skill to fit you

- **Date order.** The title puts the month first. To put the day first, edit `SKILL.md` and change `MM-DD-YY` to `DD-MM-YY` and `date +%m-%d-%y` to `date +%d-%m-%y`.
- **Project tags.** To add a tag to one project's chat titles, such as a client's initials, describe the tag in that project's `CLAUDE.md`. Claude then puts the tag between the date and the outcome, like `+ · 11-21-26 · ACME · Onboarding emails`.

## License

This skill uses the MIT license. You can use, change and share the skill, as long as you keep the copyright notice in `LICENSE`.
