# Changelog

All notable changes to the Open Threat Taxonomy (OTC) will be documented in this file.

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
