---
name: projects
description: "List cloud projects in the active workspace and open the project you choose."
disable-model-invocation: true
user-invocable: true
argument-hint: "[request]"
---

Browse Fade projects

Use the current Fade tool catalog and its input schemas. If Fade is signed out or not connected, sign the person in first. In Codex, run `codex mcp login fade` yourself (it opens the browser for the Wix sign-in), then tell them to restart Codex. Elsewhere, if Fade offers an authenticate tool, call it; if you can run shell commands, open the link it returns in the person's browser yourself (open on macOS, xdg-open on Linux, start on Windows) and tell them to finish signing in with Wix there, otherwise give them the link. Without either, tell them to connect https://mcp.tryfade.ai/mcp and sign in with Wix.

Read fade_workspace to identify the active workspace. State its name before starting work. If the request names another workspace or brand, resolve it through fade_workspace before continuing.

Use existing Fade tools for the work and preserve the sessionId, projectId, planId and next-step instructions they return. An accepted: false response needs its stated remedy; do not repeat it unchanged.

Read fade_projects for the active workspace. Show each project's name and the available status or page returned by Fade. Follow pagination when the person requests the full list.

Say which workspace the list belongs to. Do not present a local studio library as all account projects. Opening or reading a project does not authorize paid work.

User request: $ARGUMENTS
