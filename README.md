# Claude Code Reverse Engineering

A complete reverse-engineering and R&D extraction of Anthropic's Claude Code codebase. This repository contains 1,884 TypeScript source files analyzed and documented, revealing the architecture, systems, and inner workings of one of the most sophisticated AI-assisted coding platforms.

## Overview

Claude Code is a complex, production-grade system. This extraction documents its internal design through rigorous static analysis, AST inspection, and systematic reverse engineering of source code. The result is a detailed technical knowledge base suitable for researchers, engineers, and anyone interested in understanding how modern AI coding assistants are built.

On March 31, 2026, Anthropic's Claude Code client-side source was inadvertently exposed through a publicly accessible `.map` file shipped with the production npm package. This repository is the result of analyzing that leaked source. It contains no redistributed code — only original findings, observations, and documentation written by the author.

The analysis was performed by **Karan Prasad**.

### Contact

- Email: hello@karanprasad.com
- Blog: https://karanprasad.com

## Key Discoveries

The extraction uncovered 10 critical architectural patterns that define how Claude Code operates:

1. **Async Generator Agent Loop** — The core `async function* query()` implements a streaming-first architecture, allowing real-time response generation and tool execution during user interaction rather than waiting for completion.

2. **Three-Tier Context Compaction** — Context memory is managed across three tiers: Microcompact (no API calls, ~256 tokens), Full (API evaluation, ~4K tokens), and Session Memory (zero-cost in-memory cache), enabling efficient token usage across different response lengths.

3. **Two-Stage YOLO Classifier** — An automated permission system that first uses a 64-token classifier to make instant decisions, then escalates to a 4,096-token "thinking" classifier for edge cases, eliminating unnecessary permission prompts.

4. **NO_TOOLS Sandwich Pattern** — Security-critical instruction appears at the START of the system prompt and again at the END with explicit rejection consequences, ensuring tool-calling restrictions are enforced even under adversarial conditions.

5. **Prompt Cache Boundary** — A `__SYSTEM_PROMPT_DYNAMIC_BOUNDARY__` marker splits the system prompt into a global (cached) portion and a per-session (uncached) portion, dramatically reducing API latency.

6. **Copy-on-Write Speculation** — The system pre-computes the next user response on an overlay filesystem, allowing near-instant switching when users navigate between sessions or make rapid back-to-back requests.

7. **Streaming Tool Execution** — Tools execute *during* the model's streaming response generation, not after, allowing dependent operations to begin immediately rather than waiting for full completion.

8. **Bash Parser Security** — A 4,437-line Bash parser implements fail-closed security with 15 dangerous AST node types explicitly blocked, preventing shell injection and command tampering.

9. **Frustration Detection** — Regex patterns detect profanity and emotional signals in user input, triggering telemetry, feedback surveys, and automatic issue filing for product improvement.

10. **Coordinator Mode** — A multi-agent orchestration layer synthesizes results from parallel agent threads, enabling complex workflows and coordinated reasoning across multiple sub-agents.

## Repository Structure

```
.
├── README.md                        # You are here
├── METHODOLOGY.md                   # Extraction techniques and tools used
├── 01-architecture/                 # Core architecture patterns
│   ├── codebase-structure.md        # Module layout, service directory (38 services)
│   ├── ast-analysis.md              # 1,884 files, 6,552 exports, 99 classes, 1,308 types
│   └── graph-and-complexity.md      # Dependency graph, coupling metrics, top-50 complex functions
├── 02-master-extraction/            # Primary extraction documents
│   ├── claude-code-rnd-extraction.md  # THE master doc (2,564 lines, 19 appendices A-S)
│   ├── deep-extraction.md           # 2,251 findings from programmatic analysis
│   ├── deep-dive-report.md          # Detailed technical deep-dive
│   ├── analysis-report.md           # High-level analysis findings
│   └── KNOWLEDGE_BASE.md            # Consolidated knowledge compilation
├── 03-prompts/                      # Prompt engineering findings
│   ├── prompt-bible.md              # 32K-line complete prompt collection (2.1MB)
│   └── model-behavioral-contract.md # How Claude is instructed to behave
├── 04-systems/                      # Configuration and telemetry systems
│   ├── feature-flag-encyclopedia.md # 90 feature flags with descriptions
│   ├── constants-encyclopedia.md    # Every constant, threshold, magic number
│   ├── environment-variable-*.md    # Environment variable contract + inventory
│   ├── settings-policy-schema.md    # Settings and policy structure
│   └── telemetry-event-*.md         # Telemetry event catalog + inventory
├── 05-security/                     # Security analysis
│   ├── security-audit.md            # Security architecture findings
│   └── desanitization-map.md        # API sanitization reversal table
├── 06-tools-and-plugins/            # Tool and plugin systems
│   ├── tool-contract-inventory.md   # Tool specifications and schemas
│   ├── command-tool-skill-surface.md # Complete API surface
│   ├── bundled-skills-catalog.md    # Built-in skill inventory
│   └── plugin-marketplace-contract.md # Plugin system architecture
├── 07-agents/                       # Agent subsystems
│   ├── daemon-proactive-agent.md    # Background daemon and proactive mode
│   └── remote-bridge-protocol.md    # Remote communication protocol
├── 08-history/                      # Historical context
│   ├── migration-product-history.md # Product evolution timeline
│   └── Wriction-matrix.md           # Attribution and contribution tracking
└── infographics/                    # Visual architecture diagrams
    ├── architecture-overview.svg
    ├── agent-loop-flow.svg
    ├── compaction-tiers.svg
    ├── yolo-classifier-pipeline.svg
    ├── streaming-tool-executor.svg
    └── system-prompt-ordering.svg
```

