# Braintrailz Composition Notes: The Trailz Skill

Trail markers for why this skill is built the way it is. Not a changelog.

## The origin: too many tools, one memory

Marc runs Claude (Code, Chat, Cowork, Desktop), ChatGPT (Ask, Codex, mobile,
browser), Cursor, Grok, Gemini, and Muse, often in the same afternoon. Every
one of them has its own memory feature, and none of them talk to each other.
An insight surfaced in a Cowork conversation stays trapped in Cowork.

The fix is not a better memory feature in any one tool. It's a memory that
doesn't belong to a tool at all.

## Why a dotfile, not a database

`~/.trailz` is a plain text file in the home directory, not a database, not a
vector store, not a product feature. That's deliberate: any tool with a shell
and a filesystem can append to it, regardless of vendor, regardless of
whether it has a "memory" feature at all. The format is the interoperability
layer. A dotfile also travels the way the rest of Marc's dotfiles do — it's
already the right shape for a `~/dotfiles` repo or a sync tool, without any
special-casing.

## Trailz and cairn are one trigger, not two

The natural instinct when adding a second word ("cairn") is to give it a
second behavior. Resisted here on purpose. A cairn is trail slang for a
marker you stack to flag a spot worth remembering — that's exactly what
`/trailz` already does. Splitting it into a "heavier" and "lighter" save
would mean deciding, every time, which one applies, and that friction defeats
the point of a fast capture command. One action, many names for it.

## Cloud sandbox vs local shell is a real fork, not an edge case

Claude sometimes runs with a real shell against the user's actual home
directory (Claude Code), and sometimes runs in a cloud container that only
reaches the user's machine through a device bridge that can't mount the home
directory root (Cowork). Early drafts of this skill assumed direct access
and would have silently failed, or silently written somewhere unexpected, in
the sandboxed case. The skill now names both cases explicitly and never lets
a save happen quietly outside `~/.trailz` — it says so, every time, if the
entry landed somewhere other than the real file.

## Ask once, not every time

The first save in a constrained environment asks the user where to put
things. Every save after that reuses the answer. A notebook meant for fast
capture cannot start every entry with a setup question.

## Why this file exists

Trailz is a personal notebook skill; `braintrailz.md` is the same idea
applied to the skill's own construction. Every skill in this ecosystem
(solid, trailz, and whatever comes next) carries one of these — decisions and
their reasoning, not a session transcript. The trail is marked; hike it.
