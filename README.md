# add-skill-skill

A Claude Code skill for onboarding external skills into the [Higher-Ground-Institute/progressive-ai-skills](https://github.com/Higher-Ground-Institute/progressive-ai-skills/) collection.

## What it does

When you share a GitHub URL, website link, blog post, or paste raw content describing a skill or workflow, this skill handles the full onboarding workflow: fetching the source, running a security audit (PASS/FLAG/REJECT), checking license compatibility, creating properly attributed files, updating the skills table and site index, and opening a PR — all following the collection's conventions.

## Source types

The skill handles four types of input:

1. **GitHub URL** to a SKILL.md or skill repo
2. **Website or blog post** describing a skill or workflow
3. **Raw pasted content** (asks for attribution details)
4. **Skill collection or repo** with multiple skills (asks which to catalog)

## Default behavior: link with attribution

By default, the skill creates a **linked stub** — a lightweight SKILL.md that works on its own but points to the canonical source. This keeps the collection lean and respects original authorship.

**Full copy mode** is available on request ("copy this skill into the repo", "import the full content"). In full copy mode, the original SKILL.md is preserved exactly as authored — no rewrites, no restructuring. The only addition is an HTML comment attribution header:

```
<!-- Original skill by {author} — URL -->
```

## Security audit

Every external skill is audited before integration. The skill checks for:

- **Prompt injection** — hidden instructions, override attempts, conditional/sleeper behavior
- **Data exfiltration** — instructions to read or transmit sensitive files, credentials, or tokens
- **Destructive actions** — file deletion, arbitrary shell commands, unauthorized system changes
- **Deceptive scope** — the description claims one function but instructions do something broader

Each audit produces a verdict:

| Verdict | Meaning |
|---------|---------|
| **PASS** | No concerns found. Safe to proceed. |
| **FLAG** | Potential concerns identified but not necessarily malicious. User decides. |
| **REJECT** | Clear safety issues. Do not proceed. |

## License compatibility

The progressive-ai-skills repo is MIT-licensed. The skill checks source licenses and flags:

- **Permissive (MIT, Apache 2.0)** — proceed with attribution
- **Copyleft (GPL, AGPL)** — flagged for potential incompatibility
- **No license found** — flagged; default is "all rights reserved"
- **Unclear or custom** — flagged with details for user review

## Artifacts produced

For each skill imported, the skill creates four artifacts:

1. **`skills/<name>/README.md`** — description, attribution, category, usage info, and credit section
2. **`skills/<name>/SKILL.md`** — linked stub (default) or full copy with attribution comment
3. **`site/index.jsx` entry** — JavaScript object added to the SKILLS array for the website listing
4. **Root `README.md` table row** — skill entry in the main skills table

Then it commits, pushes to a feature branch, and opens a PR with:
- Security audit verdict and any flagged items
- Link to the original source
- Credit to the original author
- Testing checklist

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
- Share a website or blog post URL describing a workflow
- Paste raw skill content directly
- "Add this skill"
- "Import this skill"
- "Include this in our skills repo"
- "Turn this into a skill"
- "Convert this to our format"
- "Add this to the skills catalog"

## File structure

```
.claude/
  skills/
    add-skill/
      SKILL.md    # The skill definition
```
