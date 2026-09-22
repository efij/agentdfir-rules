# Changelog

Rule packs for [AgentDFIR](https://github.com/efij/AgentDFIR). Packs are
authored in the AgentDFIR repo under `rules/` and mirrored here; this file
records what changed and when, which the mirror alone never did.

Versions are the `version` field inside each pack, not a release of this
repository.

## community-pack v3 · starter-pack v2 — 2026-09-22

Shipped with AgentDFIR **v1.6.0**, which is the first release where these
rules actually run.

### Fixed
- **`CURL_PIPE_SHELL` was defined in both packs**, so an analysis loading
  both reported it twice against the same evidence. AgentDFIR now
  de-duplicates by rule id at load time (first pack wins) and records the
  drop in its analysis notes, but the duplicate should not have been here.
- **`AGENT_CONFIG_DISCOVERY` fired on ordinary work.** It matched
  `ls ~/.claude/skills/` and `ls ~/.claude/agents/` — which is how an agent
  uses its own skills, not reconnaissance. The rule now matches only the
  settings and MCP configuration files, and `ls` is no longer a listed verb.

### Context
Until AgentDFIR v1.6.0 these packs **never executed for anyone using a
released binary**: they were not embedded, `run` had no `--rules` flag, and
the release archives contain only the binary. On a real machine a case with
1,519 findings had produced every one of them from the built-in Go rules,
with all 81 pack rules idle. They are now compiled into the binary and
load by default, so what this repository publishes is what people run.

## Earlier

community-pack v1–v2 and starter-pack v1 predate this changelog. See the
AgentDFIR CHANGELOG for the releases they shipped with.
