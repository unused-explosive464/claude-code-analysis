---
description: "Autonomous self-knowledge skill. Fetches Claude Code's own reverse-engineered architectural docs from GitHub to understand internal subsystems — compaction, permissions, agent loops, tool execution, security, prompt engineering — and applies that knowledge to improve behavior in real-time. TRIGGER when: context is growing long, tool calls are blocked or failing, spawning sub-agents, writing complex bash, optimizing prompts, debugging rendering, or whenever deeper understanding of Claude Code internals would improve task execution."
user-invocable: true
argument-hint: "[topic or subsystem, e.g. compaction, permissions, bash-parser, agents, prompts]"
---

# Claude Code Internals — Self-Knowledge Skill

You are Claude Code. This skill gives you access to a complete reverse-engineering of your own architecture — 82 documents, 112K+ lines, extracted from 512,000 lines of your actual TypeScript source (v2.1.88).

Source repo: https://github.com/thtskaran/claude-code-analysis

## How This Works

All documents are on GitHub. Fetch them on-demand via WebFetch using this base URL:

```
https://raw.githubusercontent.com/thtskaran/claude-code-analysis/master/
```

Append any file path from the manifest below. Read the document. Extract what's relevant. Apply it.

## Autonomous Self-Improvement Loop

When this skill is active, follow this loop:

1. **Identify friction** — What subsystem is involved? Context pressure? Permission block? Tool failure? Agent coordination? Prompt quality?
2. **Pick the document** — Use the manifest below. If unsure, start with `02-master-extraction/KNOWLEDGE_BASE.md` — it indexes everything.
3. **Fetch it** — Use WebFetch on `https://raw.githubusercontent.com/thtskaran/claude-code-analysis/master/{path}`
4. **Extract the mechanism** — Find the specific threshold, pattern, fallback, or architecture relevant to your situation.
5. **Apply it** — Adjust your approach based on what you learned.
6. **Chain if needed** — Documents reference each other. Follow the chain until you have what you need.

If invoked with `$ARGUMENTS`, treat that as the topic to look up and fetch the most relevant document(s) for it.

## When To Activate (Trigger Map)

| You notice this... | Fetch this document |
|---------------------|---------------------|
| Context getting long, messages being compressed | `04-systems/compaction-algorithms-deep-dive.md` |
| Tool calls getting denied or blocked | `05-security/permissions-yolo-deep-dive.md` |
| Bash commands getting flagged | `05-security/bash-parser-deep-dive.md` |
| Need to coordinate sub-agents | `07-agents/swarm-orchestration-deep-dive.md` |
| API calls failing, retries, streaming issues | `04-systems/api-client-deep-dive.md` |
| Writing or optimizing prompts/instructions | `03-prompts/prompt-bible.md` |
| Understanding tool contracts and schemas | `06-tools-and-plugins/tool-contract-inventory.md` |
| Model behavior expectations | `03-prompts/model-behavioral-contract.md` |
| Plugin or MCP integration | `06-tools-and-plugins/plugin-system-deep-dive.md` |
| Memory system behavior | `04-systems/memdir-semantic-memory-deep-dive.md` |
| Feature flags and gating | `04-systems/feature-flag-encyclopedia.md` |
| Security review or audit | `05-security/deep-security-audit.md` |
| Need the full architectural picture | `01-architecture/codebase-structure.md` |
| Don't know where to start | `02-master-extraction/KNOWLEDGE_BASE.md` |

## Document Manifest

### Architecture — `01-architecture/`
- `codebase-structure.md` — Full module map. 38 services, dependency graph.
- `bootstrap-entry-point.md` — 10-step boot, 4 entry points, 10 startup modes.
- `component-architecture-deep-dive.md` — 389 React components mapped.
- `hooks-system-deep-dive.md` — All 104 custom hooks with signatures.
- `ink-rendering-engine-deep-dive.md` — 6-stage render pipeline, Int32Array cell packing, frame diffing.
- `query-engine-deep-dive.md` — Async generator agent loop, streaming, mid-stream tool injection.
- `build-system-deep-dive.md` — Bun bundler, 88 feature flags, dead code elimination.
- `data-flow-and-coupling.md` — Module coupling, data flow paths.
- `state-migrations-output-deep-dive.md` — State management, migration patterns.
- `native-ts-reimplementations-deep-dive.md` — Custom implementations replacing npm packages.
- `terminal-ui-deep-dive.md` — Terminal UI internals.

### Extracted Knowledge — `02-master-extraction/`
- `KNOWLEDGE_BASE.md` — Master index. Start here if unsure. Every subsystem cross-referenced.
- `deep-extraction.md` — 2,251 discrete findings across all subsystems.
- `claude-code-rnd-extraction.md` — Master synthesis, 19 appendices (A-S).
- `code-archaeology.md` — Historical patterns, dead code, evolution traces.

### Prompts & Behavior — `03-prompts/`
- `prompt-bible.md` — Full 32K-line system prompt. 7 static + 13 dynamic sections. 20 prompt engineering techniques.
- `model-behavioral-contract.md` — 24+ behavioral invariants the code expects from the model.

