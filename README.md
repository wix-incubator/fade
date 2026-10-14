# Fade skills

Agent skills for [Fade](https://www.tryfade.ai), installable with the
[`skills`](https://github.com/vercel-labs/skills) CLI into Claude Code, Codex, Cursor and
other agents.

The skills drive Fade through its MCP server (`https://mcp.tryfade.ai/mcp`, sign in with a Wix
account); [connect.md](skills/fade-onboarding/references/connect.md) has the setup for each app.

## The Fade plugin: the connection and its commands

`plugins/fade/` is one plugin with Fade's connection (`https://mcp.tryfade.ai/mcp`) and its
commands: `ad`, `remake`, `brand`, `projects`, `status` and `onboarding`. This repo is its
marketplace, so the apps follow it and update it themselves.

| App | Add it | Commands |
| --- | --- | --- |
| Claude, Cowork, Claude Desktop | Customize → Plugins → Add → Add marketplace → `wix-incubator/fade`, add **Fade**, then connect it and sign in with Wix | `/fade:ad`, `/fade:onboarding`, … |
| Claude Code | `claude plugin marketplace add wix-incubator/fade && claude plugin install fade@fade` | `/fade:ad`, `/fade:onboarding`, … |
| Codex | `codex plugin marketplace add wix-incubator/fade && codex plugin add fade@fade` | `$fade:ad`, … |

A plugin added to a Claude account is also in Cowork and Claude Code under that account.

The command files under `plugins/fade/skills/` are generated from imaxx's `@wix/fade-workflows`
(the same text Fade's server sends as MCP prompts); change them there and regenerate, not here.
`onboarding` calls the server's `fade_onboarding`, so it always plays Fade's current guide.

## Skills

| Skill | What it does |
| --- | --- |
| [`fade-onboarding`](skills/fade-onboarding/SKILL.md) | Sets a person or a new client up in Fade end to end: connect the MCP and sign in, pick the brand's workspace, read its website and add its files to the Brand folder, label them, build and approve the brand kit, then show how to start a first ad. Resumes from the first unfinished step. |

## Install

```bash
# list what's here
npx skills add wix-incubator/fade --list

# install all of them into the current project
npx skills add wix-incubator/fade

# or one, globally
npx skills add wix-incubator/fade --skill fade-onboarding -g
```

## Keep them up to date

Fade changes, and so do these skills. Update from time to time:

```bash
npx skills update -g
```

`fade-onboarding` also checks for you: when Fade has a newer version than the one you installed,
it tells you to run that command.

## Layout

One folder per skill under `skills/`, each with a `SKILL.md` (YAML frontmatter with `name`
and `description`, then the instructions):

```
skills/
  <skill-name>/
    SKILL.md
    references/   # optional: longer material SKILL.md links to
```

## Adding a skill

```bash
cd skills && npx skills init <skill-name>
```

Then fill in `skills/<skill-name>/SKILL.md`. `name` must match the folder name,
and add a row to the table above.

When you change a skill, raise `version` in its frontmatter, and in `fade-onboarding` the
version named in its body too (the update check reads that one).
Keep the body (everything after the frontmatter) under 12,000 characters: Fade refuses a
longer skill on upload.
