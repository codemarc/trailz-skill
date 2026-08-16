---
name: trailz
description: Save an insight, answer, or cross-tool comparison to a running personal log at ~/.trailz, so notes captured while working in Claude, ChatGPT, Cursor, Grok, Gemini, or Muse all land in one plain-text file for Braintrailz. Part of Trailcraft. Trigger on "/trailz", "save that for my braintrailz", "save this to trailz", "add to trailz", "/cairn", "save cairn", "drop a cairn", "cairn this", or similar phrasing.
---

# Trailz (part of Trailcraft)

Marc (codeMarc) runs multiple AI tools side by side: Claude (Code, Chat, Cowork,
Desktop), ChatGPT (Ask, Codex, mobile, browser), Cursor, Grok, Gemini, and
Muse. He wants one durable, tool-agnostic notebook that any of them can write
to: a plain text file at `~/.trailz` in his home directory. This skill is
Claude's half of that contract — it does not require any particular AI
product, just a shell and a filesystem, which is why the target is a dotfile
rather than anything product-specific. "Trailz" is the log itself; "Trailcraft"
is the umbrella name for this and any related capture tools Marc builds later
(e.g. a lighter-weight marker skill).

## When this triggers

All of the following are equivalent launch words for the same save action:

- `/trailz`
- "save that for my braintrailz"
- "save this to trailz"
- "add that to trailz"
- "trailz it"
- `/cairn`
- "save cairn"
- "drop a cairn"
- "cairn this"

A cairn is trail slang for a marker stacked up to flag a spot worth
remembering, so it's used here as a synonym for "save this" rather than a
distinct, lighter-weight action. Cairn and trailz triggers behave identically
and write to the same `~/.trailz` file.

The user may optionally give a short label or note along with the trigger
(e.g. "/trailz — memory implementations across the stack"). If no label is
given, derive a short one from the content being saved.

## What to save

Default to the most recent substantive answer or comparison Claude gave in
the conversation (e.g. a cross-tool breakdown of how a feature works). If the
user's trigger message includes its own text (a note, a snippet, a
conclusion), save that instead of re-deriving something from earlier context.
Don't pad the entry with commentary — save the actual content, trimmed to
what's worth keeping, not a re-summary of the whole conversation.

## Entry format

Each entry is appended to `~/.trailz` as:

```
## YYYY-MM-DD HH:MM TZ — <short title>
Source: <tool/context, e.g. "Claude (Cowork)", "Claude Code">

<the saved content, in plain prose/markdown, no em dashes>

---
```

Use the user's local timezone if known (America/New_York unless told
otherwise). Keep the title short (a few words) — it's a scan-friendly label,
not a headline.

If `~/.trailz` doesn't exist yet, create it and prepend a one-line header
before the first entry:

```
# Trailz — codeMarc's running cross-tool notebook
```

## How to write it

**If you have direct local shell/filesystem access** (e.g. running as Claude
Code on the user's machine, or any agent with a real shell against the user's
actual home directory): write the entry to a temp file first, then append
that temp file's contents to `~/.trailz` (avoids quoting/escaping problems
with heredocs full of markdown or code). Confirm back to the user with the
file path and the new total entry count — keep it to one line, no need to
print the whole file.

**If you are running in a cloud sandbox without direct access to the real
`~/` on the user's machine** (e.g. Cowork's cloud container, reaching the
user's computer only through a device bridge that cannot mount the home
directory root itself): you cannot write straight to `~/.trailz`. Handle it
like this:

1. Check whether a device bridge to the user's computer is available and
   whether a folder is already connected.
2. If nothing is connected, ask the user once (not every time) how they'd
   like this handled going forward — offer to connect a specific folder (a
   dotfiles folder, Documents, or similar — home directory roots typically
   can't be granted directly) that either contains `.trailz` or that you'll
   write a `trailz.md` into, and remember their choice for future entries
   this session.
3. If a suitable connected location exists, append the formatted entry there
   the same way as the local case (temp file, then append).
4. If no device connection is available at all, produce the formatted entry
   as a small file and deliver it to the user directly so they can drop it
   into `~/.trailz` themselves, and say plainly that the automatic append
   didn't happen because there's no path to their local home directory from
   this session.

Never silently skip saving — always tell the user in one short line whether
the entry landed in `~/.trailz`, landed somewhere else, or needs to be
placed manually, so the notebook doesn't quietly drift out of sync.

## Notes

- This is plain text/markdown by design, not a Claude-specific format —
  other tools (ChatGPT, Cursor, Grok, Gemini, Muse) can append to the same
  file if the user sets them up to, which is the whole point of putting it
  in a dotfile instead of a product's own memory feature.
- Don't ask for confirmation before saving unless the content to save is
  genuinely ambiguous — `/trailz` is meant to be a fast, low-friction capture
  command.
