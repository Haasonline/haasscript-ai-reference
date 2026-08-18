# Platform Ports

The same HaasOnline Cloud workflow knowledge, re-cut to fit the length budgets
of assistants other than Claude.

**[`skills/`](../skills/) is canonical.** Those files are the full-length,
on-demand Claude Skills — nothing here contains knowledge that isn't in them,
with one exception noted below. When the workflows change, change `skills/`
first and re-derive these.

## Why ports exist

Claude Skills are loaded on demand, so they can run to several hundred lines.
ChatGPT Projects and Gemini Gems load their instructions eagerly and cap them,
so the same workflow has to be compressed to fit:

| Tier | Budget | Files |
|---|---|---|
| Claude Skills (`skills/`) | on-demand, effectively uncapped | 466–789 lines |
| ChatGPT (`chatgpt/`) | ~8 K characters per document | 5–7 K characters |
| Gemini (`gemini/`) | tighter still | 2–3 K characters |

Compression is lossy by design. The ports keep the decision rules, the
mandatory tool sequences and the safety constraints; they drop the worked
examples, the extended gotcha lists and the verified code templates. If an
assistant on one of these platforms needs that depth, point it at the
`haasscript/` reference or the matching Claude skill directly.

## ChatGPT

Built for ChatGPT Projects. Paste `project-instructions.md` into the Project's
custom-instructions box; upload the rest to the Project's knowledge files.

| File | Role |
|---|---|
| [`project-instructions.md`](chatgpt/project-instructions.md) | The behavioural control layer — role, task routing, tool rules. This is the only file that goes in the instructions box. |
| [`source-map.md`](chatgpt/source-map.md) | Source-authority hierarchy; tells the model which tier wins when sources disagree, and prevents it duplicating the API reference. |
| [`core-rules.md`](chatgpt/core-rules.md) | Shared Cloud rules the three workflow files build on. |
| [`haasscript-development.md`](chatgpt/haasscript-development.md) | Port of [`haasscript-development`](../skills/haasscript-development/SKILL.md) |
| [`haasscript-performance-analysis.md`](chatgpt/haasscript-performance-analysis.md) | Port of [`haasscript-performance-analysis-cloud`](../skills/haasscript-performance-analysis-cloud/SKILL.md) |
| [`haasscript-export-instrumentation.md`](chatgpt/haasscript-export-instrumentation.md) | Port of [`haasscript-export-instrumentation-cloud`](../skills/haasscript-export-instrumentation-cloud/SKILL.md) |

## Gemini

Four files, each Markdown with YAML frontmatter carrying `name` and
`description` — the same shape as a `SKILL.md`, so they drop into any client
that follows that convention as well as into a Gem.

| File | Role |
|---|---|
| [`haasonline-cloud-core-rules.md`](gemini/haasonline-cloud-core-rules.md) | Shared Cloud rules |
| [`haasscript-development.md`](gemini/haasscript-development.md) | Port of [`haasscript-development`](../skills/haasscript-development/SKILL.md) |
| [`haasscript-performance-analysis.md`](gemini/haasscript-performance-analysis.md) | Port of [`haasscript-performance-analysis-cloud`](../skills/haasscript-performance-analysis-cloud/SKILL.md) |
| [`haasscript-export-instrumentation.md`](gemini/haasscript-export-instrumentation.md) | Port of [`haasscript-export-instrumentation-cloud`](../skills/haasscript-export-instrumentation-cloud/SKILL.md) |

## Known divergence: core rules

Both ports factor the rules common to all three workflows — authority
hierarchy, `health_check` pre-flight, dynamic ID resolution, the read-only
live-bot policy, HaasScript language essentials — into a shared `core-rules`
file. The Claude skills have no such file; each one repeats the rules it needs
inline, which is the right trade when skills load independently and one may be
read without the others.

That means core-rules content is currently maintained in three places. Until
`skills/` grows an equivalent shared layer, a change to any of those rules has
to be applied to all three tiers.

## Scope

Every file here targets **HaasOnline Cloud**. There is no self-hosted port —
see [`project-instructions/self-hosted.md`](../project-instructions/self-hosted.md)
for TradeServer, and note that the Cloud MCP exposes a different tool surface,
so these documents will produce failing tool calls against a self-hosted server.
