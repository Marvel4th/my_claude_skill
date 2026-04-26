# Installing Exported Skills on Another Device

This folder contains 100 Claude Code skills exported from three plugins:

| Folder | Plugin | Marketplace | Skills |
|---|---|---|---|
| `document-skills/` | `document-skills` | `anthropic-agent-skills` (built-in) | 17 |
| `example-skills/` | `example-skills` | `anthropic-agent-skills` (built-in) | 17 |
| `fullstack-dev-skills/` | `fullstack-dev-skills` | `jeffallan/claude-skills` (GitHub) | 66 |

There are two ways to install them on a new device. **Method 1 is strongly recommended** — it's one line per plugin and the plugins auto-update.

---

## Method 1 — Reinstall from the marketplaces (recommended)

Requires Claude Code installed and internet access. Run inside any Claude Code session:

```
/plugin install document-skills@anthropic-agent-skills
/plugin install example-skills@anthropic-agent-skills
/plugin marketplace add jeffallan/claude-skills
/plugin install fullstack-dev-skills@jeffallan
/reload-plugins
```

Notes:
- `anthropic-agent-skills` is the built-in Anthropic marketplace — no `marketplace add` needed.
- `jeffallan/claude-skills` is a third-party marketplace on GitHub (https://github.com/jeffallan/claude-skills); the `marketplace add` line registers it before installing.
- `/reload-plugins` activates them in the current session. New sessions pick them up automatically.

Verify installation:

```
/plugin
```

---

## Method 2 — Copy this folder onto the new device (offline / air-gapped)

Use this if the new device cannot reach the internet, or you want byte-identical skills without re-downloading.

### Step 1 — Copy the folder

Copy this entire `skill/` directory to the new device. Any location works (USB, network share, cloud sync). For the examples below, assume it lives at:

- **Windows:** `C:\Users\<you>\Desktop\claude\skill\`
- **macOS / Linux:** `~/Desktop/claude/skill/`

### Step 2 — Choose where to put each plugin

Claude Code looks for plugin skills under:

```
~/.claude/plugins/cache/<marketplace>/<plugin>/<version>/skills/
```

For these three plugins the exact target paths are:

| Source folder in this export | Target path on new device |
|---|---|
| `document-skills/` | `~/.claude/plugins/cache/anthropic-agent-skills/document-skills/<hash>/skills/` |
| `example-skills/` | `~/.claude/plugins/cache/anthropic-agent-skills/example-skills/<hash>/skills/` |
| `fullstack-dev-skills/` | `~/.claude/plugins/cache/fullstack-dev-skills/fullstack-dev-skills/<version>/skills/` |

`<hash>` and `<version>` are placeholders. The simplest path to a working install is to first run **Method 1** once on the new device (which creates the correct `<hash>`/`<version>` folder names), then overwrite the resulting `skills/` directory with the contents of this export. That guarantees the folder names match what Claude Code expects.

### Step 3 — Reload

After copying, run inside a Claude Code session on the new device:

```
/reload-plugins
```

Then verify with `/plugin` — all three plugins should appear, and `/<skill-name>` invocations should work.

---

## Method 3 — Project-local skills (no plugin system)

If you don't want to use plugins at all, you can drop individual skill folders into a project's `.claude/skills/` directory:

```
<your-project>/.claude/skills/<skill-name>/SKILL.md
<your-project>/.claude/skills/<skill-name>/references/...   (if present)
<your-project>/.claude/skills/<skill-name>/templates/...    (if present)
```

Or for user-level (available in every project):

```
~/.claude/skills/<skill-name>/SKILL.md
```

Copy whichever skill folders you want from `document-skills/`, `example-skills/`, or `fullstack-dev-skills/` here. They become available in any new Claude Code session — no `/reload-plugins` needed, just start a new session.

This is useful when you only need a few specific skills and don't want the whole bundle.

---

## Folder structure of each skill

Every skill folder contains at minimum a `SKILL.md` file with frontmatter that Claude Code reads to know when to invoke it. Some skills also ship with:

- `references/` — additional context Claude reads on demand
- `templates/` — starter files the skill copies into your project
- `LICENSE.txt` / `LICENSE`

Keep the folder structure intact when copying — the relative paths inside `SKILL.md` depend on it.

---

## Troubleshooting

- **`/plugin install` fails with "marketplace not found"** — for `fullstack-dev-skills`, run `/plugin marketplace add jeffallan/claude-skills` first.
- **Skills don't show up after copying** — run `/reload-plugins`, then start a new session. If still missing, check that the folder names under `~/.claude/plugins/cache/` exactly match Claude Code's expectations (use Method 1 once to generate them).
- **`/plugin` shows the plugin but skills are missing** — verify each skill folder contains a `SKILL.md` with valid YAML frontmatter (`name:`, `description:` at minimum).
- **Wrong version of `fullstack-dev-skills`** — this export is from version `0.4.12`. Newer versions may have added/renamed skills; reinstall via Method 1 to get the latest.

---

## Source versions (as exported)

- `document-skills` / `example-skills`: hash `5128e1865d67` from the `anthropic-agent-skills` marketplace
- `fullstack-dev-skills`: version `0.4.12` from `jeffallan/claude-skills`

Exported on 2026-04-26.
