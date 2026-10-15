# Opfield for AI agents

Give Codex, Claude Code, Cursor, and other compatible coding agents the operating context they need to work with [Opfield](https://opfield.dev) through its authenticated remote MCP server.

Opfield is a self-hosted infrastructure control plane for a fleet of nodes: ingress and TLS, Secure Links through relays, Docker workloads and managed databases and storage with availability across nodes, Pages, Apps, monitoring, permissions, and AI/MCP. The skills teach an agent to work through Opfield's own resources instead of shell access to the nodes.

## Quick install

The universal Agent Skills route works across supported agents and installs the whole set of Opfield skills:

```bash
npx skills add opfield/opfield-skills
```

The installer lists every skill in the package; keep them all selected, or add `--skill '*'` to skip the selection. To install a single skill, name it:

```bash
npx skills add opfield/opfield-skills --skill publishing-html-pages
```

Native plugin installation is also available and always includes every skill:

```bash
# Claude Code
claude plugin marketplace add opfield/opfield-skills
claude plugin install opfield@opfield-skills

# Codex
codex plugin marketplace add opfield/opfield-skills --ref main
codex plugin add opfield@opfield-skills
```

Use one installation route per agent, then start a new session. See [INSTALL.md](INSTALL.md) for global installation, updates, verification, and MCP connection commands.

You can also ask an agent to install it for you:

```text
Install the Opfield skills or native plugin from
https://github.com/opfield/opfield-skills, follow its AGENTS.md and
INSTALL.md, then help me connect my Opfield instance through OAuth MCP.
```

The package does not connect to infrastructure or run anything automatically. Ask your agent to connect to your Opfield installation or use an existing `opfield` MCP connection.

## Renamed from the Gateway skills

This package was `the-square-labs/gateway-skills`, with the `gateway` plugin and the `using-gateway` entry skill. GitHub redirects the old repository name. Reinstall under the new names (see [INSTALL.md](INSTALL.md#upgrading-from-the-gateway-skills)); an Opfield server still answers `gateway://skills/using-gateway/...` with the `using-opfield` skill.

## Skills

`using-opfield` is the entry point. It teaches the resource model, MCP connection and discovery, safety rules, asynchronous Tasks, layered verification, and rollback, and routes each request to the domain skill that owns it.

| Skill | Use it to |
| --- | --- |
| `using-opfield` | Install or connect Opfield, discover tools, apply the safety rules, and pick the right domain skill. |
| `publishing-html-pages` | Publish an HTML report or static site to Opfield Pages and share a link: fixed or stable Tag links, expiry, access-list protection, rotation, Git builds. |
| `deploying-workloads` | Run Containers, blue/green Deployments, Compose Projects, and Git-source builds on Docker Nodes. |
| `high-availability` | Place a workload on several Docker Nodes in replicated or failover mode. |
| `ingress-and-domains` | Expose services on Domains with Routes, TLS certificates, Access Lists, Secure Links, and maintenance mode. |
| `internal-pki` | Run private certificate authorities and issue, link, or revoke internal certificates. |
| `databases` | Provision managed PostgreSQL, Redis, or ClickHouse, bind them privately, query them, and manage backups. |
| `storage` | Manage external and managed object storage, copy jobs, and the MinIO to SeaweedFS migration. |
| `managing-nodes` | Enroll, inspect, update, and repair Opfield Nodes. |
| `hosting-providers` | Create servers as Opfield Nodes through hosting accounts and Proxmox hosts, and manage the VMs behind them. |
| `observability` | Work with logs, alerts and webhooks, SIEM export, status pages, and the audit log. |
| `access-control` | Administer users, groups, scopes, tokens, and OAuth grants with least privilege. |

Connection and product documentation is available at [docs.opfield.dev](https://docs.opfield.dev/en/ai/agent-skills/).

## Package layout

- `skills/<name>`: one folder per skill, the single source of truth consumed by direct skill and native plugin installations.
- `.codex-plugin` and `.agents/plugins`: Codex package and marketplace metadata.
- `.claude-plugin`: Claude Code package and marketplace metadata.
- `assets`: plugin presentation assets.
- `scripts/validate-repository.py`: manifest and skill validation.

No always-on hooks are included. Opfield access remains explicit and governed by the connected user's OAuth session, scopes, confirmations, and the task requested by the user.

## License

The skill instructions in this repository are available under the MIT License. Opfield itself is distributed under its own license terms.
