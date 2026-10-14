---
name: projects
description: "List cloud projects in the active workspace and open the project you choose."
disable-model-invocation: true
user-invocable: true
argument-hint: "[request]"
---

Browse Fade projects

Use the current Fade tool catalog and its input schemas. If Fade is signed out, help the person sign in before continuing.

Read fade_workspace to identify the active workspace. State its name before starting work. If the request names another workspace or brand, resolve it through fade_workspace before continuing.

Use existing Fade tools for the work and preserve the sessionId, projectId, planId and next-step instructions they return. An accepted: false response needs its stated remedy; do not repeat it unchanged.

Read fade_projects for the active workspace. Show each project's name and the available status or page returned by Fade. Follow pagination when the person requests the full list.

Say which workspace the list belongs to. Do not present a local studio library as all account projects. Opening or reading a project does not authorize paid work.

User request: $ARGUMENTS
