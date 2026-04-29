# agent-starter

Soul's universal starter pack for spinning up multi-CLI agent infrastructure in any new project. Drops in clean. Initializes itself. Evolves with the project.

## What's in here

- `CONDUCTOR.md` — universal orchestration logic. Same in every project. Never edited per-project.
- `INIT.md` — initialization prompt. Run once on day one of a new project. Generates `PROJECT.md` by reading the codebase.
- `CLI_ROUTING.md` — which CLI runs which role (Claude / Codex / Gemini). Universal.
- `log.md` — empty append-only log. Universal scaffold.
- `specs/` — empty folder for work specs. Universal scaffold.

After running `INIT.md`, the project's `.agent/` folder also contains:

- `PROJECT.md` — project-specific brain. Generated on first run, edited as the project evolves. Lives only in the project repo, never returns to the starter pack.

## How to use it on a new project

From inside the new repo's root:

    mkdir -p .agent
    cp -r ~/agent-starter/* .agent/
    cp ~/agent-starter/.gitkeep .agent/specs/.gitkeep 2>/dev/null || true

Then run the initialization once:

    agent-deck launch . -t "init-<repo-name>" -c claude \
      -m "Read .agent/INIT.md and execute it. Produce .agent/PROJECT.md as the only output."

Claude reads INIT.md, surveys the codebase, writes PROJECT.md. Soul reviews and edits PROJECT.md before the first real Conductor cycle.

Then commit:

    git add .agent/
    git commit -m "Add agent infrastructure (starter pack + project init)"
    git push

## How to use it on a new machine

    git clone https://github.com/souldayan/agent-starter.git ~/agent-starter

That's it. The local copy is now mirrored from GitHub.

## How to evolve the starter pack

When you learn something that should apply to every future project — a new sequencing rule, a better CLI default, a sharper init prompt — update the file here in `agent-starter` and push:

    cd ~/agent-starter
    # edit
    git add .
    git commit -m "what changed and why"
    git push

Older projects keep their version. New projects pick up the update on next copy. That's the right behavior — last week's project should not mutate because you changed your mind today.

## Versions of record

- claude --version   2.1.x
- codex  --version   0.125.x
- gemini --version   0.40.x
- agent-deck         (verify with `agent-deck --version`)
