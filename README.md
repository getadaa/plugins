# Adaa plugins

Plugin marketplace for [Claude Code](https://code.claude.com) and
[Codex](https://developers.openai.com/codex).

## Install

Claude Code:

```sh
/plugin marketplace add getadaa/plugins
/plugin install <plugin>@adaa
```

Codex:

```sh
codex plugin marketplace add getadaa/plugins
```

## Layout

Each plugin lives in `plugins/<name>/` and is shared by both clients:

```
plugins/<name>/
├── .claude-plugin/plugin.json   # Claude Code manifest
├── plugin.json                  # Codex manifest
└── skills/<skill>/SKILL.md      # read by both
```

The marketplaces are listed twice because the clients use different schemas:

- `.claude-plugin/marketplace.json` for Claude Code
- `.agents/plugins/marketplace.json` for Codex

## Adding a plugin

1. Create `plugins/<name>/` with the layout above.

   `.claude-plugin/plugin.json`:

   ```json
   {
     "name": "<name>",
     "description": "…",
     "version": "0.1.0"
   }
   ```

   `plugin.json`:

   ```json
   {
     "$schema": "https://agent-plugins.org/schemas/1.0.0/plugin.schema.json",
     "name": "<name>",
     "version": "0.1.0",
     "description": "…"
   }
   ```

2. Add it to `.claude-plugin/marketplace.json`:

   ```json
   { "name": "<name>", "source": "./plugins/<name>", "description": "…" }
   ```

3. Add it to `.agents/plugins/marketplace.json`:

   ```json
   {
     "name": "<name>",
     "source": { "source": "local", "path": "./plugins/<name>" },
     "policy": { "installation": "AVAILABLE", "authentication": "ON_INSTALL" },
     "category": "Productivity"
   }
   ```

4. Check the Claude Code side with `claude plugin validate .`.
