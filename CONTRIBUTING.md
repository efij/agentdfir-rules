# Contributing rules

**Open your pull request against
[AgentDFIR](https://github.com/efij/AgentDFIR), not this repository.**

This repo is a **mirror**. `.github/workflows/sync.yml` pulls `rules/*.json`
from AgentDFIR `main` every six hours, validates them with the real loader,
and commits only on change. A pull request merged here would be silently
overwritten by the next sync, and the rule would never reach anyone: since
AgentDFIR v1.6.0 the packs are compiled into the binary, so what ships is
whatever is in the AgentDFIR repo at build time.

This repo exists so the rules can be read, diffed and pinned on their own,
independently of the engine.

## Where to make the change

| You want to | Do this |
|---|---|
| Add or edit a rule | PR against `rules/` in [AgentDFIR](https://github.com/efij/AgentDFIR/tree/main/rules) |
| Report a false positive | Open an issue on AgentDFIR with the rule id and the command or file that matched |
| Pin a specific rule set | Use a tag here (for example `community-pack-v3`) and pass `--rules` |

## What a rule needs

The loader (`internal/rulepack`) rejects a pack that does not satisfy all of
this, so a rule that passes `agentdfir rules validate` will load:

- `id` — unique across **every** pack. The loader de-duplicates by id and the
  first pack wins, so a repeated id is silently dropped rather than fired
  twice. `CURL_PIPE_SHELL` was defined in both shipped packs and reported
  every match twice until v1.6.0.
- `title`, `severity` (`INFO`…`CRITICAL`), and a `match` with `contains` or
  `regex` (2048 bytes maximum).
- `false_positive_notes` — **mandatory**. If you cannot describe when the
  rule is wrong, it is not ready. Severity says how bad it is if real;
  these notes are what stop an analyst treating a guess as a fact.
- `mitre_attack` / `mitre_atlas` where a technique genuinely applies. Do not
  invent a mapping to look thorough: a rule that describes ordinary agent
  activity is a building block, not a technique.

## Test it before you send it

```sh
agentdfir rules validate ./rules          # the real loader
agentdfir rules list                      # everything that will run
```

In the AgentDFIR repo, add the rule to the benign or attack corpus under
`internal/corpus/testdata/` so CI measures it. Benign cases carry a
false-positive budget that is currently zero for every rule; an attack case
fails the build if the rule stops firing. A rule with no corpus case is a
rule nobody can tell is working.
