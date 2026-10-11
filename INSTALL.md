# Install Opfield for your agent

The repository supports two distribution paths:

- install the skills with `npx skills`, either the whole set or individual skills;
- install the native `opfield` plugin in Claude Code or Codex, which always contains every skill.

Choose one path for each agent. The plugin and the direct skills contain the same operating instructions. `using-opfield` is the entry point and routes to the domain skills, so install the whole set unless you have a reason to trim it.

## Universal skill installation

Install every Opfield skill in the current project and select the agents that should receive them:

```bash
npx skills add opfield/opfield-skills
```

The installer lists all skills in the package; keep them all selected. To skip the selection, name them explicitly:

```bash
npx skills add opfield/opfield-skills --skill '*'
```

Install for the current user across projects:

```bash
npx skills add opfield/opfield-skills --skill '*' -g
```

Target specific agents non-interactively:

```bash
npx skills add opfield/opfield-skills --skill '*' -a codex -a claude-code -y
```

Install only some skills by name. Include `using-opfield` whenever you trim the set, because the domain skills rely on its connection and safety rules:

```bash
npx skills add opfield/opfield-skills --skill using-opfield --skill publishing-html-pages
```

List the skills without installing anything:

```bash
npx skills add opfield/opfield-skills --list
```

Update installed skills later with `npx skills update`.

## Claude Code plugin

```bash
claude plugin marketplace add opfield/opfield-skills
claude plugin install opfield@opfield-skills
```

Verify or update it:

```bash
claude plugin list
claude plugin marketplace update opfield-skills
```

## Codex plugin

```bash
codex plugin marketplace add opfield/opfield-skills --ref main
codex plugin add opfield@opfield-skills
```

Verify or update it:

```bash
codex plugin list
codex plugin marketplace upgrade opfield-skills
codex plugin remove opfield
codex plugin add opfield@opfield-skills
```

Start a new Claude Code or Codex session after installing or updating so the skills are loaded into the new agent context.

## Upgrading from the Gateway skills

The package was renamed: repository `the-square-labs/gateway-skills` is now `opfield/opfield-skills`, the plugin `gateway@gateway-skills` is `opfield@opfield-skills`, and the entry skill `using-gateway` is `using-opfield`. The domain skills keep their names. Remove the old installation, then install the new one:

```bash
# Skills installed with npx skills
npx skills remove using-gateway
npx skills add opfield/opfield-skills --skill '*'

# Claude Code
claude plugin uninstall gateway@gateway-skills
claude plugin marketplace remove gateway-skills
claude plugin marketplace add opfield/opfield-skills
claude plugin install opfield@opfield-skills

# Codex
codex plugin remove gateway
codex plugin marketplace remove gateway-skills
codex plugin marketplace add opfield/opfield-skills --ref main
codex plugin add opfield@opfield-skills
```

An Opfield server keeps serving the old entry skill name over MCP (`gateway://skills/using-gateway/SKILL.md` and the `skill-using-gateway` prompt return `using-opfield`), so agents that still ask for it keep working.

## Connect the installed skills to Opfield

This package does not bundle a fixed MCP server because every Opfield installation has its own hostname and OAuth boundary.

For Codex:

```bash
codex mcp add opfield --url https://opfield.example.com/api/mcp
codex mcp login opfield
```

For Claude Code:

```bash
claude mcp add --transport http opfield --scope user https://opfield.example.com/api/mcp
claude mcp login opfield
```

Replace `opfield.example.com` with the public hostname of your installation. Complete authentication in the browser; do not paste a Opfield API token into the agent chat.

See the [Opfield agent guide](https://docs.opfield.dev/en/ai/agent-skills/) for permissions, first prompts, and MCP setup details.
