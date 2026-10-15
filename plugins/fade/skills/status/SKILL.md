---
name: status
description: "Check the Fade connection, active workspace and progress of your current ad."
disable-model-invocation: true
user-invocable: true
argument-hint: "[request]"
---

Check Fade status

Use the current Fade tool catalog and its input schemas. If Fade is signed out or not connected, sign the person in first: if Fade offers an authenticate tool, call it; if you can run shell commands, open the link it returns in the person's browser yourself (open on macOS, xdg-open on Linux, start on Windows) and tell them to finish signing in with Wix there, otherwise give them the link. Without that tool, tell them to connect https://mcp.tryfade.ai/mcp and sign in with Wix.

Read fade_workspace to identify the active workspace. State its name before starting work. If the request names another workspace or brand, resolve it through fade_workspace before continuing.

Use existing Fade tools for the work and preserve the sessionId, projectId, planId and next-step instructions they return. An accepted: false response needs its stated remedy; do not repeat it unchanged.

Read fade_whoami to check the hosted connection and workspace. If fade_client_status is available, read it for the installed client release/build and connected runtime metadata.

For an existing sessionId, call fade_status and report its current position, completed work and the next decision. Do not start a new session or retry paid work to check status.

Report versions and capabilities only when a tool returned them. If the host does not expose installed client metadata, say that the client version is unavailable rather than guessing.

User request: $ARGUMENTS
