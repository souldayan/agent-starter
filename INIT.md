# Initialization Prompt

Run this ONCE per new project, on the first day the agent-starter lands in the repo. It produces `.agent/PROJECT.md` — the project-specific brain that the Conductor reads alongside `CONDUCTOR.md` from this point forward.

## How to run it

From inside the new repo, with the starter pack already copied to `.agent/`:

```
agent-deck launch . -t "init-<repo-name>" -c claude -m "Read .agent/INIT.md and execute it. Produce .agent/PROJECT.md as the only output."
```

Claude reads this file, then reads the codebase, then writes `.agent/PROJECT.md`.

## What Claude is asked to produce

A single file at `.agent/PROJECT.md` with the following sections, populated from what Claude actually finds in the codebase. If a section has no real content, Claude writes "None observed" rather than inventing material.

---

## INSTRUCTIONS TO CLAUDE — execute everything below

You are initializing the agent infrastructure for a project you have never seen before. Your job is to produce `.agent/PROJECT.md` — a single markdown file that captures what is project-specific about this codebase, so future agent cycles can read it instead of re-discovering it every time.

### Step 1 — Survey

Read in this order:
1. `README.md` (root level) if present
2. `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, or whatever manifest exists — to identify language and dependencies
3. The top-level folder structure (one level deep only)
4. Any `CLAUDE.md`, `AGENTS.md`, `GEMINI.md` already present at root
5. Any `docs/` folder — list files but do not read them all unless small

Do NOT read every source file. Do NOT run tests. This is reconnaissance, not analysis.

### Step 2 — Decide what is project-specific

For each candidate fact, ask: would this be true of every project Soul ever builds, or only this one? If universal, it does not belong in `PROJECT.md` — that is what `CONDUCTOR.md` is for. If project-specific, it goes in.

Examples of project-specific facts:
- "This is a TypeScript Next.js app" → yes, project-specific
- "Spec before build" → no, that is universal (already in CONDUCTOR.md)
- "Memory writes go through the Reflector class in `lib/reflector.ts`" → yes, project-specific
- "Don't touch `migrations/` without Soul's approval" → yes, project-specific
- "Always run tests before merging" → no, that is universal

### Step 3 — Write `.agent/PROJECT.md`

Create the file with these sections, in this order. Fill each from what you observed. Use "None observed" for empty sections. Be terse — this file gets read often, by both you and Soul.

```markdown
# PROJECT — <repo name>

Project-specific brain for this codebase. Read alongside CONDUCTOR.md.

## What this project is
<2-3 sentences. What does this codebase do, in plain English?>

## Stack
- Language(s):
- Framework(s):
- Key dependencies (top 5 only, by importance not alphabetic):
- Test runner:
- Type checker / linter:

## Architecture (one paragraph)
<How is the code organized? What are the major modules? Where does data flow?>

## Project-specific rules
<Patterns, conventions, or rules that are NOT universal. Examples: a memory-write pattern, an enforcement model, a layering rule, a naming convention. If the codebase has its own CLAUDE.md, mine it. If not, infer from structure.>

## No-touch zones
<Files, folders, or systems where any change requires Soul's explicit approval. Examples: production data paths, auth/permission code, generated files, migrations. Conductor must escalate before dispatching work that touches these.>

## Existing docs to consult
<List any docs/ files relevant to architecture, decisions, or build history. One line each. Do not summarize them — just list paths.>

## Quick commands
<The 3-5 commands a builder will need most often. Test, lint, type-check, dev server, build. Pull from package.json scripts or equivalent.>

## Open questions for Soul
<Anything you noticed during initialization that you couldn't classify confidently. Bullet list. Soul will resolve these on first review.>
```

### Step 4 — Stop

Once `.agent/PROJECT.md` is written, stop. Do not modify any other files. Do not commit. Do not start work on any spec. Soul reviews `PROJECT.md` first, edits as needed, and only then begins the normal Conductor loop.

If anything in this initialization felt like guesswork rather than observation, list it under "Open questions for Soul" rather than fabricating an answer.
