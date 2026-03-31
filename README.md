# add-skill-skill

A Claude Code skill for onboarding external skills into the [Higher-Ground-Institute/progressive-ai-skills](https://github.com/Higher-Ground-Institute/progressive-ai-skills/) collection.

## What it does

When you share a GitHub URL to a skill repo, this skill handles the full onboarding workflow: fetching the source, running a security audit, creating properly attributed files, updating the skills table, and opening a PR — all following the collection's conventions.

## Setup

To use this skill on any machine, symlink or copy it into your Claude Code skills directory:

```bash
# Clone this repo (if you haven't already)
git clone https://github.com/kategage/add-skill-skill.git

# Create the skills directory and symlink
mkdir -p ~/.claude/skills
ln -s "$(pwd)/add-skill-skill/.claude/skills/add-skill" ~/.claude/skills/add-skill
```

Or copy directly:

```bash
mkdir -p ~/.claude/skills/add-skill
cp add-skill-skill/.claude/skills/add-skill/SKILL.md ~/.claude/skills/add-skill/SKILL.md
```

## Trigger phrases

- Share a GitHub link to a repo containing a SKILL.md
- "Add this skill"
- "Import this skill"
- "Include this in our skills repo"

## What the skill does on activation

1. Fetches and reviews the source repo (SKILL.md, README, LICENSE)
2. Runs a security audit (checks for prompt injection, data exfiltration, destructive actions)
3. Creates `skills/{name}/SKILL.md` with attribution
4. Creates `skills/{name}/README.md` following the collection's format
5. Updates the main README skills table
6. Commits, pushes, and opens a PR with full context

## File structure

```
.claude/
  skills/
    add-skill/
      SKILL.md    # The skill definition
```
