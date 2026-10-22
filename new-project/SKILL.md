---
name: new-project
description: Use when the user types "NEW PROJECT" (any casing, optionally "quick"), or says they want to start, kick off, set up, or re-plan a project, app, product, channel or idea, or asks to update or adopt an existing project's playbook.
---

# New Project Playbook

## Overview
Interview first, build second. The answers become a playbook that any future session can read cold, so the user never re-explains the project. Be concrete: real names, numbers, screens, files. No abstract slogans. Use the vault, agent memory and task apps when they exist; work fine without them.

## Step 0: Detect and pick a mode
Run before asking anything.

| Check | If present | If absent |
|---|---|---|
| Vault: an Obsidian vault path named in the user's CLAUDE.md or memory, with an agent guide such as `<vault>/.agent/README.md` (ask once if none is known) | Read the README, list `Projects/`, read any note matching the project name | Skip vault steps |
| Agent memory: `memory_recall` / `memory_smart_search` tools, or `curl -s localhost:3111/agentmemory/livez` answers | Recall the project name and topic, plus user preferences and past decisions (read-only) | Skip memory steps |
| Tracking tools: Todoist, Superlist, Notion, Linear, GitHub (`gh auth status`) or any other tracker connector | Offer as the default answer to the tracking question | Still ask; the user may use a system with no connector |
| Folder: `PLAYBOOK.md` / `CLAUDE.md` in the project folder | Read them | None |

Tell the user in one line what was found. Then choose the mode:

| Situation | Mode |
|---|---|
| Nothing exists for this project | **Full** (3 rounds) |
| User typed `NEW PROJECT quick`, or says it is a small idea | **Quick** (5 questions) |
| A vault note, memory or playbook already exists | **Update** |

## Update mode (project already exists)
1. Map the existing note, memory and folder onto the playbook sections. Old notes often hold the content under other headings; reuse it, don't re-ask it.
2. List which sections are missing or stale (blocked, next, v1 checks, out of scope are the usual gaps).
3. Ask only about those gaps, max 4 questions per round.
4. Show a diff-style proposal: sections to add, lines to change. Never delete or rewrite the user's existing text, and never move or rename the note.

## Step 1: Pre-fill
Memory search often returns raw command logs rather than curated memories; keep only results that state a fact, preference or decision, and prefer the vault note when they disagree. Use those and vault notes to answer questions yourself (preferred stack, working style, related projects, past decisions). Show them as "Assuming X, correct me", not as questions. Skip anything the user's message or the folder already answers.

## Step 2: Pick a project type
Ask first (one choice question): **App/software · Content/video · Research · Business/service**. Add its extras to Round 2.

| Type | Extra questions |
|---|---|
| App/software | Data it stores; auth and users; platforms (web/iOS/API); deploy target; what runs where |
| Content/video | Platform(s) and cadence; format and length; face or faceless; asset sources; success metric (views, signups, sales) |
| Research | The decision it feeds; sources allowed; output format; deadline |
| Business/service | Offer and price; first 10 customers and how to reach them; channels; legal or platform rules to respect |

## Step 3: Interview
Use AskUserQuestion for choice questions with a recommended default; plain text for open ones. Max 4 per round.

**Round 1, what and why:** name and one-sentence description; who uses it and what they do today without it; 3 observable checks for "v1 is done" (e.g. "I upload a photo and get a priced listing in under 10 seconds").

**Round 2, scope and constraints:** must-haves (max 5) and what is NOT in v1; stack, repo or path, hosting, deadline, budget; external services and accounts (record *where* keys live, never the key); risks, unknowns, blockers; plus the type extras.

**Round 3, how to work together:** autonomy (ask each step / decide small things / run until blocked); what must always be confirmed first (spending, deploying, deleting, messaging third parties); first milestone and this session's goal; **tracking** (below).

