---
name: brand
description: "Show the active workspace's Brand facts, assets and files that still need reading."
disable-model-invocation: true
user-invocable: true
argument-hint: "[request]"
---

Read your Fade Brand folder

Use the current Fade tool catalog and its input schemas. If Fade is signed out, help the person sign in before continuing.

Read fade_workspace to identify the active workspace. State its name before starting work. If the request names another workspace or brand, resolve it through fade_workspace before continuing.

Use existing Fade tools for the work and preserve the sessionId, projectId, planId and next-step instructions they return. An accepted: false response needs its stated remedy; do not repeat it unchanged.

Read fade_brand without mutation arguments. Summarize the existing facts, product assets and guidelines relevant to the person's request. Distinguish read, pending and unread files and identify missing information.

Do not create a second brand store. If the person asks to add or update Brand material, use the current fade_import schema with onDone: brand or the Brand page returned by Fade, and follow its confirmation steps.

User request: $ARGUMENTS
