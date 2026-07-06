# Contract Hero plugin marketplace

A Claude Code and Codex plugin marketplace for plugins maintained by [Contract Hero](https://github.com/contract-hero) and the wider community of contributors. It currently advertises tooling for Sui Move development and developer-experience extensions.

**👉 [Browse the marketplace site](https://contract-hero.github.io/plugin-marketplace/)**

## Install

In a Claude Code session:

```
/plugin marketplace add contract-hero/plugin-marketplace
/plugin install sui-pilot@contract-hero
/plugin install codex-bridge@contract-hero
/plugin install acc-deepbook-course@contract-hero
```

Restart Claude Code after install so MCP servers spawn correctly.

You can also browse and install via the `/plugin` UI once the marketplace is added.

In Codex:

```bash
codex plugin marketplace add contract-hero/plugin-marketplace
codex plugin add sui-pilot@contract-hero
```

Codex reads `.agents/plugins/marketplace.json`. Unlike the Claude catalog, Codex 0.142 indexes plugin entries from local paths inside the marketplace snapshot, so Codex-compatible plugins live under `plugins/<plugin-name>/`.

## Plugins

| Name | Description | Source | Docs |
|---|---|---|---|
| [`sui-pilot`](https://github.com/contract-hero/sui-pilot) | Sui Move development plugin — 753 bundled docs, Move LSP, Sui Prover formal verification, and five specialized skills. | `contract-hero/sui-pilot` | https://contract-hero.github.io/sui-pilot/ |
| [`codex-bridge`](https://github.com/contract-hero/codex-bridge) | Bridge to OpenAI Codex CLI — `/codex` for direct calls and `/claudex` for multi-round Claude↔Codex prompt refinement. | `contract-hero/codex-bridge` | https://github.com/contract-hero/codex-bridge |
| [`code-forge`](https://github.com/contract-hero/code-forge) | Multi-agent build system with TDD-as-phase, parallel review, best-of-N implementer, and forge-guard hook discipline. | `contract-hero/code-forge` | https://github.com/contract-hero/code-forge |
| [`acc-deepbook-course`](https://github.com/contract-hero/acc-deepbook-course) | Interactive Sui DeepBook course for advanced DeFi developers — `/acc-deepbook-course:start` opens a guided lesson driven by an MCP server, with preflight diagnostics and an escalating help ladder. | `contract-hero/acc-deepbook-course` | https://github.com/contract-hero/acc-deepbook-course |

## What this repo contains

```
plugin-marketplace/
├── .agents/
│   └── plugins/
│       └── marketplace.json   ← the catalog Codex reads
├── .claude-plugin/
│   └── marketplace.json   ← the catalog Claude Code reads
├── plugins/
│   └── sui-pilot/         ← vendored Codex-compatible plugin snapshot
├── README.md              ← this file
└── LICENSE                ← Apache-2.0
```

The Claude catalog references each plugin by GitHub source. Plugin sources are pinned to the default branch (`main`) of each plugin repo today; we may move to tag-pinned references (`ref: vX.Y.Z`) once the plugins adopt a stable release cadence.

The Codex catalog currently exposes `sui-pilot` from `./plugins/sui-pilot`. Keep that directory synced from [`contract-hero/sui-pilot`](https://github.com/contract-hero/sui-pilot) when updating the Codex listing.

## Adding a plugin

To propose a new plugin for this marketplace:

1. Open a PR that appends an entry to `plugins[]` in `.claude-plugin/marketplace.json`. Keep it sorted/grouped sensibly. Required fields per entry are `name` (kebab-case) and `source`; recommended fields are `description`, `homepage`, `repository`, `license`, `category`, and `keywords`.
2. Make sure the plugin repository contains a valid `.claude-plugin/plugin.json` so Claude Code can resolve its components (commands, skills, agents, hooks, MCP servers).
3. If the plugin should be installable from Codex, add or update a vendored plugin snapshot under `plugins/<plugin-name>/` and append an entry to `.agents/plugins/marketplace.json` whose source is `{"source": "local", "path": "./plugins/<plugin-name>"}`.
4. Validate locally before submitting:
   ```bash
   claude plugin validate .
   ```
5. Optionally test Claude end-to-end by adding the marketplace from your local clone:
   ```
   /plugin marketplace add ./
   /plugin install <plugin-name>@contract-hero
   ```
6. Test Codex end-to-end from a local clone:
   ```bash
   codex plugin marketplace add ./
   codex plugin list --marketplace contract-hero --available
   codex plugin add <plugin-name>@contract-hero
   ```

The full marketplace schema lives at <https://code.claude.com/docs/en/plugin-marketplaces>.

## License

Apache-2.0 — see [LICENSE](./LICENSE). Each plugin retains its own license, declared per-entry in the catalog.
