---
name: chat-routine
description: "Run this before your first reply in every Claude Code chat, and again when the work wraps up. It names the chat + · MM-DD-YY · Outcome and tracks its status (+ open, ✓ done, ✗ dropped). At the start it agrees on what done means, checks project rules, memory, git state, handoffs and other open chats, and gives a short opening note. At the end it checks the result, gives a closing note with recommended next steps (commit, record decisions, hand off, archive) and changes the status once the user agrees. Use it even when the first request looks small or the project has its own naming rules, when a chat's goal shifts, when the work looks finished, and whenever the user says things like close, wrap up, we're done, let's stop, drop this, park this or hand this off. Also use it when writing a prompt that starts another chat."
license: MIT
metadata:
  author: Nikhil Daftary
  version: "1.0"
---

# Chat routine

Created by Nikhil Daftary.

The user runs many chats at once, often across several projects. They scan chat titles to see what's open and what's finished, and they come back to chats weeks later. This routine keeps each title honest and makes each chat easy to pick up, hand off, or close. A ✓ only means something if you and the user agreed on "done" at the start and checked the result at the end, so both ends of the chat matter.

Skip this routine only when you're a helper inside another chat, started with the Agent tool. That chat owns the title, and you have none to set.

A chat that another chat started, such as a side session from `start_session`, is a chat of its own, with its own row in the sidebar. Run this routine first in that chat, even when its first message is a long task list from the other chat and not from the user. If the task list names a title, use that title.

## The title

`+ · MM-DD-YY · Outcome`

**Status** comes first:

- `+` means the chat is open: active, paused, blocked, or waiting on the user.
- `✓` means the outcome is done. Use it only after the user agrees and any project closeout checklist passes.
- `✗` means the user decided not to move forward. Use it only when the user makes that call.

Use exactly these characters: a plus sign, ✓ (U+2713), and ✗ (U+2717). Don't swap in ✅ or other marks, since a mix of marks makes the sidebar harder to scan.

**Date** is the local date the chat started, with dashes, like `09-24-26`. It stays the same when the status changes. For a new chat, run `date +%m-%d-%y` if you're unsure. If you're naming a chat that began on an earlier day, use its creation time from `get_session` (session ID `self`), converted to local time.

**Outcome** names the thing the chat produces, in a few plain words, like `Pricing page` or `Contact import plan`. Leave out finish words such as approved, closed, built, updated, done, fixed or confirmed. The status mark alone says whether the work is finished, and a finish word beside a `+` makes an open chat look done in the sidebar. Written this way, the title reads true with any status: `+` says the work is under way, ✓ says it happened, and ✗ says the user chose not to do it. Keep the whole title under about 50 characters, since sidebars cut long titles off. When the project tracks a release or area that matters, put it at the front of the outcome: `r1.4 Checkout flow`.

**Optional tag.** A project can ask for a short tag in its own slot between the date and the outcome: `+ · MM-DD-YY · Tag · Outcome`. Use a tag only when the project's CLAUDE.md or AGENTS.md defines one, such as a client's initials or a workstream code. Chats in other projects have no tag slot.

Separate the parts with a space, a middle dot, and a space (` · `).

```
+ · 09-24-26 · Chat routine skill
✓ · 09-22-26 · Pricing page
✗ · 09-20-26 · Podcast page
+ · 10-07-26 · ACME · Onboarding emails
```

Leave a title alone when the chat is closed or when the user or a project chose its format. Project records often refer to chats by their exact titles, and renaming them would break those references. An open chat that still has the app's automatic title can take a new-format title, dated with the day it started.

## Other assistants' formats stay as they are

Some projects also run another coding assistant, such as OpenAI Codex, and their AGENTS.md or closeout files may name a task title format for that assistant, such as `MM/DD/YY · Project area · Outcome`. Those formats belong to that assistant.

- In Claude chats, use this skill's title format even when project files name a different one.
- Don't edit the naming lines in those files, and don't rename the other assistant's tasks.
- Keep following everything else in those files: read-first lists, opening checks, completion checks, approval boundaries, and required closeout contents. Where a project sets a stricter bar than this skill, the stricter bar wins.
- When you write a prompt that starts a task in the other assistant, propose its title in the format the project's files give for it. When you write a prompt for a new Claude chat, use this skill's format.
- When you start a new Claude chat yourself with `start_session`, put the finished title in the tool's `title` field, like `+ · 11-20-26 · Signup copy`, and use the same title in the task list you send. The sidebar then shows the right title from the start, even if the new chat skips its opening routine.
- When the user pastes a handoff that says "Title this task: ...", keep its outcome words and convert the title to this skill's format.

## Opening routine

