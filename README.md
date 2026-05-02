# Contract Hero plugin marketplace

A Claude Code plugin marketplace catalog for plugins maintained by [Contract Hero](https://github.com/contract-hero) and the wider community of contributors. It currently advertises tooling for Sui Move development and developer-experience extensions.

## Install

In a Claude Code session:

```
/plugin marketplace add contract-hero/plugin-marketplace
/plugin install sui-pilot@contract-hero
/plugin install codex-bridge@contract-hero
/plugin install sui-deepbook-course@contract-hero
```

Restart Claude Code after install so MCP servers spawn correctly.

You can also browse and install via the `/plugin` UI once the marketplace is added.

## Plugins

| Name | Description | Source | Docs |
|---|---|---|---|
| [`sui-pilot`](https://github.com/alilloig/sui-pilot) | Sui Move development plugin with LSP integration, bundled docs (Sui, Walrus, Seal, TS SDK), and code quality / review skills. | `alilloig/sui-pilot` | https://github.com/alilloig/sui-pilot |
| [`codex-bridge`](https://github.com/alilloig/codex-bridge) | Bridge to OpenAI Codex CLI — `/codex` for direct calls and `/claudex` for multi-round Claude↔Codex prompt refinement. | `alilloig/codex-bridge` | https://github.com/alilloig/codex-bridge |
| [`code-forge`](https://github.com/alilloig/code-forge) | Multi-agent build system with TDD-as-phase, parallel review, best-of-N implementer, and forge-guard hook discipline. | `alilloig/code-forge` | https://github.com/alilloig/code-forge |
| [`sui-deepbook-course`](https://github.com/alilloig/sui-mcp-course) | Interactive Sui DeepBook course for advanced DeFi developers — `/sui-deepbook-course:start` opens a guided lesson driven by an MCP server, with preflight diagnostics and an escalating help ladder. | `alilloig/sui-mcp-course` | https://github.com/alilloig/sui-mcp-course |

## What this repo contains

```
plugin-marketplace/
├── .claude-plugin/
│   └── marketplace.json   ← the catalog Claude Code reads
├── README.md              ← this file
└── LICENSE                ← Apache-2.0
```

The catalog references each plugin by GitHub source. Plugin sources are pinned to the default branch (`main`) of each plugin repo today; we may move to tag-pinned references (`ref: vX.Y.Z`) once the plugins adopt a stable release cadence.

## Adding a plugin

To propose a new plugin for this marketplace:

1. Open a PR that appends an entry to `plugins[]` in `.claude-plugin/marketplace.json`. Keep it sorted/grouped sensibly. Required fields per entry are `name` (kebab-case) and `source`; recommended fields are `description`, `homepage`, `repository`, `license`, `category`, and `keywords`.
2. Make sure the plugin repository contains a valid `.claude-plugin/plugin.json` so Claude Code can resolve its components (commands, skills, agents, hooks, MCP servers).
3. Validate locally before submitting:
   ```bash
   claude plugin validate .
   ```
4. Optionally test end-to-end by adding the marketplace from your local clone:
   ```
   /plugin marketplace add ./
   /plugin install <plugin-name>@contract-hero
   ```

The full marketplace schema lives at <https://code.claude.com/docs/en/plugin-marketplaces>.

## License

Apache-2.0 — see [LICENSE](./LICENSE). Each plugin retains its own license, declared per-entry in the catalog.
