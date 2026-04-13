# NOTES.md

Internal notes on how this skill is packaged, distributed, and where it should evolve. Not user-facing — see README.md for install instructions.

## Repo layout

- **`skills/mentormates/SKILL.md`** — canonical skill content (organizer + participant agent APIs). Source of truth. Edit here.
- **`.claude-plugin/plugin.json`** — makes this repo itself a valid Claude Code plugin install target, not just a marketplace source. Users can `/plugin install github.com/edumame/mentormates-skill` directly.
- **`README.md`** — user-facing install + setup.
- **`LICENSE`** — MIT.
- **`.github/workflows/sync-marketplace.yml`** — auto-mirrors `skills/mentormates/SKILL.md` to `edumame/mentormates-marketplace` on every push to main that touches the file. Requires `MARKETPLACE_PAT` repo secret (fine-grained PAT with `Contents: Read and write` on `edumame/mentormates-marketplace`).

## Sibling repo

[`edumame/mentormates-marketplace`](https://github.com/edumame/mentormates-marketplace) — Claude Code plugin marketplace. Registers two plugins:

- `mentormates` (this skill) → `plugins/mentormates/skills/mentormates/SKILL.md` (auto-synced from here)
- `hackeragent` → `plugins/hackeragent/skills/hackeragent/SKILL.md`

User install path:

```
/plugin marketplace add edumame/mentormates-marketplace
/plugin install mentormates@mentormates-marketplace
```

Then `/mentormates` in any Claude Code session.

## When to graduate beyond a single SKILL.md

The current setup is the right baseline for a v1 single-skill plugin. Borrow from gstack (github.com/garrytan/gstack) when any of these trigger:

| Trigger                                              | What to add from gstack                                                   |
|------------------------------------------------------|---------------------------------------------------------------------------|
| Adding a 2nd skill (e.g. organizer-only shortcut)    | `SKILL.md.tmpl` + `scripts/gen-skill-docs.ts` shared preamble template    |
| >10 active organizers using the skill                | `~/.mentormates/projects/<slug>/learnings.jsonl` per-workspace memory     |
| Users repeatedly hand-editing env vars               | `mentormates-config get/set` shell tool (~50 lines bash)                  |
| API evolves and old skill versions silently drift    | Preamble hits `/api/version`, prints `UPGRADE_AVAILABLE` if stale         |

## Patterns to explicitly skip (overkill for a domain API skill)

- Compiled browser daemon (gstack's `browse`) — we're API-only
- Multi-host abstraction (Codex/Cursor/Factory) — Claude Code only
- LLM-as-judge eval harness (~$4/run) — 1 skill doesn't justify it
- Team mode `.claude/` repo bootstrap
- SessionStart auto-inject meta-skill (superpowers pattern) — only needed for broadly-applicable skills; `/mentormates` is domain-specific and surfaces fine via SKILL.md description alone

## Reference research

Two reviews of neighboring skill ecosystems, both done 2026-04-13:

- **superpowers** (github.com/obra/superpowers): the SessionStart hook + `using-superpowers` meta-skill pattern auto-activates every session. We deliberately don't replicate this — noise for users not working on hackathons.
- **gstack** (github.com/garrytan/gstack): 23-skill suite with shared template preamble, compiled binary, telemetry, per-project learnings. We borrow the template preamble idea (deferred) and learnings JSONL (deferred).

## History

- 2026-04-13: stale `mentormates-organizer.md` deleted (was organizer-only doc superseded by unified SKILL.md).
- 2026-04-13: marketplace entry added + sync workflow created.
