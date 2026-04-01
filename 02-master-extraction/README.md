# Master Extraction Documents

Primary extraction outputs from systematic codebase analysis. These documents form the foundational layer that seeded all subsequent deep-dive analyses. They represent the exhaustive, semantic-level inventory of Claude Code's structure, patterns, and behaviors extracted through automated and manual code archaeology.

## Document Inventory

| Document | Description | Lines |
|----------|-------------|-------|
| **claude-code-rnd-extraction.md** | Master synthesis document with 19 appendices (A-S) covering architecture, prompts, systems, security, and behavioral specifications. Core reference for understanding codebase organization. | 2,564 |
| **deep-extraction.md** | Semantic-level extraction capturing 2,251 discrete findings across types, patterns, protocols, and behaviors. Fine-grained inventory of implemented features and architectural choices. | 17,640 |
| **KNOWLEDGE_BASE.md** | Consolidated knowledge base aggregating key findings, definitions, and relationships across all extraction domains. Reference format for quick lookups. | 1,777 |
| **deep-dive-report.md** | Initial deep-dive findings establishing baseline understanding before specialized deep-dives. Includes summary statistics and key metrics. | 1,371 |
| **analysis-report.md** | Formal analysis report documenting methodology, findings, and conclusions from the extraction process. | 1,478 |
| **code-archaeology.md** | Historical code patterns and evolution traces. Documents legacy patterns, refactoring history, and technical debt markers. | 133 |

## Extraction Scope

The master extraction documents catalog the following dimensions:

### Code Inventory
- **6,552 exported functions** identified across the codebase
- **99 classes** mapped with inheritance hierarchies
- **1,308 type definitions** including Zod schemas and TypeScript interfaces
- **327 Zod schemas** extracted and categorized by domain

### Feature & Configuration
- **90 feature flags** cataloged with conditional code paths
- **88 build-time flags** eliminating dead code paths during bundling
- **47 runtime gates** (GrowthBook integration points)
- **156 environment variables** with runtime contracts

### Behavioral Specifications
- **19 major subsystem categories** with behavioral documentation
- **230+ custom hooks** with lifecycle and dependency patterns
- **389 React components** with component responsibility mapping
- **18 telemetry event categories** with sampling rate definitions

### Security & Systems
- **Permission model** (6-layer architecture)
- **Credential management** patterns and secure storage
- **API client** streaming pipeline with provider abstraction
- **Memory systems** (7 interconnected subsystems)
- **Shell integration** with 4 entry point variants

## Key Findings

### Architectural Patterns
- **Async generator architecture** for streaming LLM responses and tool execution
- **Custom Zustand-like state management** with `useSyncExternalStore` hook
- **Terminal UI via Ink.js wrapper** with native TypeScript reimplementation of core reconciler
- **Modular API client** supporting 5 cloud providers (Anthropic, Bedrock, Vertex, Foundry, Azure)

### Code Metrics
- **Total codebase**: ~81,546 LOC in components + hooks alone
- **Component layer**: 389 components across 81,546 LOC
- **Hooks layer**: 104 custom hooks across 19,204 LOC
- **Rendering pipeline**: 6 stages from React tree to terminal output

### Configuration Complexity
- **Conditional compilation**: 88 feature flags with dead code elimination at build time
- **Runtime gating**: GrowthBook integration for A/B testing and feature rollout
- **Provider abstraction**: Single API client serving 5 distinct provider integrations
- **Environment contracts**: 156 env vars with validation schemas

### System Integration
- **LLM streaming**: Bidirectional streaming with tool execution loop
- **Shell proxying**: CONNECT protocol over WebSocket with 4 shell detection strategies
- **Semantic memory**: 7 memory subsystems (persistent, transient, team, extraction, etc.)
- **Telemetry**: 200+ event types with structured context attachment

### Extraction Methodology
The master extraction employed:
1. AST parsing for structural inventory (exports, classes, types)
2. Regex pattern analysis for feature flags and configuration
3. Semantic code reading for behavioral documentation
4. Dependency tracing for coupling metrics
5. Historical analysis for architectural evolution

These master documents serve as the authoritative single source of truth for subsequent analyses, with all section-specific deep-dives referencing extracted inventories.

