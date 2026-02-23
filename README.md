# Open Threat Classification (OTC)

A public, versioned classification of threat patterns found in AI agent skills, MCP servers, and plugins. Maintained by [Oathe](https://oathe.ai).

## What is OTC?

The Open Threat Classification catalogs the categories of malicious or risky behavior that Oathe's audit engine detects when evaluating third-party AI agent skills. Each pattern is identified by a T-code (e.g., T1.1) and mapped to one of six scoring dimensions.

OTC publishes pattern metadata only — names, descriptions, dimensions, and severity levels. Detection logic (regex patterns, heuristics, scoring weights) is proprietary and not included in this taxonomy.

**Version**: 1.0.0
**License**: [CC-BY-4.0](LICENSE)

## Scoring Dimensions

Oathe evaluates skills across six behavioral dimensions. Each dimension carries a weight in the composite trust score.

| Dimension | Weight | Description | Active Patterns |
|-----------|--------|-------------|-----------------|
| `prompt_injection` | 0.15 | Attempts to override agent instructions or manipulate LLM behavior | T3.1, T3.2 |
| `data_exfiltration` | 0.20 | Unauthorized transmission of data to external endpoints | T1.1, T1.5 |
| `code_execution` | 0.15 | Unauthorized process execution, filesystem access, or resource abuse | T2.1, T2.2, T5.1, T5.2, T6.1 |
| `clone_behavior` | 0.15 | Behavior that modifies the host system beyond the skill's directory | T4.1 |
| `canary_integrity` | 0.20 | Whether the skill tampers with monitoring canaries | *No pattern-based detection in v1.0.0 — scored by model grader* |
| `behavioral_reasoning` | 0.15 | Overall behavioral assessment by the model grader | *No pattern-based detection in v1.0.0 — scored by model grader* |

Weights are from Oathe methodology version 1.0.0.

## Pattern Catalog

| T-Code | Name | Dimension | Max Severity | Description |
|--------|------|-----------|--------------|-------------|
| T1.1 | Direct Exfiltration | `data_exfiltration` | CRITICAL | Detects attempts to send data to external endpoints via HTTP, DNS, or raw sockets |
| T1.5 | Credential Harvest | `data_exfiltration` | CRITICAL | Detects access to credential and secret files |
| T2.1 | Filesystem Escape | `code_execution` | HIGH | Detects path traversal, symlink attacks, and access outside skill directory |
| T2.2 | Process Spawning | `code_execution` | CRITICAL | Detects suspicious process execution and privilege escalation attempts |
| T3.1 | Prompt Injection | `prompt_injection` | CRITICAL | Detects prompt injection patterns in skill content |
| T3.2 | Manifest Spoofing | `prompt_injection` | HIGH | Detects package.json install scripts and mismatched metadata |
| T4.1 | File Drops | `clone_behavior` | CRITICAL | Detects files created outside the skill directory during install |
| T5.1 | Cryptomining | `code_execution` | CRITICAL | Detects cryptocurrency mining activities or binaries |
| T5.2 | Denial of Service | `code_execution` | CRITICAL | Detects fork bombs, infinite loops, disk fills, and resource exhaustion |
| T6.1 | Environment Sensing | `code_execution` | MEDIUM | Detects sandbox detection, environment fingerprinting, and conditional behavior |

**Max Severity** represents the highest severity level that any single match of this pattern can produce. Individual matches may be lower depending on context.

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
  note   = {Version 1.0.0, CC-BY-4.0}
}
```

## License

This work is licensed under [Creative Commons Attribution 4.0 International (CC-BY-4.0)](LICENSE).

You are free to share and adapt this taxonomy for any purpose, including commercial use, as long as you provide attribution.

[![CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
