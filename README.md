# MentorMates Skill

Agent skill for managing MentorMates hackathon events and participant submissions via the MentorMates API.

## Install

```bash
npx skills add edumame/mentormates-skill
```

## Setup

Generate an API key at [mentormates.ai](https://www.mentormates.ai) → API Keys, then:

```bash
# Organizer key
export MENTORMATES_API_KEY="mm_sk_..."
export MENTORMATES_EVENT_ID="<event_uuid>"  # for reusable keys

# Or participant key
export MENTORMATES_PARTICIPANT_API_KEY="mm_sk_..."
```

## What it does

- **Organizer**: manage events, participants, approvals, projects, judges; send judge emails
- **Participant**: discover + join events, submit and edit projects

See [SKILL.md](./SKILL.md) for the full API reference.
