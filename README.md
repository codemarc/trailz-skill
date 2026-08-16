# The Trailz Skill

An Agent Skill for Claude, Codex, Grok, and Muse that saves an insight,
answer, or cross-tool comparison to one running notebook, `~/.trailz`, no
matter which AI tool you were talking to when it came up.

A [Braintrailz](https://braintrailz.com) composition by [codemarc](https://github.com/codemarc).
Part of **Trailcraft**, an umbrella for capture tools built on the same
dotfile convention.

## The idea

Marc runs Claude (Code, Chat, Cowork, Desktop), ChatGPT (Ask, Codex, mobile,
browser), Cursor, Grok, Gemini, and Muse. Every one has its own memory
feature; none of them talk to each other. Trailz sidesteps that by not being
a product feature at all — it's a plain text dotfile any tool with a shell
can append to.

## What it does

- **Saves on trigger** — `/trailz`, `/cairn`, "save that for my braintrailz,"
  "save this to trailz," "drop a cairn," "cairn this," and a few more all do
  the same thing: append the answer under discussion to `~/.trailz`
- **Defaults smart** — saves the most recent substantive answer if you don't
  paste your own note
- **Formats consistently** — timestamped, titled, sourced entries so the log
  stays scannable months later
- **Handles sandboxed environments honestly** — when running somewhere
  without direct access to your real home directory (like a cloud sandbox),
  it says so and asks where to save instead of silently doing nothing, or
  silently doing the wrong thing

## Install

**Claude.ai (web/desktop/mobile):**

1. Download [`trailz.skill`](./trailz.skill) from this repo (or grab it from Releases)
2. Drop it into a Claude chat and click **Save skill** — or add it via Settings → Capabilities → Skills

**Claude Code:**

```bash
git clone https://github.com/codemarc/trailz-skill.git
mkdir -p ~/.claude/skills
cp -r trailz-skill/trailz ~/.claude/skills/trailz
```

**Codex:**

```bash
git clone https://github.com/codemarc/trailz-skill.git
mkdir -p ~/.codex/skills
cp -r trailz-skill/trailz ~/.codex/skills/trailz
```

**Grok Build:**

```bash
git clone https://github.com/codemarc/trailz-skill.git
mkdir -p ~/.grok/skills
cp -r trailz-skill/trailz ~/.grok/skills/trailz
```

Grok also reads Claude Code skills with no extra setup — if
`~/.claude/skills/trailz` is already installed, you can skip the copy.

**Muse Code:**

```bash
git clone https://github.com/codemarc/trailz-skill.git
mkdir -p ~/.config/muse/skills
cp -r trailz-skill/trailz ~/.config/muse/skills/trailz
# or: muse skills install ./trailz --scope user
```

## Update

```bash
cd /path/to/trailz-skill
git pull
rm -rf ~/.claude/skills/trailz
cp -r trailz ~/.claude/skills/trailz
```

Swap the target path for `~/.codex/skills`, `~/.grok/skills`, or
`~/.config/muse/skills` as needed.

## Usage

Say `/trailz`, `/cairn`, or any of the natural-language triggers ("save that
for my braintrailz," "drop a cairn," "add to trailz") at any point in a
conversation. The skill appends the current answer, or a note you supply, to
`~/.trailz` with a timestamp, short title, and source tool.

## How it's built

The reasoning behind the design is in
[`trailz/references/braintrailz.md`](./trailz/references/braintrailz.md) —
why it's a dotfile instead of a database, why trailz and cairn are one
trigger instead of two, and why the sandbox fallback never fails silently.

## License

MIT — see [LICENSE](./LICENSE).