**Tracking question (always asked, in Full, Quick and Update mode):** "Do you track projects or tasks anywhere? (Todoist, Superlist, Notion, Linear, GitHub Issues, Trello, Obsidian checkboxes, a paper list, none, other)". Multi-select, plus "other" for any system. Then ask only what is needed:
- Where in it: project, list, board or database name, and whether it already exists or should be created.
- What goes in: milestones only, or every task; who it is assigned to; how due dates are set.
- Who keeps it current: the agent after each ship/block/decide event, or only on request.
Detected tools (Step 0) become the recommended default. Record the answers under `Working agreement` as `Tracking: <system> / <location> / <what goes in> / <who updates>`; "none" is a valid answer and skips Step 5 tasks.

**Quick mode asks only:** name + one-liner, who uses it, 3 v1 checks, stack or tool, first action, tracking system. Mark every other section "unknown" and offer to run Full later.

## Step 4: Draft and approve
Sections in this order (write "unknown" rather than guessing):
`Summary` · `Users & problem` · `v1 done when` · `In scope` · `Out of scope` · `Stack & paths` · `Integrations & access` · `Risks & blockers` · `Working agreement` · `Milestones` · `Decisions log` · `Recent` · `Open`

Show the draft and wait for a yes before writing any file.

## Step 5: Write, by what exists
| Target | Condition | Content |
|---|---|---|
| Folder `PLAYBOOK.md` | Always (stub when vault exists) | Full playbook, or a pointer to the vault note |
| Folder `CLAUDE.md` | Always; append, never overwrite | Summary, v1 checks, stack, working agreement, pointer |
| Vault `Projects/<Name>.md` | Vault found | Frontmatter `type: project, status, next, blocked, repo, path, url`, then the playbook; follow the vault's agent guide if it has one |
| agentmemory | Memory found | `memory_save` (or the `remember` skill): decisions and working agreement, plus a `project` memory with name, path and v1 checks. Then run the vault's memory export script, if it has one (e.g. `<vault>/.agent/export_memory.py`) |
| Tracker | User named a system in the tracking question | Turn Milestones (and tasks, if chosen) into items for that system. Show the list, create only the approved ones, in one batch call where the tool allows. See the table below |

**Tracker by system:**
| System | How |
|---|---|
| Todoist, Superlist, Notion | Their connector tools (find the project or list first; reuse existing labels) |
| GitHub Issues | `gh issue create` per item, milestones via `gh api` |
| Linear, Trello, Jira, Asana, any other with a connector | Use the connector if listed; otherwise treat as "no connector" |
| Obsidian checkboxes | `- [ ]` items under `## Open` in the vault note |
| No connector, paper, or "other" | Write a paste-ready checklist (markdown, plus CSV if the system imports it) to `PLAYBOOK.md` and show it; say clearly that nothing was created in the tool |

Never claim items were created in a system the agent could not reach.

No secrets in any file, note, memory or task.

## Step 6: Hand over
End with the first milestone and one next action. Build only if the user says go. Later ship/push/block/decide events update the vault note's `status`, `next`, `blocked` and `## Recent`, and, if the working agreement says the agent keeps the tracker current, complete or add the matching items there too.

## Common mistakes
| Mistake | Fix |
|---|---|
| Full interview on a project that already has a note | Update mode: ask only about gaps |
| Re-asking what memory or the vault already says | Pre-fill and confirm |
| Failing when vault, memory or task app is missing | Detect first, fall back to folder files |
| Duplicating the playbook in folder and vault | Vault holds it; folder keeps a stub |
| Overwriting existing notes or `CLAUDE.md` | Append or propose a diff |
| Vague answers ("make it good") | Ask for an observable check |
| Assuming one tracker, or skipping the question because no connector is detected | Always ask; fall back to a paste-ready checklist |
| Writing files or creating tasks before approval | Show the draft, wait |
| Coding straight after the interview | Stop at the first milestone unless told to go |
