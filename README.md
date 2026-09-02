# agentdfir-rules

Community detection rule packs for [AgentDFIR](https://github.com/efij/AgentDFIR) — open-source DFIR for AI agents.

Rules are declarative JSON, shareable independently of the engine (Sigma-style). Load them at triage time:

```sh
agentdfir triage --rules ./rules <case>.adfir
```

## Packs

| Pack | Rules | Purpose |
|---|---|---|
| `rules/starter-pack.json` | 4 | Minimal format example — copy this to start an org pack. |
| `rules/community-pack.json` | 37 | Curated detections: credential access, defense evasion, persistence, container escape, exfil/C2, insecure MCP config. 26 HIGH/CRITICAL, 14 high-confidence. Every rule mapped to MITRE ATT&CK where a valid technique exists, with OWASP LLM / Agentic references. |

The community pack is the canonical copy; the same file ships in the
[AgentDFIR](https://github.com/efij/AgentDFIR/tree/main/rules) repo and is kept in sync on each release.

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
- `references` carries OWASP LLM Top 10 / Agentic mappings; rule IDs must not collide with AgentDFIR built-in rules.

## Contributing

One PR per pack. Include: the pack JSON, a short rationale per rule, and (where possible) a synthetic fixture reproducing the behavior (`agentdfir simulate` output welcome). CI validates pack syntax with the AgentDFIR loader.

## License

MIT
