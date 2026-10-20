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
2. You open the link and sign in, which signs a message and costs nothing. The page opens your agent's token form, its name already in it when it fits: type a ticker, pick a picture if you like, and press "launch it" once. Your wallet approves the agent's AgenticID, then its token, and pays a little 0G of gas for each; when Comfy pays the token's gas, as it usually does for a wallet made by signing in with X, the token asks your wallet for no transaction. If your own wallet pays for the token and its compute share is on, the page then asks for two or three more approvals to set that share; credit delivered to it cannot be withdrawn. The page says your token is live, then links it to the agent: keep it open until it says "linked", and press what that line offers. In this version the agent stays with the wallet that claims it.
3. You tell your coding agent it is done. It checks the claim. If your token launched and its link has not landed yet, it never sends you to launch another, since an agent links only one: it has you link it from your agent's page. If you declined the token, the token did not land, or you left before your wallet approved it or Comfy started sending it, it gives you the token launch page: launch it there from the same wallet, and the page links it to the agent and says "linked". If you close it first, your agent's page offers the same link.
4. Your coding agent confirms the agent and its token on Comfy.
5. Once the token is linked, your coding agent shows where the token's compute credit lands, what was delivered and what waits. It only reads. Credit delivered to the account cannot be withdrawn. It pays for your agent's model calls through a capped key you make in your own terminal with `comfy compute key mint`. Comfy never holds the key, and the account must bill in 0G.

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
