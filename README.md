# agent-starter

Soul's starter pack for spinning up multi-CLI agent infrastructure in any new repo.

## What this is

Four files that describe how Claude Code, Codex, and Gemini work together inside a repo, orchestrated by Agent Deck. Drop them into any new project's `.agent/` folder and you have a working setup.

- `CONDUCTOR.md` — the org-level orchestration logic (builder / auditor / qa / ai-lead, sequencing rules, compound checkpoint)
- `CLI_ROUTING.md` — which vendor CLI runs which role
- `log.md` — empty file, append cycle entries here
- `specs/` — empty folder, one file per work spec

## How to use it in a new repo

From inside the new repo's root:

    mkdir -p .agent
    cp -r ~/agent-starter/* .agent/
    cp -r ~/agent-starter/.gitkeep .agent/specs/ 2>/dev/null || true

Then open `.agent/CONDUCTOR.md` and replace `<REPO_PATH>` with the new repo's actual local path.

Commit:

    git add .agent/
    git commit -m "Add agent-deck conductor and CLI routing"
    git push

## Updating the starter pack

When you learn something that should apply to every future project (a new sequencing rule, a better CLI routing default, a better escalation policy), update the file here in `agent-starter` and push to GitHub. Older repos keep their version unless you intentionally re-copy.

## Versions of record

- claude --version   2.1.x
- codex  --version   0.125.x
- gemini --version   0.40.x
- agent-deck         (verify with `agent-deck --version`)