Match the routine to the chat. A quick question that won't change any files only needs a title. Run the full routine when the chat will change files, run long, or continue earlier work.

**1. Agree on "done."** Work out the outcome from the user's first message, write it as a title, and set it. If the goal is unclear, propose your best reading in one line and invite a correction instead of stalling.

**2. Get your bearings.** Do these checks quietly and report only what matters:

- **Project rules.** Read AGENTS.md, CLAUDE.md, and any files they say to read first, including a closeout checklist if the project has one.
- **Memory.** Your memory index loads on its own. Open the memory files that bear on this outcome.
- **Git.** Check the branch and `git status --short`. Uncommitted work you didn't make probably belongs to another chat, so keep your changes separate from it, and don't stash, reset, or clean it.
- **Handoffs.** Look for a handoff or next-task note the project keeps for this outcome, such as HANDOFF.md or NEXT_TASK.md.
- **Other open chats.** If you can list sessions, check the user's other `+` chats in the same folder and flag any that may edit the same files.

**3. Send a short opening note** of two to four lines: the title, anything surprising you found, and your plan. Then start the work in the same reply when you can. The note should orient the user, not hold them up.

```
Title: + · 09-25-26 · Onboarding guide
Heads-up: two folders have uncommitted work from another chat. I'll leave them alone.
Plan: read the current guide and last week's handoff, then show you the changes before I save them.
```

## While you work

Keep one outcome per chat. If the goal shifts, update the outcome and keep the date. If a separate goal comes up, suggest a new chat and offer a ready-to-paste starting prompt with a proposed title. Small side fixes can stay in the current chat.

## Closing routine

Start the closing routine yourself when the outcome looks done, rather than waiting for the user to say "close." The user can also start it at any time, including when they want to stop, pause, drop, or hand off the work.

**1. Check the result against the outcome you agreed on.** Verify in proportion to the risk: run the tests or the build, re-read the changed files, and look at the diff. If the project has its own closeout checklist, run it now.

**2. Send a closing note** in plain English, in this shape:

```
Title: + · 09-25-26 · Onboarding guide (✓ once you agree)
Status: Complete
Changed: docs/onboarding.md (setup section)
Checked: every setup step still matches the current install; links work
Memory: nothing new, since the change lives in the guide itself
Left: nothing
Your call on each:
1. Commit the change (pushing is a separate yes)
2. Archive this chat
Reply with the numbers you want, and I'll mark the chat ✓.
```

Pick one status: Complete, Complete with a follow-up, Waiting on you, Blocked, Partial, or Dropped. Recommend only the next steps that apply:

- Commit the changes. A push, merge, or deploy each needs its own yes.
- Record a decision in the project's decision log, if the project keeps one.
- Hand off with a ready-to-paste prompt and a proposed title for the next chat.
- Back up new files that live outside Git.
- Archive this chat.

If the project's closeout file lists required closeout contents, include those too.

**3. Change the status only when it's earned.** Switch `+` to `✓` once the user agrees the work is done and any project checklist has passed. Switch to `✗` only when the user decides to drop it. Otherwise the chat keeps its `+`. Treat a plain "yes" or "close" as agreement on the status, and ask before any numbered step the user didn't clearly pick.

**4. Do the steps the user picks, one yes at a time.** A yes to committing doesn't cover pushing. Closing a chat and archiving it are different things, so archive only when the user says so.

**If work remains,** keep the `+` and leave a restart point: what's done, what's left in order, the blocker or missing input, the next safe step, and a ready-to-paste prompt with a proposed title. After the handoff, suggest archiving this chat so the user's open list only shows live work.

**If the user drops the work,** write down why in a sentence or two. If the decision retires a real direction, offer a dated entry in the project's decision log so nobody repeats the same work later. Label leftover files as dropped or superseded instead of deleting them. Then offer to archive the chat.

## Memory versus project files

Claude keeps a separate memory for each folder, and other assistants keep their own, so none of them sees the others' notes. Put project facts, decisions, and handoffs in the project's files, which every assistant and teammate can read. Save to memory only how the user likes to work and facts that don't belong in any project file.

## Chat tools

In the Claude desktop app, the session tools can rename this chat (`set_session_title` with the session ID `self`), list the user's other chats (`list_sessions`), and archive a chat (`archive_session`). They may be deferred, so load them with ToolSearch before calling them. Replacing a title the app generated needs no approval, but the app may ask the user before replacing a title they set themselves.

If you can't rename the chat, as in a terminal session, put the exact title on its own line at the top of your first reply and again in the closing note so the user can paste it.

Write the opening and closing notes in plain English, and follow any writing rules the user keeps in their CLAUDE.md.
