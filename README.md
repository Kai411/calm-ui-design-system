# Calm UI design system

A Claude skill for building calm, consistent web app interfaces: dashboards, trackers, internal tools and chat apps.

Before writing any styles, Claude asks you to pick:

- **A colour scheme**: Warm paper, Cool slate, Forest, Plum, Ochre or Mono ink (or your own brand colour)
- **A font pairing**: six Google Fonts pairs, such as Hanken Grotesk + JetBrains Mono or Geist + Geist Mono (or your own)

It then generates light and dark OKLCH colour tokens from your choice and applies the rest of the system: type scale, spacing, motion (short, ease-out, never bouncy), components (buttons, fields, tabs, list rows, chat, modals, menus, toasts), dark mode and phone rules. It finishes by checking contrast and phone width.

## Install

### Claude Code (plugin, recommended)

Inside Claude Code run:

```
/plugin marketplace add Kai411/calm-ui-design-system
/plugin install calm-ui-design-system@calm-ui
```

### Claude Code (manual)

Copy the skill folder into your skills directory:

```bash
git clone https://github.com/Kai411/calm-ui-design-system.git
mkdir -p ~/.claude/skills
cp -r calm-ui-design-system/skills/calm-ui-design-system ~/.claude/skills/
```

Use `.claude/skills/` inside a project instead of `~/.claude/skills/` to turn it on for that project only.

### Claude app and claude.ai

1. Download `calm-ui-design-system.zip` from the [Releases](../../releases) page, or zip the `skills/calm-ui-design-system` folder yourself.
2. Open **Settings → Capabilities → Skills** and choose **Upload skill**.
3. Pick the zip (don't unzip it).

## Use

Ask for UI work as usual, for example "build a dashboard page for my orders" or "restyle this settings screen". Claude offers the colour and font choices first. Say "you choose" to take the recommended ones.

## What's inside

```
skills/calm-ui-design-system/SKILL.md   the skill
.claude-plugin/plugin.json              plugin details for Claude Code
.claude-plugin/marketplace.json         lets /plugin marketplace add find it
```

## License

MIT
