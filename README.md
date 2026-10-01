# Contract Hero plugin marketplace

A Claude Code and Codex plugin marketplace for plugins maintained by [Contract Hero](https://github.com/contract-hero) and the wider community of contributors. It currently advertises tooling for Sui Move development and developer-experience extensions.

**👉 [Browse the marketplace site](https://contract-hero.github.io/plugin-marketplace/)**

## Install

In a Claude Code session:

```
/plugin marketplace add contract-hero/plugin-marketplace
/plugin install sui-pilot@contract-hero
/plugin install agentic-community-college@contract-hero
/plugin install acc-claude-sdk@contract-hero
```

Restart Claude Code after install so MCP servers spawn correctly.

You can also browse and install via the `/plugin` UI once the marketplace is added.

In Codex:

```bash
codex plugin marketplace add contract-hero/plugin-marketplace
codex plugin add sui-pilot@contract-hero
```

Codex reads `.agents/plugins/marketplace.json`. Use a current release with Git
plugin sources (Sui Pilot's native package is validated with Codex CLI 0.153.4).
Sui Pilot now installs directly from its canonical repository, including its
OpenAI manifest, shared skills, and two prebuilt **local stdio MCP servers**.
Start a new session after installation. A compatible local desktop client can
use the same marketplace; a browser-only session cannot run the local toolchain.

This marketplace is separate from OpenAI's public plugin directory. Public
distribution of a local MCP through that directory still requires coordination
with OpenAI; see the [local MCP guidance](https://developers.openai.com/plugins/guides/submit-claude-plugin).

## Plugins

| Name | Description | Source | Docs |
|---|---|---|---|
| [`sui-pilot`](https://github.com/contract-hero/sui-pilot) | Sui Move development plugin — 753 bundled docs, Move LSP, Sui Prover formal verification, and five specialized skills. | `contract-hero/sui-pilot` | https://contract-hero.github.io/sui-pilot/ |
| [`code-forge`](https://github.com/contract-hero/code-forge) | Multi-agent build system with TDD-as-phase, parallel review, best-of-N implementer, and forge-guard hook discipline. | `contract-hero/code-forge` | https://github.com/contract-hero/code-forge |
| [`agentic-community-college`](https://github.com/contract-hero/agentic-community-college) | Claude Code learning framework (v0.3 chapter model): a lesson is split into chapters defined by tests; the conductor implements each chapter in a seeded workspace, explains it with an HTML artifact, and closes with an end-to-end test and a summary. Bring a course plugin. | `contract-hero/agentic-community-college` | https://contract-hero.github.io/agentic-community-college/ |
| [`acc-claude-sdk`](https://github.com/contract-hero/acc-claude-sdk) | ACC course: build agents with the Claude Agent SDK (TypeScript), chapter by chapter. `/acc-claude-sdk:start` opens lesson 1: a first `query()`, a custom in-process tool, a testable CLI. | `contract-hero/acc-claude-sdk` | https://github.com/contract-hero/acc-claude-sdk |
| [`acc-deepbook-course`](https://github.com/contract-hero/acc-deepbook-course) | Previous work: Sui DeepBook course on the ACC v0.2 section model, pending migration to v0.3 chapters. `/acc-deepbook-course:start` once migrated. | `contract-hero/acc-deepbook-course` | https://github.com/contract-hero/acc-deepbook-course |
| [`acc-evm-wal`](https://github.com/contract-hero/acc-evm-wal) | Previous work: six Walrus × EVM lessons on the ACC v0.2 section model, pending migration to v0.3 chapters. | `contract-hero/acc-evm-wal` | https://github.com/contract-hero/acc-evm-wal |
| [`skypies`](https://github.com/contract-hero/skypies-plugin) | skypies plugin: share the HTML artifacts you build in Claude Code to your iPhone or another Mac over an end-to-end encrypted peer-to-peer link, no server. | `contract-hero/skypies-plugin` | https://contract-hero.github.io/skypies-core/ |

## What this repo contains

```
plugin-marketplace/
├── .agents/
│   └── plugins/
│       └── marketplace.json   ← the catalog Codex reads
├── .claude-plugin/
│   └── marketplace.json   ← the catalog Claude Code reads
├── plugins/
│   └── sui-pilot/         ← legacy snapshot; no longer used by the current catalog
├── README.md              ← this file
└── LICENSE                ← Apache-2.0
```

The Claude catalog references each plugin by GitHub source. Plugin sources are pinned to the default branch (`main`) of each plugin repo today; we may move to tag-pinned references (`ref: vX.Y.Z`) once the plugins adopt a stable release cadence.

The Codex catalog resolves `sui-pilot` from
[`contract-hero/sui-pilot`](https://github.com/contract-hero/sui-pilot) on `main`.
Maintain its native OpenAI package in that repository; do not update the legacy
snapshot to release a new version. For existing installations, run
`codex plugin marketplace upgrade contract-hero`, reinstall
`sui-pilot@contract-hero`, and start a new session.

## Adding a plugin

To propose a new plugin for this marketplace:

1. Open a PR that appends an entry to `plugins[]` in `.claude-plugin/marketplace.json`. Keep it sorted/grouped sensibly. Required fields per entry are `name` (kebab-case) and `source`; recommended fields are `description`, `homepage`, `repository`, `license`, `category`, and `keywords`.
2. Make sure the plugin repository contains a valid `.claude-plugin/plugin.json` so Claude Code can resolve its components (commands, skills, agents, hooks, MCP servers).
3. If the plugin should be installable from Codex, include a supported OpenAI manifest in its source repository and append an entry to `.agents/plugins/marketplace.json`. A Git source uses `{"source": "url", "url": "https://github.com/owner/repo.git", "ref": "main"}`. Local sources under `./plugins/<plugin-name>` remain available when vendoring is intentional.
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
