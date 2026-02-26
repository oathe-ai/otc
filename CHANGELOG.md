# Changelog

All notable changes to the Open Threat Taxonomy (OTC) will be documented in this file.

## v1.1.0 — 2026-02-27

Seven new patterns covering agent-ecosystem-specific attacks discovered during the [1,620-skill OpenClaw audit](https://oathe.ai/engineering/we-audited-1620-ai-agent-skills).

### New Patterns

- T3.3 — Heartbeat C2 (35% prevalence in flagged skills)
- T3.4 — Identity Hijack (13%)
- T3.5 — Anti-Scanner Injection (1%)
- T3.6 — Multi-Language Obfuscation (18%)
- T7.1 — Trojan Auditor (10%)
- T7.2 — Autonomous Financial Agent (10%)
- T7.3 — Prompt Worm (9%)

### New Dimension Coverage

- `prompt_injection` — 4 new patterns (T3.3, T3.4, T3.5, T3.6, T7.3)
- `data_exfiltration` — 1 new pattern (T7.1)
- `code_execution` — 1 new pattern (T7.2)

### Added

- Prevalence data from 1,620-skill audit added to Pattern Catalog
- Richer descriptions synced from YAML to README

---

## v1.0.0 — 2026-02-22

Initial release of the Open Threat Taxonomy.

### Patterns

- T1.1 — Direct Exfiltration
- T1.5 — Credential Harvest
- T2.1 — Filesystem Escape
- T2.2 — Process Spawning
- T3.1 — Prompt Injection
- T3.2 — Manifest Spoofing
- T4.1 — File Drops
- T5.1 — Cryptomining
- T5.2 — Denial of Service
- T6.1 — Environment Sensing

### Dimensions Covered

- `data_exfiltration` (T1.1, T1.5)
- `code_execution` (T2.1, T2.2, T5.1, T5.2, T6.1)
- `prompt_injection` (T3.1, T3.2)
- `clone_behavior` (T4.1)

### Dimensions Without Active Patterns

- `canary_integrity` — scored by model grader only in v1.0.0
- `behavioral_reasoning` — scored by model grader only in v1.0.0
