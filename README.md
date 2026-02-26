# Open Threat Classification (OTC)

A public, versioned classification of threat patterns found in AI agent skills, MCP servers, and plugins. Maintained by [Oathe](https://oathe.ai).

## What is OTC?

The Open Threat Classification catalogs the categories of malicious or risky behavior that Oathe's audit engine detects when evaluating third-party AI agent skills. Each pattern is identified by a T-code (e.g., T1.1) and mapped to one of six scoring dimensions.

OTC publishes pattern metadata only — names, descriptions, dimensions, and severity levels. Detection logic (regex patterns, heuristics, scoring weights) is proprietary and not included in this taxonomy.

In a [study of 1,620 agent skills](https://oathe.ai/engineering/we-audited-1620-ai-agent-skills) from the OpenClaw ecosystem, 88 skills (5.4%) were flagged as dangerous or malicious. The ecosystem's leading safety scanner missed 91% of these threats. Every pattern in this taxonomy was observed in the wild.

**Version**: 1.1.0
**License**: [CC-BY-4.0](LICENSE)

## Scoring Dimensions

Oathe evaluates skills across six behavioral dimensions. Each dimension carries a weight in the composite trust score.

| Dimension | Weight | Description | Active Patterns |
|-----------|--------|-------------|-----------------|
| `prompt_injection` | 0.15 | Attempts to override agent instructions or manipulate LLM behavior | T3.1, T3.2, T3.3, T3.4, T3.5, T3.6, T7.3 |
| `data_exfiltration` | 0.20 | Unauthorized transmission of data to external endpoints | T1.1, T1.5, T7.1 |
| `code_execution` | 0.15 | Unauthorized process execution, filesystem access, or resource abuse | T2.1, T2.2, T5.1, T5.2, T6.1, T7.2 |
| `clone_behavior` | 0.15 | Behavior that modifies the host system beyond the skill's directory | T4.1 |
| `canary_integrity` | 0.20 | Whether the skill tampers with monitoring canaries | *No pattern-based detection in v1.0.0 — scored by model grader* |
| `behavioral_reasoning` | 0.15 | Overall behavioral assessment by the model grader | *No pattern-based detection in v1.0.0 — scored by model grader* |

Weights are from Oathe methodology version 1.0.0.

## Pattern Catalog

Prevalence data from [1,620 audited skills](https://oathe.ai/engineering/we-audited-1620-ai-agent-skills) (88 flagged).

| T-Code | Name | Dimension | Max Severity | Prevalence | Description |
|--------|------|-----------|--------------|------------|-------------|
| T1.1 | Direct Exfiltration | `data_exfiltration` | CRITICAL | 32% | Detects attempts to send data to external endpoints via HTTP, DNS, or raw sockets |
| T1.5 | Credential Harvest | `data_exfiltration` | CRITICAL | 93% | Detects access to credential and secret files beyond stated scope, transmission to external endpoints, or storage in attacker-accessible locations |
| T2.1 | Filesystem Escape | `code_execution` | HIGH | — | Detects path traversal, symlink attacks, and access outside skill directory |
| T2.2 | Process Spawning | `code_execution` | CRITICAL | 10% | Detects suspicious process execution and privilege escalation attempts |
| T3.1 | Prompt Injection | `prompt_injection` | CRITICAL | — | Detects prompt injection patterns in skill content |
| T3.2 | Manifest Spoofing | `prompt_injection` | HIGH | — | Detects package.json install scripts and mismatched metadata |
| T3.3 | Heartbeat C2 | `prompt_injection` | CRITICAL | 35% | Detects remote instruction fetching via heartbeat files, cron-style polling, or self-update mechanisms that allow the skill author to rewrite agent instructions post-install |
| T3.4 | Identity Hijack | `prompt_injection` | HIGH | 13% | Detects persona replacement, anti-transparency rules, and instructions that override the agent's identity, values, or behavioral constraints |
| T3.5 | Anti-Scanner Injection | `prompt_injection` | HIGH | 1% | Detects instructions specifically targeting automated LLM-based auditors, attempting to manipulate the scanner into producing a clean verdict |
| T3.6 | Multi-Language Obfuscation | `prompt_injection` | MEDIUM | 18% | Detects non-English instructions designed to bypass English-language pattern matching in security scanners |
| T4.1 | File Drops | `clone_behavior` | CRITICAL | — | Detects files created outside the skill directory during install |
| T5.1 | Cryptomining | `code_execution` | CRITICAL | — | Detects cryptocurrency mining activities or binaries |
| T5.2 | Denial of Service | `code_execution` | CRITICAL | — | Detects fork bombs, infinite loops, disk fills, and resource exhaustion |
| T6.1 | Environment Sensing | `code_execution` | MEDIUM | — | Detects sandbox detection, environment fingerprinting, and conditional behavior |
| T7.1 | Trojan Auditor | `data_exfiltration` | CRITICAL | 10% | Detects skills disguised as security auditors that exfiltrate code, system prompts, or credentials to attacker-controlled infrastructure while positioning themselves as trusted gatekeepers |
| T7.2 | Autonomous Financial Agent | `code_execution` | CRITICAL | 10% | Detects skills that hold cryptocurrency private keys and execute irreversible on-chain transactions without per-transaction user consent |
| T7.3 | Prompt Worm | `prompt_injection` | CRITICAL | 9% | Detects self-propagating instruction-layer attacks designed to spread across agents through recommendation, auto-installation, or SSH-reachable host propagation |

**Max Severity** represents the highest severity level that any single match of this pattern can produce. Individual matches may be lower depending on context.

**Prevalence** is the percentage of flagged skills (88 total) exhibiting each pattern, from a [1,620-skill audit](https://oathe.ai/engineering/we-audited-1620-ai-agent-skills). "—" means the pattern was not observed at statistically significant rates in this dataset. Credential harvesting (93%) includes a known tendency of the LLM grader to over-flag legitimate credential configuration.

## How Oathe Uses OTC

Each T-code maps to detection logic in the Oathe audit engine. When a skill is submitted for audit, Oathe runs it in a sandboxed environment, collects behavioral evidence, and evaluates it against these patterns. Findings are scored across the six dimensions above, producing a composite trust score and verdict (SAFE, CAUTION, DANGEROUS, or MALICIOUS).

Detection logic is proprietary. This taxonomy is the public classification layer — it tells you *what* Oathe looks for, not *how* it looks for it.

## Machine-Readable Format

The full taxonomy is available in YAML at [`taxonomy/patterns.yaml`](taxonomy/patterns.yaml). This file includes all T-code metadata and can be consumed programmatically.

## Versioning

The taxonomy is versioned independently from Oathe's methodology version. The `patterns.yaml` file includes both:

- `version` — the OTC taxonomy version (tracks when patterns are added, renamed, or reclassified)
- `methodology_version` — the Oathe methodology version that this taxonomy aligns with

When Oathe adds new T-codes or reclassifies existing ones, the OTC version is bumped and documented in the [CHANGELOG](CHANGELOG.md).

## Citation

If you reference this taxonomy in academic or industry publications:

```bibtex
@misc{oathe-otc-2026,
  title  = {Open Threat Taxonomy for AI Agent Skills},
  author = {Oathe},
  year   = {2026},
  url    = {https://github.com/oathe-ai/otc},
  note   = {Version 1.1.0, CC-BY-4.0}
}
```

## License

This work is licensed under [Creative Commons Attribution 4.0 International (CC-BY-4.0)](LICENSE).

You are free to share and adapt this taxonomy for any purpose, including commercial use, as long as you provide attribution.

[![CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