## Quick Start

**New to this repo?** Start with these documents in order:

1. **[METHODOLOGY.md](METHODOLOGY.md)** — Understand how this extraction was performed and what techniques were used.

2. **[02-master-extraction/claude-code-rnd-extraction.md](02-master-extraction/claude-code-rnd-extraction.md)** — The authoritative master document. 2,564 lines with 19 appendices covering architecture, systems, security, and operational details.

3. **[01-architecture/codebase-structure.md](01-architecture/codebase-structure.md)** — Get the lay of the land: 38 services, 1,884 files, module organization.

4. **[03-prompts/prompt-bible.md](03-prompts/prompt-bible.md)** — Explore the 32K-line complete prompt collection that instructs Claude's behavior.

For specific topics, jump directly to the relevant sections (e.g., `04-systems/` for feature flags and environment variables, `05-security/` for security architecture).

## By The Numbers

- **1,884** TypeScript source files analyzed
- **6,552** exported functions, constants, and utilities
- **99** classes identified
- **1,308** type definitions
- **90** feature flags documented
- **327** Zod validation schemas
- **38** distinct services
- **2,564** lines in the master extraction document
- **19** appendices with detailed tables and reference material
- **30** analysis markdown documents total

## Methodology

This is not a decompilation or disassembly. The extraction was performed using:

- **Static code analysis** on the TypeScript source
- **AST (Abstract Syntax Tree) inspection** to identify classes, types, and exports
- **Dependency graph analysis** to understand service coupling and complexity
- **Pattern recognition** to isolate architectural decisions
- **Systematic documentation** of findings with citations

See [METHODOLOGY.md](METHODOLOGY.md) for the complete technical approach.

## Origin and Legal Context

On **March 31, 2026**, Anthropic shipped a production build of the `@anthropic-ai/claude-code` npm package that included a `.map` (source map) file. Source maps are debugging artifacts that reconstruct the original TypeScript source from bundled/minified JavaScript. Because npm packages are public by design, this file was accessible to anyone who installed or inspected the package. Anthropic later removed the source map in a subsequent release.

This repository was created by analyzing the source that was exposed through that `.map` file. It is important to understand what this repo is and is not:

**What this repository contains:**
- Original analysis, commentary, and documentation written by the author
- Architectural diagrams and infographics created by the author
- Descriptions of design patterns, algorithms, and system behaviors observed in the source
- Extracted constants, configuration values, and prompt text quoted for research purposes

**What this repository does NOT contain:**
- Anthropic's source code (no `.ts` or `.js` files are redistributed)
- Verbatim copies of substantial code files
- Any material obtained by circumventing access controls, DRM, or security measures
- Any proprietary model weights, training data, or API secrets

**Fair use rationale:** This work constitutes independent research and commentary on a publicly (albeit accidentally) available software artifact. The findings are transformative — the original is a functioning codebase; this repo is a structured analysis documenting engineering patterns for educational purposes. No code is redistributed. Short quotations of constants, prompts, and configuration values are included for accuracy and are analogous to quoting passages in a book review. This falls squarely within fair use / fair dealing doctrine in most jurisdictions.

**Disclaimer:** This repository is not affiliated with, endorsed by, or sponsored by Anthropic. All trademarks (Claude, Claude Code, Anthropic) belong to their respective owners. If Anthropic requests removal of specific content, the author will comply promptly — reach out at hello@karanprasad.com.

## Use Cases

Researchers, engineers, and students will find value in:

- Understanding how production AI coding assistants are architected
- Learning streaming-first design patterns for AI applications
- Studying context management and token optimization strategies
- Reviewing security design for AI-assisted coding tools
- Exploring multi-agent orchestration and coordinator patterns
- Analyzing prompt engineering at scale

## License

The **original analysis, documentation, diagrams, and commentary** in this repository are released under the MIT License:

```
MIT License

Copyright (c) 2026 Karan Prasad

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

**Important:** This license covers only the author's original work (analysis text, diagrams, methodology documentation). It does not grant any rights to Anthropic's underlying intellectual property. Quoted constants, prompt fragments, and configuration values remain the property of Anthropic, PBC and are included here under fair use for research and commentary purposes.

## Contributing

This is a research and documentation repository. Pull requests for corrections, clarifications, and additional analysis are welcome. If you find errors in the extracted analysis, please open an issue with references to the source documentation.

## Acknowledgments

Credit to Anthropic for building a genuinely impressive system. The engineering quality visible in this codebase — streaming-first architecture, multi-tier context management, fail-closed security parsing — reflects serious craftsmanship. This analysis is meant as a contribution to the broader understanding of how production AI systems are built, not as an adversarial act.

The source became available due to an accidental `.map` file inclusion in a public npm package on March 31, 2026. No access controls were bypassed.

---

**Questions, corrections, or takedown requests?** Reach out at hello@karanprasad.com or visit https://karanprasad.com.
