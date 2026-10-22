<div align="center">

# 🚀 new-project

### Type `NEW PROJECT` and your agent interviews you, then writes the playbook.

A [Claude Code](https://claude.com/claude-code) skill that turns "I have an idea" into a written, resumable project playbook, so no future session ever starts from zero.

![Claude Code](https://img.shields.io/badge/Claude%20Code-skill-D97757?style=for-the-badge)
![License](https://img.shields.io/badge/license-MIT-green?style=for-the-badge)
![Obsidian](https://img.shields.io/badge/Obsidian-optional-7C3AED?style=for-the-badge)
![Memory](https://img.shields.io/badge/agentmemory-optional-0EA5E9?style=for-the-badge)

</div>

---

## ✨ What it does

Most projects start with a vague prompt and a lot of re-explaining. This skill front-loads the thinking:

- 🎤 **Interviews you** in short rounds (max 4 questions each), skipping anything it already knows
- 📋 **Drafts a playbook** with 14 fixed sections, from `Summary` to `Open`
- ✅ **Waits for your yes** before it writes a single file
- 🔁 **Remembers the project** so the next session reads the playbook instead of asking again

## 🧭 Modes

| Mode | Trigger | What happens |
|---|---|---|
| 🆕 **Full** | `NEW PROJECT` | 3 interview rounds: what & why, scope & constraints, how we work together |
| ⚡ **Quick** | `NEW PROJECT quick` | 6 questions for small ideas, everything else marked `unknown` |
| 🔄 **Update** | A note, memory or playbook already exists | Maps what exists onto the template, asks only about the gaps, proposes a diff and never overwrites your text |

## 🧩 Project types

The skill asks what you are building and adds questions for it:

| Type | Extra questions |
|---|---|
| 💻 App / software | Data stored, auth, platforms, deploy target |
| 🎬 Content / video | Platforms, cadence, format, face or faceless, success metric |
| 🔬 Research | The decision it feeds, allowed sources, output format |
| 💼 Business / service | Offer and price, first 10 customers, channels, platform rules |

## 🔌 Works with what you have

Everything below is **optional**. The skill detects each one first and falls back gracefully.

### 📓 Obsidian vault
Reads existing `Projects/<Name>.md` notes, writes the playbook there with `status / next / blocked` frontmatter, and keeps the folder's `PLAYBOOK.md` as a short pointer so there is one source of truth. Never moves, renames or deletes your notes.

### 🧠 agentmemory
Recalls your preferences and past decisions to pre-fill answers ("Assuming X, correct me"), then saves the decisions and working agreement for later sessions.

### ✔️ Any project tracker
Every mode asks: *"Do you track projects or tasks anywhere?"*

| System | How it is handled |
|---|---|
| Todoist, Superlist, Notion | Connector tools |
| GitHub Issues | `gh issue create` |
| Linear, Trello, Jira, Asana | Connector if available |
| Obsidian checkboxes | `- [ ]` items in the vault note |
| Anything else, or paper | Paste-ready markdown/CSV checklist, and it tells you nothing was created |

The answer is stored as `Tracking: <system> / <location> / <what goes in> / <who updates>`. The agent never claims it created items in a tool it could not reach.

## 🗂️ The playbook template

`Summary` · `Users & problem` · `v1 done when` · `In scope` · `Out of scope` · `Stack & paths` · `Integrations & access` · `Risks & blockers` · `Working agreement` · `Milestones` · `Decisions log` · `Tracker items` · `Recent` · `Open`

Unknown answers are written as `unknown`, never guessed.

## 📦 Install

```bash
npx skills add madeinkenzo/new-project-skill
```

<details>
<summary>Manual install</summary>

```bash
git clone https://github.com/madeinkenzo/new-project-skill.git
cp -R new-project-skill/new-project ~/.claude/skills/new-project
```

</details>

Start a new Claude Code session and type:

```
NEW PROJECT
```

## 🛡️ Ground rules baked in

- 🔒 No secrets in any file, note, memory or task. It records *where* a key lives, never the key
- 🧱 Concrete answers only. Vague goals like "make it good" get turned into an observable check
- 🙋 Asks before spending money, deploying, deleting, or messaging third parties, if you say so in the working agreement
- 🛑 Stops at the first milestone. It builds only when you say go

## 🛠️ Customize

Everything lives in one file, [`new-project/SKILL.md`](new-project/SKILL.md). Edit the question rounds, the project-type table, or the playbook sections to match how you work.

## 📄 License

MIT. See [LICENSE](LICENSE).
