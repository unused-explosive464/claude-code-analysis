# Product History & Attribution

Product evolution, architectural transitions, and community intelligence on Claude Code's development. This section documents version history, migration patterns, architectural decisions, and attribution tracking across subsystems.

## Documents

| Filename | Description | Lines |
|----------|-------------|-------|
| **migration-product-history.md** | Complete migration inventory tracking 11 shipped migrations, version evolution from earlier Claude CLI versions, architectural transition patterns (model aliases, settings consolidation, MCP approval workflow), deprecated names and default shifts | 226 |
| **Wriction-matrix.md** | Attribution and contribution tracking across subsystems, component ownership, development history | 155 |
| **community-osint.md** | Community-sourced intelligence and public observations about Claude Code development, external signals correlated with source analysis | 98 |

## Key Findings

### Product Evolution

- **11 Shipped Migrations**: Full inventory of product changes encoded in migration code
  - Settings-store consolidation wave: auto-updates, bypass-permissions, MCP approval
  - Model-alias and default shift wave: Pro → Opus transition, model version rollouts
  - Permission system evolution: `bypassPermissions` acknowledgement tracking
  - Remote control branding: `replBridgeEnabled` → `remoteControlAtStartup` rename

### Migration Patterns

- **Legacy Config Consolidation**: Migration from global config to `settings.json` layer
  - Auto-updates setting migration with disabled state preservation
  - Bypass-permissions acknowledgement normalization
  - MCP server approval state merging (enabled + disabled lists)

- **Model Defaults and Aliasing**:
  - Fennec → Opus migration path
  - Opus → Opus 1M → current version rollout
  - Sonnet 1M → Sonnet 4.5 → Sonnet 4.6 progression
  - Pro default shifted to Opus with user notification
  - Opt-in reset for auto-mode defaults

- **Rollout Safety Conditions**: Migrations conditional on user state (custom settings preserved, opt-in tracking)

### Architectural Transitions

- **Permission System Maturation**: Evolution from simple allow/deny to six-mode system with YOLO classifier
- **Settings System Growth**: Progressive consolidation of configuration sources into unified settings layer
- **MCP Integration**: Shift from project config to local settings for server approval state

### Attribution & Development History

- Component ownership tracking across subsystems
- Contribution history and responsibility matrix
- Development timeline correlated with feature releases

### Community Intelligence

- External observations from public channels and discussions
- Development signals from community feedback
- Architectural inference from public statements and demos

---

**Analysis Date**: 2026-04-02
**Source**: Shipped migration code and product startup path analysis