### Core Runtime — `04-systems/`
- `api-client-deep-dive.md` — 5-provider streaming pipeline, retry with exponential backoff + jitter.
- `compaction-algorithms-deep-dive.md` — 3-tier compaction. 93% trigger. 9-section summary template. Circuit breakers.
- `model-selection-deep-dive.md` — 11 models, 5-tier fallback chain, capability matrix.
- `memdir-semantic-memory-deep-dive.md` — 7 interconnected memory subsystems.
- `memory-extraction-pipeline-deep-dive.md` — How memories are extracted and stored.
- `settings-policy-infrastructure-deep-dive.md` — 5-scope priority merging for settings.
- `shell-entrypoints-proxy-deep-dive.md` — Shell management, proxy, entrypoints.
- `lsp-integration-deep-dive.md` — Language server protocol integration.
- `team-memory-sync-deep-dive.md` — Cross-agent memory synchronization.
- `prompt-suggestion-deep-dive.md` — Suggestion engine internals.
- `suggestions-and-secure-storage-deep-dive.md` — Credential storage.
- `telemetry-event-catalog.md` — 200+ event types, sampling rates.
- `feature-flag-encyclopedia.md` — 88 build-time + 600+ runtime flags.
- `growthbook-gates-inventory.md` — A/B testing gates.
- `constants-encyclopedia.md` — All constants and magic numbers.
- `environment-variable-contract.md` — Every env var contract.

### Security — `05-security/`
- `permissions-yolo-deep-dive.md` — 2-stage YOLO classifier (64-token fast pass, 4096-token reasoning). 6 permission modes. 7-stage pipeline.
- `bash-parser-deep-dive.md` — Hand-rolled recursive descent. 15 dangerous AST types. 35+ blocked builtins.
- `deep-security-audit.md` — Cross-system vulnerability analysis.
- `oauth-lifecycle-deep-dive.md` — PKCE, token lifecycle.
- `terminal-ui-security.md` — UI-layer security.
- `team-memory-security.md` — Multi-agent security boundaries.
- `desanitization-map.md` — Sanitization application/removal points.
- `security-audit.md` — Additional security findings.

### Tools & Plugins — `06-tools-and-plugins/`
- `command-system-deep-dive.md` — 189 commands, 3 dispatch types.
- `plugin-system-deep-dive.md` — 6-phase lifecycle, DFS dependency resolution, homograph detection.
- `tool-contract-inventory.md` — Every tool's input/output contract.
- `computer-use-deep-dive.md` — Screen capture, input handling.
- `claude-in-chrome-deep-dive.md` — Chrome extension integration.
- `bundled-skills-catalog.md` — Built-in skill definitions.
- `buddy-system-deep-dive.md` — Pair programming system.
- `keybindings-system.md` — Keyboard shortcuts.
- `command-tool-skill-surface.md` — Full command/tool/skill surface area.
- `utility-subsystems-deep-dive.md` — Utility modules.
- `small-services-deep-dive.md` — Smaller services.
- `unmapped-systems.md` — Not yet fully documented.
- `plugin-marketplace-contract.md` — Marketplace contracts.
- `computer-use-protocols.md` — Computer use protocols.

### Agents — `07-agents/`
- `swarm-orchestration-deep-dive.md` — Leader-follower, file-based mailbox IPC, 3 execution backends.
- `session-teleport-deep-dive.md` — Cross-environment state preservation.
- `coordinator-voice-moreright-deep-dive.md` — Team coordination, voice pipeline.
- `swarm-bridge-sdk.md` — Swarm communication SDK.
- `remote-bridge-protocol.md` — Remote session protocol.
- `daemon-proactive-agent.md` — Background/proactive agent.

### History — `08-history/`
- `migration-product-history.md` — 11 shipped migrations, permission evolution.
- `community-osint.md` — External research findings.

### Diagrams — `infographics/`
SVGs you can fetch for visual architecture maps:
- `architecture-overview.svg` — Full system
- `bootstrap-sequence.svg` — 10-step startup
- `ink-rendering-pipeline.svg` — 6-stage rendering
- `api-client-pipeline.svg` — API request flow
- `memory-systems.svg` — 7 memory subsystems
- `compaction-state-machine.svg` / `compaction-tiers.svg` — Compaction
- `permission-engine.svg` / `yolo-classifier-pipeline.svg` — Permissions
- `command-dispatch.svg` — Command routing
- `plugin-lifecycle.svg` — Plugin lifecycle
- `streaming-tool-executor.svg` — Tool execution
- `swarm-orchestration.svg` / `agent-loop-flow.svg` — Multi-agent
- `system-prompt-ordering.svg` — Prompt sections

## Key Constants (No Fetch Needed)

| What | Value |
|------|-------|
| Compaction trigger | 93% context utilization |
| YOLO Stage 1 | 64 tokens max |
| YOLO Stage 2 | 4,096 tokens, T=0 |
| Token escalation | 8K -> 16K -> 32K -> 64K on truncation |
| Terminal rendering | 100ms frame interval (10 FPS) |
| Swarm mailbox poll | 1000ms |
| Compaction circuit breaker | 3 consecutive failures |
| Permission denial escalation | 3 consecutive or 20 total |
| Prompt cache savings | ~76% cost reduction |
| Microcompact tool expiry | 60 min |
