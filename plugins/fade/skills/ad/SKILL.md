---
name: ad
description: "Plan a video ad from your workspace's Brand folder, then approve its price before generation."
disable-model-invocation: true
user-invocable: true
argument-hint: "[request]"
---

Create an ad with Fade

Use the current Fade tool catalog and its input schemas. If Fade is signed out, help the person sign in before continuing.

Read fade_workspace to identify the active workspace. State its name before starting work. If the request names another workspace or brand, resolve it through fade_workspace before continuing.

Use existing Fade tools for the work and preserve the sessionId, projectId, planId and next-step instructions they return. An accepted: false response needs its stated remedy; do not repeat it unchanged.

Read the active workspace's Brand folder with fade_brand before planning. Use its existing facts and assets. State which files are read, pending or unread, and ask only for missing facts needed for this ad. Do not invent claims, testimonials or product details.

Send the person's brief and the Brand context to fade_chat. Continue the returned session using fade_status and fade_answer where needed. Keep the person's request intact.

Show the proposed ad plan and its quoted price. Call fade_approve only after the person approves that specific plan and price. A command invocation does not authorize paid analysis, retrieval or generation.

After approved work finishes, show the Fade review or export page returned by the tools. State what completed and what still needs the person's decision.

User request: $ARGUMENTS
