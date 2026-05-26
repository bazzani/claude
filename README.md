# My Claude Configuration

Version-controlled copy of the parts of `~/.claude` that are worth keeping
across machines — global `CLAUDE.md`, `settings.json`, and the custom
`agents/`, `commands/`, `skills/`, and `hooks/` directories. Runtime state
(sessions, caches, history, etc.) is deliberately excluded — see `.gitignore`.

## Layout

| Path            | What it is                                         |
| --------------- | -------------------------------------------------- |
| `CLAUDE.md`     | Global user instructions (voice, commit style, …) |
| `settings.json` | Global Claude Code settings                        |
| `agents/`       | Custom subagent definitions                        |
| `commands/`     | Custom slash commands                              |
| `skills/`       | Custom skills                                      |
| `hooks/`        | Hook scripts triggered by Claude Code events       |

## Setup on a new machine

1. Install [Claude Code](https://docs.claude.com/claude-code) and run it once
   so `~/.claude` is created.

2. Clone this repo somewhere outside `~/.claude`:

   ```sh
   git clone <repo-url> ~/dev/projects/claude
   ```

3. Back up anything already in `~/.claude` that would collide, then remove
   the originals:

   ```sh
   cd ~/.claude
   for p in CLAUDE.md settings.json agents commands skills hooks; do
     [ -e "$p" ] && mv "$p" "$p.bak"
   done
   ```

4. Symlink the repo contents into `~/.claude`:

   ```sh
   cd ~/.claude
   REPO=~/dev/projects/claude
   ln -s "$REPO/CLAUDE.md"     CLAUDE.md
   ln -s "$REPO/settings.json" settings.json
   ln -s "$REPO/agents"        agents
   ln -s "$REPO/commands"      commands
   ln -s "$REPO/skills"        skills
   ln -s "$REPO/hooks"         hooks
   ```

5. Verify:

   ```sh
   ls -l ~/.claude | grep '^l'
   ```

   You should see six symlinks pointing into the repo.

6. Once you're happy the symlinks work, delete the `.bak` files from step 3.

## Editing

Edit files from either path — `~/.claude/CLAUDE.md` and
`~/dev/projects/claude/CLAUDE.md` are the same file. Commit from the repo.

## What is *not* tracked

`.gitignore` excludes machine-local state that lives under `~/.claude` but
shouldn't be shared between machines: conversation history, session/project
state, caches, telemetry, plugin marketplace payloads, and local-only
settings overlays (`settings.local.json`).
