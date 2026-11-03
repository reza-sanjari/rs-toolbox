# rs-toolbox

Personal Claude plugin marketplace with reusable skills (and later hooks and sub-agents). It works in **Claude Code** and in **claude.ai** (web and Desktop Chat tab).

| Plugin    | Contents                     | Who installs it       |
|-----------|------------------------------|-----------------------|
| `general` | Everyday skills              | Everyone (family too) |
| `coding`  | Development skills           | Reza only             |

> In claude.ai web chat and the Desktop Chat tab, only **skills** are supported. Hooks and sub-agents run only in Claude Code and Cowork.

## Modes

A "mode" is just the set of plugins you install:

- **General mode:** install `general` only.
- **Full mode:** install `general` **and** `coding`.

There is no separate "full" plugin, so no skill ever shows up twice.

## Install: Claude Code

```
/plugin marketplace add reza-sanjari/rs-toolbox
/plugin install general@rs-toolbox
/plugin install coding@rs-toolbox     # only for Reza
```

### Switching modes in Claude Code

Set the mode in `~/.claude/settings.json`. To set it for one project only, use that project's `.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "general@rs-toolbox": true,
    "coding@rs-toolbox": true
  }
}
```

- Full mode: both `true`.
- General mode: set `coding@rs-toolbox` to `false`.

### Updates in Claude Code

- **Auto-update** is off by default for third-party marketplaces. To turn it on, open `/plugin`, go to the **Marketplaces** tab, and enable auto-update for `rs-toolbox`. Updates are then pulled at startup.
- **Manual update:**
  ```
  /plugin marketplace update rs-toolbox
  /plugin update general@rs-toolbox
  /plugin update coding@rs-toolbox
  ```
- **Test locally without installing:**
  ```
  claude --plugin-dir ./plugins/general
  ```

## Install: claude.ai (web / Desktop chat)

1. Go to **Customize → Plugins → Personal plugins → "+" → "Add marketplace" → "Add from a repository"** and enter `reza-sanjari/rs-toolbox`.
2. Install `general` (and `coding` for Reza only).
3. For automatic updates, open the marketplace's menu in the **Personal** tab and turn on **"Sync automatically"**. Each user has to do this in their own account.
4. Each user, family included, needs a paid plan (Pro, Max, Team or Enterprise).

To switch modes in claude.ai, install, uninstall or disable plugins under **Customize → Plugins**. There is no text toggle.

## Repository layout

```
rs-toolbox/
├── .claude-plugin/
│   └── marketplace.json
├── plugins/
│   ├── general/
│   │   ├── .claude-plugin/plugin.json
│   │   ├── skills/
│   │   ├── hooks/      (reserved)
│   │   └── agents/     (reserved)
│   └── coding/
│       ├── .claude-plugin/plugin.json
│       ├── skills/
│       ├── hooks/      (reserved)
│       └── agents/     (reserved)
└── README.md
```

Rules:

- Only `plugin.json` goes inside `.claude-plugin/`. `skills/`, `hooks/` and `agents/` sit at the plugin root.
- Don't add hook config files (such as `hooks/hooks.json`) until hooks are actually needed.
- No top-level `bin/` directory in any plugin, because claude.ai rejects plugins that have one.
- No `version` field in `plugin.json` or `marketplace.json`, so every pushed commit counts as a new version.
- No `CLAUDE.md` anywhere. Plugins install at user level and work in every project.
- Use `skills/` only, with no `commands/` folder.

## Adding a skill

Each skill is a folder: `plugins/<plugin>/skills/<skill-name>/SKILL.md`. Supporting files (reference docs, templates, scripts) go in the same folder.

```markdown
---
name: example-skill
description: Do X. Use whenever the user asks for X, mentions Y, or wants Z.
---
# Instructions
...
```

- Skills are namespaced by plugin. A skill `review` in `coding` is invoked as `/coding:review`.
- Claude decides whether to invoke a skill automatically based on its **`description`**. Write it as "when to use this", with concrete triggers, phrasings and situations.
- Several narrow skills trigger more reliably than one broad skill.
- For skills that should only run when called by hand (deploy, release, and similar), add `disable-model-invocation: true` to the frontmatter.
- `$ARGUMENTS` captures the text typed after the slash command.
