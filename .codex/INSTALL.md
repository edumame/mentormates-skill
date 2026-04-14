# MentorMates skill — Codex install

Codex CLI scans `~/.agents/skills/` for user-level skills (NOT `~/.codex/skills/` — that path is ignored).

## Install

```bash
mkdir -p ~/.agents/skills/mentormates
curl -fsSL https://raw.githubusercontent.com/edumame/mentormates-skill/main/skills/mentormates/SKILL.md \
  -o ~/.agents/skills/mentormates/SKILL.md
```

## Set your API key

Generate a key at https://www.mentormates.ai/keys, then add to your shell's rc file (`~/.zshrc`, `~/.bashrc`, etc.):

```bash
# Participant
export MENTORMATES_PARTICIPANT_API_KEY="mm_sk_..."

# Or organizer
export MENTORMATES_API_KEY="mm_sk_..."
```

Then `source` the rc file or open a new shell.

## Restart Codex once

Codex hot-reloads existing skill content, but first-time discovery of a new skill folder requires restarting the CLI. After restart, the skill is loaded for all future sessions.

## Invoke

In Codex you do **not** type `/mentormates` — Codex doesn't auto-map skill names to top-level slash commands the way Claude Code does. Use either:

- `/skill mentormates` (the `/skill` selector)
- `$mentormates` mention syntax in your prompt

When invoked, the agent will greet you with the three-question intake (event, project, in-repo).

## Update later

Re-run the curl command above to fetch the latest SKILL.md. Codex picks up content edits without a restart.

## Uninstall

```bash
rm -rf ~/.agents/skills/mentormates
```
