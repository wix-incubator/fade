# Connecting the Fade MCP

Fade is a remote MCP server at `https://mcp.tryfade.ai/mcp`. It signs in with OAuth (a Wix
account) in the browser; no key or token is pasted anywhere.

| App                        | How                                                                                                                                                |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| Claude Code                | `claude mcp add --transport http fade https://mcp.tryfade.ai/mcp --scope user`, then `/mcp` → fade → Authenticate                                  |
| Codex CLI                  | `codex mcp add fade --url https://mcp.tryfade.ai/mcp`, then `codex mcp login fade`                                                                 |
| Cursor                     | Settings → MCP → Add, or in `~/.cursor/mcp.json`: `{"mcpServers": {"fade": {"url": "https://mcp.tryfade.ai/mcp"}}}`; then sign in when Cursor asks |
| Claude Desktop / claude.ai | Settings → Connectors → Add custom connector → URL `https://mcp.tryfade.ai/mcp` → Connect                                                          |
| Any other MCP host         | Add a Streamable HTTP server with that URL; the host's OAuth flow opens the sign-in                                                                |

In Codex, run `codex mcp login fade` yourself: it opens the browser for the Wix sign-in, then the
person restarts Codex.

If Fade is connected but not signed in, the app offers Fade's authenticate tool: call it and, if you
can run shell commands, open the link it returns in the person's browser yourself (`open` on macOS,
`xdg-open` on Linux, `start` on Windows); otherwise give them the link.

After connecting, the app may need a restart or a new chat before the `fade_*` tools appear.
Check with `fade_whoami`.

If sign-in fails or the tools never appear, the person can still do every step on the web at
https://app.tryfade.ai: the Brand page, Members, Settings and starting a session are all there.
