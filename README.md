# Service Network plugins

The official Service Network plugin for Claude and Codex. One folder installs on both.

It contains two reviewable pieces and nothing else:

- a small bootstrap skill that recognizes Service Network work, confirms the account and company it is working under, and loads the current operating guide and tool catalog from Service Network at run time;
- the hosted Service Network connection at `https://service-network.pages.dev/api/mcp`, which signs in through the browser.

The plugin contains no credentials and no company data. Tool schemas and product policy are not copied into it; they stay on the Service Network server, so an installed copy does not go stale.

Marketplace address: `https://github.com/local-service-sites/service-network-plugins` (`local-service-sites/service-network-plugins` as shorthand).

## Install in Claude

**Claude on the web, the desktop app, and mobile (chat and Cowork).** Open **Customize > Plugins**, add a marketplace, and paste `https://github.com/local-service-sites/service-network-plugins`. Install **Service Network Operator**, then connect Service Network on the plugin's **Connectors** tab. A plugin installed on your account also reaches Claude Code as a synced plugin at the next session start.

**Claude Code (terminal, IDE extensions, desktop Code tab).**

```sh
claude plugin marketplace add local-service-sites/service-network-plugins
claude plugin install service-network-operator@service-network
```

## Install in Codex

**Codex desktop app.** Open **Plugins**, add a marketplace, paste `https://github.com/local-service-sites/service-network-plugins`, and install **Service Network Operator**.

**Codex CLI.**

```sh
codex plugin marketplace add local-service-sites/service-network-plugins
codex plugin add service-network-operator@service-network
```

## Use with Meta Muse

Muse does not install plugins or skill files, so this folder does not apply to it. Muse connects to the same hosted Service Network connection as a **custom connector** that it builds for you in chat.

1. In Muse (the iOS or Android app, or the web), start a new chat and paste:

   > Build a custom integration to Service Network, the operations system for my home-service company. Its MCP server URL is https://service-network.pages.dev/api/mcp and its documentation is at https://service-network.pages.dev/connector. It signs in with OAuth (PKCE and dynamic client registration). Open the sign-in in the browser and let me approve it; never ask me to paste a password or token into the chat. Once connected, call account_context_get first and tell me which company you are working in. Then use capability_search and capability_get to find the right tool before doing anything else. Ask me before every call the documentation marks as a sensitive write.

2. Approve the Service Network sign-in when Muse opens it. Never paste a password or token into the chat.
3. Check that Muse names your company before it says anything about live records.

Muse runs in Meta's cloud, so the local project folder, the local runner, and scheduled Operations Updates still need an assistant on your computer. Meta does not review custom connectors, and the records you ask about pass through Meta's service. Service Network is not yet listed in the Muse connector directory.

The connector documentation, with every tool marked read, write, or sensitive write, is published at `https://service-network.pages.dev/connector` (also as `/connector.md`, `/connector.json`, and `/llms.txt`).

## What does not install from here

- **ChatGPT on the web and mobile** cannot add a marketplace from a repository. The plugin reaches those surfaces only after it is published to a ChatGPT workspace from the desktop app, or listed in the public directory. Until then, add `https://service-network.pages.dev/api/mcp` as a custom connector.
- **The Codex IDE extension** does not load plugins. It uses the MCP connection from the shared Codex configuration.

## First use

1. Start a fresh session and ask the assistant to check your Service Network access.
2. Finish the browser sign-in for the `service-network` connection. Never paste a password or token into the chat.
3. The assistant should call `account_context_get` and name your company before it claims anything about live records.

Work that needs this computer, such as the local project folder, the local runner, and scheduled Operations Updates, is set up from the Service Network dashboard under **Agent install**. The plugin does not replace it.

## Layout

```
.claude-plugin/marketplace.json      Claude marketplace
LICENSE                              MIT
.agents/plugins/marketplace.json     Codex marketplace
plugins/service-network-operator/
  plugin.json                        portable manifest (Codex reads this first)
  mcp.json                           portable MCP declaration
  .claude-plugin/plugin.json         Claude manifest
  .mcp.json                          Claude MCP declaration
  .codex-plugin/plugin.json          fallback for older Codex builds
  skills/service-network/SKILL.md    bootstrap skill
```

The three manifests must agree on name, version, and description, and both MCP files must point at the same address. Bump `version` in all three on every change; both apps cache an installed plugin by version.
