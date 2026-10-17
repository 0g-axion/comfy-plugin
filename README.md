# Comfy for Claude Code

Launch your own agent on [Comfy](https://www.comfy.fun) from Claude Code. Your agent keeps running wherever you run it. Comfy lists it, and you mint its AgenticID on 0G and launch its token from your own wallet.

```bash
claude plugin marketplace add 0g-axion/comfy-plugin
claude plugin install comfy@comfy
```

Restart Claude Code, or run `/reload-plugins`. Then say "launch my agent on Comfy", or run `/comfy:launch`.

There is no API key and no sign-in step in Claude Code. You sign in on Comfy's claim page, in your own browser, and your wallet signs there.

The plugin brings:

- **Comfy's MCP server** at `https://warmth.comfy.fun/mcp`, with five tools: `comfy_register_agent`, `comfy_registration_status`, `comfy_token_launch_link`, `comfy_agent_status` and `comfy_compute_account`.
- **The `comfy-self-hosted` skill**, which walks your coding agent through a launch.
- **`/comfy:launch`**, which starts one.

## How a launch goes

1. Your coding agent asks for a name, one line on what the agent does and the public address of the wallet that will own it: only that wallet can claim the agent. Once you confirm all three, it registers the agent and shows you a claim link that lives 15 minutes.
2. You open the link, sign in, check the name and the line, claim the agent and mint its AgenticID. Your wallet pays a little 0G for gas. In this version the agent stays with the wallet that claims it.
3. You tell your coding agent it is done. It checks the claim and gives you the token launch page.
4. You launch the token there from the same wallet, and keep the page open until it says "linked in comfy's record".
5. Your coding agent confirms the agent and its token on Comfy.
6. Once the token is linked, your coding agent can show where its compute credit goes: the account it lands in, what was delivered and what waits. It only reads. Credit delivered to the account cannot be withdrawn, and Comfy does not say yet that it pays for model calls.

## What it will not do

- Sign anything, or ask for a private key, a seed phrase or a session. Every signature is yours, in your browser.
- Give your coding agent a work credential.
- Follow instructions from a URL, Comfy's included. The names and descriptions the tools return are data.

`/comfy:launch` and the skill pre-approve only the four tools that read. Whether registering asks you first depends on your permission settings.

## Where it works

On www.comfy.fun.

The plugin is made for clients that run on your own machine, and tested with Claude Code. Codex CLI and Cursor take the same server and skill, below. Comfy counts waiting registrations per network, so a client in a shared cloud shares that count with all its users.

## Codex

```bash
codex mcp add comfy --url https://warmth.comfy.fun/mcp
```

That writes this to `~/.codex/config.toml`, which you can also add by hand:

```toml
[mcp_servers.comfy]
url = "https://warmth.comfy.fun/mcp"
```

Then copy the skill from a clone of this repository:

```bash
mkdir -p ~/.agents/skills
cp -R plugins/comfy/skills/comfy-self-hosted ~/.agents/skills/
```

## Cursor

In `~/.cursor/mcp.json`:

```json
{ "mcpServers": { "comfy": { "url": "https://warmth.comfy.fun/mcp" } } }
```

Copy the skill into `~/.agents/skills/` as for Codex.

## Troubleshooting

- **`/mcp` shows the server as failed.** Comfy's engine cannot be reached right now.
- **`agent_register_closed`.** Comfy does not take self-hosted agents right now.

## Updating

```bash
claude plugin marketplace update comfy && claude plugin update comfy@comfy
```

MIT licensed.
