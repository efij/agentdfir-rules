# agentdfir-rules

Community detection rule packs for [AgentDFIR](https://github.com/efij/AgentDFIR) — open-source DFIR for AI agents.

Rules are declarative JSON, shareable independently of the engine (Sigma-style). Load them at triage time:

```sh
agentdfir triage --rules ./rules <case>.adfir
```

## Rule format

```json
{
  "pack": "my-org", "version": "1",
  "rules": [{
    "id": "MY_RULE", "title": "…", "description": "…",
    "severity": "HIGH", "confidence": "medium",
    "match": {"type": "command|summary|config|transcript",
              "contains": ["…"], "regex": "…"},
    "false_positive_notes": "MANDATORY — expected FP causes",
    "mitre_atlas": "AML.T00xx", "mitre_attack": "T1xxx"
  }]
}
```

- `false_positive_notes` is **mandatory** — rules without FP analysis are rejected by the loader.
- MITRE fields only where a valid technique exists; never force mappings.
- Matched secret-like values are never echoed into findings.

## Contributing

One PR per pack. Include: the pack JSON, a short rationale per rule, and (where possible) a synthetic fixture reproducing the behavior (`agentdfir simulate` output welcome). CI validates pack syntax with the AgentDFIR loader.

## License

MIT
