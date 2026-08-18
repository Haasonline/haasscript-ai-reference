# HaasScript Project Source Map — Final v2.1

## Purpose

Define the project's source hierarchy and prevent duplication between the
ChatGPT Project, HaasOnline Cloud MCP documentation, workflow skills, and
uploaded reference material.

The Project Instructions are already maintained separately as **v2.1** and
remain the behavioral control layer.

---

## 1. Source authority

### Tier 1 — Current HaasBot Cloud MCP

The connected HaasBot Cloud MCP is authoritative for current Cloud behavior.

Use:
- current tool schemas
- direct tool responses
- `health_check`
- `search_haasscript_docs`
- current compile/runtime/backtest/Lab results.

### Tier 2 — HaasScript MCP documentation

`search_haasscript_docs` is authoritative for current HaasScript API details:

- command existence
- exact command names
- signatures
- parameter order/types
- return types
- command semantics when documented.

Do not maintain a duplicate HaasScript API catalogue in the Project
Instructions or Cloud workflow skills.

When a command is uncertain, query the MCP documentation instead of guessing.

### Tier 3 — Cloud workflow skills

These define how the assistant should perform Cloud engineering tasks:

- `core-rules.md`
- `haasscript-development.md`
- `haasscript-performance-analysis.md`
- `haasscript-export-instrumentation.md`

They contain methodology and Cloud-specific operational knowledge, not a
duplicate HaasScript API reference.

### Tier 4 — Uploaded HaasScript references

The uploaded HaasScript documentation remains useful as supplementary
reference material:

- HaasScript Part 1 / Part 2
- mathematics
- price data
- technical analysis
- charting
- trade actions.

Use these for explanation, background, examples, and concepts when helpful.

They do **not** override current MCP documentation when the two conflict.

### Tier 5 — Research and examples

Historical scripts, experiments, and strategy examples are patterns only.

Never treat an example as proof that a command is currently supported.

---

## 2. What belongs where

### Project Instructions v2.1
Keep:
- role and behavior
- task routing
- tool discipline
- safety
- evidence standards
- high-level HaasScript conventions
- live-bot read-only policy
- backtest/Lab principles.

Do not add a HaasScript API catalogue.

### Cloud Core
Keep:
- current Cloud mechanics
- tool-use rules
- backtest evidence requirements
- pagination
- ID resolution
- Cloud-specific runtime semantics.

### Development
Keep:
- create/modify workflow
- compile/debug
- backup/versioning
- regression testing
- zero-trade diagnosis
- implementation methodology.

### Performance
Keep:
- benchmark/alpha analysis
- fee/edge analysis
- drawdown/risk analysis
- Lab robustness
- regime validation
- production-readiness methodology.

### Instrumentation
Keep:
- decision-edge capture
- export design
- export retrieval
- feature/outcome joins
- optimization hypothesis workflow.

### Uploaded HaasScript references
Keep as supplementary knowledge. Do not reproduce their API catalogue
elsewhere.

---

## 3. Conflict resolution

When sources disagree:

1. Current MCP tool behavior/schema wins for Cloud operations.
2. `search_haasscript_docs` wins for current HaasScript API details.
3. Project workflow rules win for how this assistant should perform engineering
   tasks, provided they do not contradict current tool behavior.
4. Uploaded references provide supplementary explanation.
5. Historical examples are lowest authority.

Do not silently reconcile a conflict. State it and verify the disputed behavior.

---

## 4. Maintenance rules

- Do not copy current HaasScript command signatures into every skill.
- Do not duplicate Cloud limits across every skill.
- Update Cloud Core when shared Cloud behavior changes.
- Update only the affected specialist skill when workflow methodology changes.
- Keep volatile Cloud/pricing/license information separate from stable
  HaasScript language concepts.
- Mark experimentally observed behavior as observed/verified, preferably with
  a verification date.
- Preserve known-good scripts as versioned backups.
- Prefer proven project patterns over large generic examples.

---

## 5. Desired architecture

```text
ChatGPT Project Instructions v2.1
              |
              v
     HaasOnline Cloud Core
              |
      +-------+--------+
      |       |        |
 Development Performance Instrumentation
      |       |        |
      +-------+--------+
              |
              v
   Current MCP HaasScript Docs
      search_haasscript_docs
              |
              v
 Uploaded reference material
   (supplementary context)
              |
              v
      Research / examples
```

The key principle is:

**Do not build a second HaasScript documentation system inside the project.
Use the MCP connector as the living API reference.**
