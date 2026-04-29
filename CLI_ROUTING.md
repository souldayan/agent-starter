# CLI Routing — Multi-Vendor Dev Environment

This file describes which CLI to use for which dev task. It does not
override CONDUCTOR.md — the Conductor still routes to builder/auditor/
qa/ai-lead. This is about which CLI those roles run under.

## Default vendor per role
- builder    → Claude Code  (strongest tool use, knows CLAUDE.md)
- auditor    → Codex        (different vendor = different blind spots)
- qa         → Claude Code  (test discipline, runtime checks)
- ai-lead    → Gemini       (1M context for whole-repo reasoning)

## When to override the default
- Builder stuck for 2+ cycles → swap to Codex for one attempt.
- Auditor and Builder agree too easily → add Gemini as third reviewer.
- Spec touches > 20 files → ai-lead (Gemini) maps blast radius first.

## Launch syntax (agent-deck)
  agent-deck launch . -t "<role>-<slug>" -c <claude|codex|gemini> \
    -m "<message>"

## Versions of record (verify before each session)
- claude --version   (expect: 2.1.x)
- codex  --version   (expect: 0.125.x)
- gemini --version   (expect: 0.40.x)
