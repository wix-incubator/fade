---
name: fade-onboarding
description: >-
  Set a person up in Fade (tryfade.ai, AI video ads) end to end: connect the Fade MCP and sign in,
  pick or make the workspace for their brand, fill its Brand folder (website read, logos, product
  photos, guidelines, old ads), label it, build and approve the brand kit, then show how to start
  a first ad. Use when someone says "set up Fade", "onboard me", "connect my brand to Fade", "add a
  new client to Fade", or when Fade's tools answer with an empty workspace or an empty Brand folder.
kind: craft
version: 4
---

# Fade onboarding

Walk the person through setting up Fade one step at a time, the way a good agency onboards a new
client: ask only what you cannot find out, show what Fade found, and stop when they want to stop.
Every step is optional after step 2; a person can make an ad with an empty Brand folder, it is
just less on-brand.

Onboarding ends with the brand set up. It never starts an ad: it ends by showing the person how to
start one themselves (step 8).

The flow runs through the Fade MCP (the `fade_*` tools). If you are Fade's own agent, in a chat in
the Fade app, read [Inside the Fade app](#inside-the-fade-app) first.

**House rules for the whole flow**

- Relay Fade's words; do not invent what Fade found. Fade's answers are the source of truth.
- A result with `accepted: false` is Fade declining for the reason it gives. Tell the person, do
  what it suggests, never retry it unchanged.
- Say where work lands before making anything: every answer carries `workspace`.
- Free steps run without asking. Anything that spends (research, a render) is stated with its
  price first and runs only on the person's explicit yes.
- Never ask for, paste or pass an API key or password in chat. Fade sends links for that.
- Keep each message short: one question, or one result and the next step.

## The flow

Track progress with this checklist and show it when the person asks where they are:

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

Before starting, find out where they are: if the Fade tools are connected, call `fade_whoami`,
`fade_workspace` and `fade_brand` (all free) and tick what is already done. Resume from the first
open step; never redo a finished one.

### 1. Connect and sign in

If no `fade_*` tools are available, the Fade MCP is not connected. Its address is
`https://mcp.tryfade.ai/mcp` (remote, OAuth sign-in with a Wix account). Give the person the one
command or setting for their app from [references/connect.md](references/connect.md); in a
terminal agent you may run the command yourself, then they sign in in the browser.

Then call `fade_whoami`:

- `allowlisted: false` → Fade is in early access and this account is not invited yet. Tell them to
  ask for access at https://www.tryfade.ai and stop here.
- Otherwise say who they are signed in as and continue.

### 2. The workspace for the brand

**One brand = one workspace.** The Brand folder belongs to the workspace, and every ad made there
reads it. An agency makes one workspace per client.

1. `fade_workspace` (no arguments) lists their workspaces with the current one marked.
2. Ask which brand this is for. If a workspace for it exists, switch:
   `fade_workspace {workspaceId}`.
3. A brand with no workspace yet (a client, a company) gets a new one:
   `fade_workspace {newWorkspace: "<brand name>"}` makes it, owned by the person, and switches to
   it. Say the name you will use before you make it. Their personal workspace is fine for one
   brand of their own. (On the web: workspace switcher → new workspace.)
4. Teammates are invited on the workspace's Members page:
   `https://app.tryfade.ai/<workspaceId>/members`. Offer it once; do not push.

Confirm in one line: "Setting up <brand> in <workspace>."

### 3. Read the brand's website (free, about a minute)

Ask for the brand's website if you do not know it. Then `fade_brand {fromSite: "acme.com"}` and
poll `fade_brand {siteRun}` until it is `done` or `failed`.

Fade opens the site in a browser, measures its palette by painted area, its typefaces, name and
tagline, and files the logo, a screenshot, a few pictures and a `site.md` under `site/<host>/`.
Tell the person which files were filed. The measured colours and typefaces are drawn on the Brand
page (`page` in the answer); send them there to check it looks like them. Over MCP you only see
each file's one-line reading, so never state a colour or a font you have not seen in an answer.
A site behind a login or a cookie wall reads only what it drew; say so.

Start step 4 while the site is read; you do not have to wait for it.

### 4. Add the brand's own files

What the brand declares outranks what Fade measured from the site, so ask for what they have,
in this order of value:

1. Logos (SVG or PNG; a version for dark backgrounds if they have one)
2. Product photos, by product
3. Brand guidelines / brand book, research, briefs (PDF, MD, TXT; Word/PowerPoint: ask for a PDF
   export, those are kept but not read yet)
4. Old ads, especially the ones that performed best, and their numbers
5. Fonts, sounds or a sonic logo, with their licences

How to send them:

- **They point you at a folder on this machine** → `fade_import` with `onDone: "brand"`, an
  `include` list you curate (each with `role`, `product`, `note`) and an `exclude` for what you
  left out and why. Run its `shell.plan`, show the output, and on their OK run `shell.upload`.
  Send only this brand's files; never secrets, contracts with personal data, or other brands'
  assets.
- **No shell** (desktop or phone chat) → give them the import's `uploadPage`, or the Brand page
  `https://app.tryfade.ai/<workspaceId>/brand`, where they drag and drop.

Files are read on arrival, which takes a few minutes for a batch. `fade_brand` shows each one's
one-line reading (`reading: "pending"` while it reads). Do not sleep or poll in a loop for it:
carry on, and look again when you need a reading (step 5).

### 5. Label what matters

Call `fade_brand` and look at roles. Labels are optional, but Fade trusts them over its own guess,
so set the few that change an ad, in one `fade_brand {label: [...]}` call:

- the primary logo (`role: "logo", use: "primary"`) and the one for dark backgrounds
  (`use: "on-dark"`);
- each product's photos (`role: "product-photo", product: "<name>"`);
- the best old ad (`role: "old-ad", note: "best performer, CTR 2.1%"`).

Keep one primary logo. The site read labels its logo `primary`, so when the person also uploads
one, theirs wins (the brand's own file outranks a reading of its site): clear `use` on the other
with `use: ""`. A placeholder or test file never keeps `primary`.

Propose the labels from the paths and readings, and set them once the person agrees. A picture
whose reading is still pending is labelled later: say which ones are left and pick them up at the
end or next time. Rename and delete are on the Brand page.

### 6. Build the brand kit (may spend; ask first)

The kit is the onboarding binder an agency team would get on day one: canon (name, one-liner),
look, voice, products, claims with sources, rules and compliance, audiences, past ads, market.
Fade's kit session builds it from the Brand folder and public sources, writes each layer into the
folder as a draft, and asks only what only the brand can answer.

Tell the person it may buy public research (page text, ad libraries, reviews, search volume)
inside a small capped budget per kit, and that it says what it spent. On their yes:

```
fade_chat {text: "Build this workspace's brand kit from <site>", skill: "brand-research"}
```

Relay its progress and questions; answer questions with `fade_answer` using the person's own
words (rights to faces, fonts and music, which claims are cleared, real ad results). If research
is unavailable, the session still drafts the kit from the folder and the site read. Follow up
with `fade_status {sessionId, after: lastSeq}` while it says it is still working.

### 7. Approve the kit

Nothing a kit session drafted reaches an ad until the person approves it. Drafts carry the note
`draft (inferred)`. Ask them to read `BRAND.md` (the kit's one-page canon, which names the file
behind every line) and the drafts, on the Brand page or with you.

When they approve a file, clear its draft note: `fade_brand {label: [{path, note: ""}]}`. Only on
their explicit say-so per file or for the whole kit ("approve the kit"); "looks fine" about one
line is not approval of the kit. Edits they want go back to the kit session in `fade_chat` with
its `sessionId`.

The kit is **ready** when it has canon, look, voice, one product with its claims, and, for a
regulated category (finance, health, people search, alcohol, gambling), its compliance rules.

### 8. Ready for a first ad

Onboarding never starts an ad, even when the person asks for one in the same message: do not call
`fade_find_ads`, `fade_use_ad`, `fade_upload` or `fade_chat` to make one. Show them how to start
it instead, with prompts they can send as they are, filled in with their brand and product, and
say they can send one now:

- Remake a real ad: "Find <competitor or brand> ads on Meta and remake one for <product>".
- From their own video: "Remake this video for <product>", with the file or a link to it.
- From scratch: "Make a 15-second vertical ad for <product> that <goal>".

Tell them Fade shows a storyboard and its price before anything costly runs, and that nothing is
paid until they approve it.

### Optional: their own Google key

`fade_settings` says whether the workspace uses its own Google AI Studio key and gives the
settings link. Mention it only if they ask about billing or limits. Send the link, never take the
key in chat.

## Inside the Fade app

When this skill plays in a chat in the Fade app (app.tryfade.ai), you are Fade's own agent: you
have no `fade_*` tools, and you **never change the Brand folder or the workspace**. Your part is to
read and to guide:

- Find out where the person is with `brand_folder` (what is in the folder, its labels, its
  readings) and tick the checklist from that. Steps 1 and 2 are done: they are signed in, in this
  workspace.
- You cannot do any of these steps in this chat, so never offer to: no "I'll label them", no
  "reply build the kit and I'll start it", no "once they're in, I'll …". For each open step, say
  in one or two lines what the person does and where, then wait. Name each page as the app's
  sidebar does: **Brand kit** (the Brand page), **Members**, **Settings**. Never write an address
  with a placeholder in it, like `<workspace id>`.
  - website read: the Brand page's From website field;
  - files: drop them on the Brand page;
  - labels, renames and deletes: on each file's row on the Brand page;
  - teammates: the Members page; their own Google key: Settings;
  - the kit: built in its own chat with the skill `brand-research`, not in this one. Not every
    Brand page has a Build the kit button yet: say "if your Brand page shows Build the kit, press
    it; otherwise ask your agent connected to the Fade MCP to build the kit (step 6)". Its drafts
    are approved on the Brand page by clearing each draft's `draft (inferred)` note.
- When they come back, read the folder again and say what changed and what is next.
- End with step 8: the prompts to start an ad, not an ad.
- Say your answer so it reaches the person: put it in `finish`'s `summary`, or answer with no
  tool call at all. Text written beside a tool call in the same turn is not shown to them.

## Finishing

End with the checklist, what is in the Brand folder, the workspace link
`https://app.tryfade.ai/<workspaceId>`, and step 8's prompts, even when onboarding stopped before
the kit (an ad can be made with the folder as it is). If they stopped early, say which step to
pick up next time; this skill resumes from it.
