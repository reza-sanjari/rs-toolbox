# rs-toolbox

Personal Claude plugin marketplace with reusable skills (and later hooks and sub-agents). It works in **Claude Code** and in **claude.ai** (web and Desktop Chat tab).

| Plugin   | Contents           | When to install        |
|----------|--------------------|------------------------|
| `rs`     | Everyday skills    | Always                 |
| `rs-dev` | Development skills | Only for development   |

> In claude.ai web chat and the Desktop Chat tab, only **skills** are supported. Hooks and sub-agents run only in Claude Code and Cowork.

## Skills

### `rs`

| Skill            | Starts                        | Lasts        | What it does                                                   |
|------------------|-------------------------------|--------------|----------------------------------------------------------------|
| `/rs:brief`      | Automatically or by command   | Whole session | Short, focused answers in simple sentences. Say "stop brief" or "normal mode" to turn it off. |
| `/rs:tldr`       | By command only               | One answer   | A few sentences at most, nothing extra.                        |
| `/rs:detailed`   | By command only               | One answer   | A complete explanation in simple sentences, without repetition. |
| `/rs:grill`      | Automatically or by command   | Whole session | Asks clarifying questions in small batches, flags contradictions, skips trivial points, and confirms a summary before starting. Say "stop grill" or "normal mode" to turn it off. |
| `/rs:grill-full` | By command only               | Whole session | Like `grill`, but asks about every open point and picks no defaults. |

`/rs:tldr` and `/rs:detailed` accept a question after the command. Without one, they apply to the next message.

## Modes

A "mode" is just the set of plugins you install:

- **Everyday mode:** install `rs` only.
- **Full mode:** install `rs` **and** `rs-dev`.

There is no separate "full" plugin, so no skill ever shows up twice.

## Install: Claude Code

```
/plugin marketplace add reza-sanjari/rs-toolbox
/plugin install rs@rs-toolbox
/plugin install rs-dev@rs-toolbox     # optional, for development
```

### Switching modes in Claude Code

Set the mode in `~/.claude/settings.json`. To set it for one project only, use that project's `.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "rs@rs-toolbox": true,
    "rs-dev@rs-toolbox": true
  }
}
```

- Full mode: both `true`.
- Everyday mode: set `rs-dev@rs-toolbox` to `false`.

### Updates in Claude Code

- **Auto-update** is off by default for third-party marketplaces. To turn it on, open `/plugin`, go to the **Marketplaces** tab, and enable auto-update for `rs-toolbox`. Updates are then pulled at startup.
- **Manual update:**
  ```
  /plugin marketplace update rs-toolbox
  /plugin update rs@rs-toolbox
  /plugin update rs-dev@rs-toolbox
  ```
- **Test locally without installing:**
  ```
  claude --plugin-dir ./plugins/rs
  ```

## Install: claude.ai (web / Desktop chat)

1. Go to **Customize → Plugins → Personal plugins → "+" → "Add marketplace" → "Add from a repository"** and enter `reza-sanjari/rs-toolbox`.
2. Install `rs` (and `rs-dev` if needed).
3. For automatic updates, open the marketplace's menu in the **Personal** tab and turn on **"Sync automatically"**. Each user has to do this in their own account.
4. Each user needs a paid plan (Pro, Max, Team or Enterprise).

To switch modes in claude.ai, install, uninstall or disable plugins under **Customize → Plugins**. There is no text toggle.

## Repository layout

```
rs-toolbox/
├── .claude-plugin/
│   └── marketplace.json
├── plugins/
│   ├── rs/
│   │   ├── .claude-plugin/plugin.json
│   │   ├── skills/
│   │   ├── hooks/      (reserved)
│   │   └── agents/     (reserved)
│   └── rs-dev/
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

- Skills are namespaced by plugin. A skill `review` in `rs-dev` is invoked as `/rs-dev:review`.
- Claude decides whether to invoke a skill automatically based on its **`description`**. Write it as "when to use this", with concrete triggers, phrasings and situations.
- Several narrow skills trigger more reliably than one broad skill.
- For skills that should only run when called by hand (deploy, release, and similar), add `disable-model-invocation: true` to the frontmatter.
- `$ARGUMENTS` captures the text typed after the slash command.
