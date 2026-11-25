# Fade by Wix

Fade makes video ads for a brand. This plugin connects your AI app to Fade and adds its commands, so you can build a brand kit, remake ads that already work and plan new ones from a conversation.

## What it adds

- **The Fade connection:** the remote MCP server at `https://mcp.tryfade.ai/mcp`. You sign in with your Wix account in the browser (OAuth); the plugin holds no keys or tokens.
- **Six commands:**
  - `onboarding`: set up your brand in Fade, from its workspace and Brand folder to an approved brand kit.
  - `ad`: plan a video ad from your Brand folder, then approve its price before anything is generated.
  - `remake`: adapt a reference video's hook, pacing and structure to your brand.
  - `brand`: show the workspace's brand facts, assets and files still to read.
  - `projects`: list the workspace's projects and open one.
  - `status`: check the connection, the workspace and the progress of your current ad.

## What it sends and where

Everything goes to Fade's own server at `mcp.tryfade.ai`, run by Wix. The plugin runs no local programs, scripts or hooks. When you use it, Fade receives what you ask for in the conversation, the files you add to your Brand folder, and the websites you ask it to read. For a new ad, Fade shows a plan with its price, then a storyboard, and waits for your approval before it makes the video.

Fade is in early access: signing in needs an invited Wix account, and making videos uses your workspace's balance, bought at [app.tryfade.ai](https://app.tryfade.ai).

## Install

In Claude Code: `/plugin install fade --marketplace wix-incubator/fade`, then `/mcp` → fade → Authenticate. In Claude and Cowork: Customize → Plugins → Add marketplace → `wix-incubator/fade`, then connect Fade and sign in with Wix.

## Privacy, terms and support

- Privacy policy: https://www.tryfade.ai/privacy
- Terms of use: https://www.tryfade.ai/terms-of-use
- Support: https://github.com/wix-incubator/fade/issues
