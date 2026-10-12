# Fade skills

Agent skills for [Fade](https://www.tryfade.ai), installable with the
[`skills`](https://github.com/vercel-labs/skills) CLI into Claude Code, Codex, Cursor and
other agents.

## Install

```bash
# list what's here
npx skills add wix-incubator/fade --list

# install all of them into the current project
npx skills add wix-incubator/fade

# or one, globally
npx skills add wix-incubator/fade --skill <name> -g
```

## Layout

One folder per skill under `skills/`, each with a `SKILL.md` (YAML frontmatter with `name`
and `description`, then the instructions):

```
skills/
  <skill-name>/
    SKILL.md
```

## Adding a skill

```bash
cd skills && npx skills init <skill-name>
```

Then fill in `skills/<skill-name>/SKILL.md`. `name` must match the folder name.
