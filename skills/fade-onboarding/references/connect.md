# Connecting the Fade MCP

Fade is a remote MCP server at `https://mcp.tryfade.ai/mcp`. It signs in with OAuth (a Wix
account) in the browser; no key or token is pasted anywhere.

| App | How | Starter command |
| --- | --- | --- |
| Claude (claude.ai, Desktop, Cowork) | Open https://claude.ai/new?q=set+up+fade+%E2%80%94+https%3A%2F%2Fmcp.tryfade.ai%2Fllms.txt, or Customize → Plugins (https://claude.ai/new#customize/plugins) → Add → Add marketplace → `wix-incubator/fade` → add **Fade** → Connectors tab → Connect. Remove any older URL connector first. On Claude Team/Enterprise, an owner can set `wix-incubator/fade` to Installed by default. | `/fade:onboarding` |
| Claude Code | `claude plugin marketplace add wix-incubator/fade && claude plugin install fade@fade`, then `/mcp` → fade → Authenticate | `/fade:onboarding` |
| Codex (App & CLI) | `codex plugin marketplace add wix-incubator/fade && codex plugin add fade@fade`, then `codex mcp login fade` and restart Codex | `$fade:onboarding` |
| Cursor | Open https://cursor.com/link/prompt?text=set+up+fade+%E2%80%94+https%3A%2F%2Fmcp.tryfade.ai%2Fllms.txt so Cursor's agent adds `fade` to `~/.cursor/mcp.json` and writes the six `~/.cursor/skills/fade-*` commands; then Customize → fade → sign in, and `Developer: Reload Window` | `/fade-onboarding` |
| ChatGPT | Settings → Connectors → add `https://mcp.tryfade.ai/mcp` and sign in with Wix | `"Onboard me to Fade."` |
| Any other MCP host | Add a Streamable HTTP server at `https://mcp.tryfade.ai/mcp`; the host's OAuth flow opens the sign-in | `"Onboard me to Fade."` |

In Codex, run `codex mcp login fade` yourself: it opens the browser for the Wix sign-in, then the
person restarts Codex.

If Fade is connected but not signed in, the app offers Fade's authenticate tool: call it and, if you
can run shell commands, open the link it returns in the person's browser yourself (`open` on macOS,
`xdg-open` on Linux, `start` on Windows); otherwise give them the link.

After connecting, the app may need a restart or a new chat before the `fade_*` tools appear.
Check with `fade_whoami`.

If sign-in fails or the tools never appear, the person can do every step on the web at
https://app.tryfade.ai: Brands, Account menu → Workspace settings / Members, and starting a session
are all there.
