---
name: fade-onboarding
description: >-
  Set a person up in Fade (tryfade.ai, AI video ads) end to end: connect the Fade MCP and sign in,
  pick or make the workspace for their brand, fill its Brand folder (website read, logos, product
  photos, guidelines, old ads), label it, build and approve the brand kit, then show how to start
  a first ad. Use when someone says "set up Fade", "onboard me", "connect my brand to Fade", "add a
  new client to Fade", or when Fade's tools answer with an empty workspace or an empty Brand folder.
kind: craft
version: 9
---

# Fade onboarding

Walk the person through setting up Fade one step at a time, as a good agency onboards a client:
ask only what you cannot find out, show what Fade found, stop when they want to.
Every step is optional after step 2; a person can make an ad with an empty Brand folder, it is
just less on-brand.

If the person picks `instant-ad` (making an ad right now from their own media and prompt without
setting up a brand kit first), follow `start.options` (`fade_upload` + `fade_chat {withoutBrand: true}`)
immediately. When running the brand-kit flow below, end by showing step 8's starter prompts rather
than launching an unasked ad.

The flow runs through the Fade MCP (the `fade_*` tools). If you are Fade's own agent in the Fade
web app, read [Inside the Fade app](#inside-the-fade-app) first.

**House rules**

- Relay Fade's words; never invent what Fade found.
- A result with `accepted: false` is Fade declining for the reason given: tell the person, follow
  its suggestion, never retry unchanged.
- State where work lands before making anything (`workspace` on every answer).
- Free steps run without asking. Paid steps state their price first and run only on explicit yes.
- Never ask for, paste or pass an API key or password in chat.
- Keep each message short: one question, or one result and the next step.

## The flow

Track progress with this checklist and show it when asked:

```
- [ ] 1. Connected and signed in
- [ ] 2. Workspace for the brand
- [ ] 3. Website read into the Brand folder
- [ ] 4. Brand files added
- [ ] 5. Files labelled
- [ ] 6. Brand kit built
- [ ] 7. Kit approved
- [ ] 8. Ready for a first ad
```

Before starting, inspect `currentState` on `fade_onboarding`'s answer (if present), or call
`fade_whoami`, `fade_workspace` and `fade_brand` (all free). If the person already started a
website scan (`activeSiteRun`) or kit research (`webWizard.researchSessionId`) in the web Welcome
wizard, resume those IDs (`fade_brand {siteRun}` and `fade_status {sessionId}`) instead of asking
for their URL again or starting duplicates.

**This is version 9 of this skill.** Once per conversation (skip inside the Fade app or when
`fade_onboarding` served this text directly), call `fade_list {limit: 1}` (free): if
`fade-onboarding`'s `version` is higher than 9, mention once that they can run
`npx skills update -g`, then continue.

### 1. Connect and sign in

If no `fade_*` tools are available, give the setup for their app from
[references/connect.md](references/connect.md); in a terminal agent you may run the plugin install
yourself, then they sign in in the browser.

Call `fade_whoami` (or check `currentState.allowlisted`):

- `allowlisted: false` → Fade is in early access and this account is not on the invite list yet.
  Signing in at https://app.tryfade.ai notifies the Fade team in Slack automatically; tell them so
  and stop here.
- Otherwise say who they are signed in as and continue.

### 2. The workspace for the brand

**One brand = one workspace.** Every ad made in a workspace reads its Brand folder; agencies make
one workspace per client.

1. `fade_workspace` lists their workspaces (`recommendedWorkspaceId` marks a shared team workspace
   they joined while Personal has no work).
2. If they joined a shared team workspace that already has an approved Brand folder and no personal
   work, switch to it (`fade_workspace {workspaceId}`) and jump to step 8 unless they asked to set
   up another brand.
3. For a brand with no workspace yet, confirm the name and call
   `fade_workspace {newWorkspace: "<brand name>"}`. Note that new shared workspaces start at
   `$0.00` usage until an admin adds a plan or top-up (`Personal` includes the monthly Hobby
   allowance). (Web: account menu → workspace switcher → **Create a workspace**.)
4. Teammates are invited at `https://app.tryfade.ai/<workspaceId>/members`. Offer once; do not push.

Confirm in one line: "Setting up <brand> in <workspace>."

### 3. Read the brand's website (free, ~1 minute)

If `currentState.activeSiteRun` is running, poll `fade_brand {siteRun: "<runId>"}`. If
`currentState.brandFolder.siteHost` is already present, skip to step 4. Otherwise ask for the
website, call `fade_brand {fromSite: "acme.com"}`, and poll `fade_brand {siteRun}` until `done` or
`failed`.

Fade measures palette, typefaces, name and tagline (`look` in the answer) and files the logo,
screenshot, pictures and `site.md` under `site/<host>/`. Share what was measured and filed plus
`https://app.tryfade.ai/<workspaceId>/brand` to verify visually. If `blocked: "bot-check"`, do not
retry; offer step 6 instead. Start step 4 without waiting.

### 4. Add the brand's own files

Declared assets outrank site measurements. Ask in order of value:

1. Logos (SVG/PNG; plus an `on-dark` variant if available)
2. Product photos, by product
3. Brand guidelines, research, briefs (PDF, MD, TXT)
4. Best-performing old ads and their metrics
5. Licensed fonts, sounds or sonic logos

How to send:

- **Local folder with shell access** → `fade_import` (`onDone: "brand"`, curated `include` with
  `role`/`product`/`note`, `exclude` with reasons). Run `shell.plan`, confirm with the user, then
  run `shell.upload`. Never upload secrets, personal-data contracts or other brands' files.
- **No shell** → share `uploadPage` or `https://app.tryfade.ai/<workspaceId>/brand` for drag-and-drop.

Readings arrive asynchronously (`reading: "pending"`). Carry on without polling in a loop.

### 5. Label what matters

Call `fade_brand`, propose key labels from paths/readings, and apply on their OK via
`fade_brand {label: [...]}`:

- primary logo (`role: "logo", use: "primary"`) and dark-ground logo (`use: "on-dark"`); if they
  uploaded their own logo, clear `use: ""` on the site-scanned logo so only theirs stays `primary`;
- product photos (`role: "product-photo", product: "<name>"`);
- top old ad (`role: "old-ad", note: "best performer, CTR 2.1%"`).

### 6. Build the brand kit

Fade's `brand-research` session drafts canon (`BRAND.md`), look, voice, products, sourced claims,
compliance rules, audiences and market inside a capped `$0.50` research budget Fade covers (`$0.00`
from workspace usage).

If `currentState.webWizard.researchSessionId` exists, check `fade_status {sessionId}` instead of
starting another. Otherwise, on their yes:

```
fade_chat {text: "Build this workspace's brand kit from <site>", skill: "brand-research"}
```

Relay progress and questions (`fade_answer`), following with `fade_status {sessionId, after: lastSeq}`.

### 7. Approve the kit

Drafts carry `note: "draft (inferred)"` and are ignored by ads until approved. Ask them to review
`BRAND.md` and the drafts on **Brands** (where **Approve all** clears all draft notes) or with you.
On explicit approval, clear notes via `fade_brand {label: [{path, note: ""}]}`; send requested
edits back to `fade_chat {sessionId}`.

### 8. Ready for a first ad

Give three ready-to-send prompts filled with their brand and product:

- Remake a real ad: "Find <competitor or brand> ads on Meta and remake one for <product>".
- From their own video: "Remake this video for <product>", with the file or a link.
- From scratch: "Make a 15-second vertical ad for <product> that <goal>".

Fade asks on a `$0` plan first, draws preview stills under a small preview allowance, and quotes
the video render before any video, voice or music is spent.

### Optional: Google AI Studio key

If they ask about billing or limits, check `fade_settings` and share the settings link (never take
a key in chat).

## Inside the Fade app

In a chat on `app.tryfade.ai`, you have no `fade_*` tools and **never modify the Brand folder or
workspace yourself**:

- Call `brand_folder` (and `brand_folder {refresh: true}` after user edits) to check progress;
  steps 1–2 are already done.
- Guide in one or two lines per open step using exact UI names: **Brands** in the sidebar (page
  title **Brand kit**), **Brands → From website**, **Brands → Build the kit**, **Approve all** on
  the kit drafts banner, and **Account menu → Workspace settings** (for **Manage members** and
  Google key). Never write placeholder URLs like `<workspace id>`.
- End with step 8's prompts inside `finish`'s `summary` (or a plain reply with no tool call).

## Finishing

Close with the checklist, what is in the Brand folder, `https://app.tryfade.ai/<workspaceId>`, and
step 8's prompts (or the next open step if they paused early).
