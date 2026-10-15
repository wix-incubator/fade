---
name: brand
description: "Show the active workspace's Brand facts, assets and files that still need reading."
disable-model-invocation: true
user-invocable: true
argument-hint: "[request]"
---

Read your Fade Brand folder

Use the current Fade tool catalog and its input schemas. If Fade is signed out or not connected, sign the person in first: if Fade offers an authenticate tool, call it; if you can run shell commands, open the link it returns in the person's browser yourself (open on macOS, xdg-open on Linux, start on Windows) and tell them to finish signing in with Wix there, otherwise give them the link. Without that tool, tell them to connect https://mcp.tryfade.ai/mcp and sign in with Wix.

Read fade_workspace to identify the active workspace. State its name before starting work. If the request names another workspace or brand, resolve it through fade_workspace before continuing.

Use existing Fade tools for the work and preserve the sessionId, projectId, planId and next-step instructions they return. An accepted: false response needs its stated remedy; do not repeat it unchanged.

Read fade_brand without mutation arguments. Summarize the existing facts, product assets and guidelines relevant to the person's request. Distinguish read, pending and unread files and identify missing information.

Do not create a second brand store. If the person asks to add or update Brand material, use the current fade_import schema with onDone: brand or the Brand page returned by Fade, and follow its confirmation steps.

User request: $ARGUMENTS
