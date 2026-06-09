# AI Asset Governance Patterns

Updated: 2026-06-10

Risk label: SAFE_PATTERN
Production labels: ASSET_PIPELINE, DEBUG_QA, RUNTIME_ANIMATION

## Pattern 1 — AI output never bypasses review

Pipeline:

```text
Generate
↓
Technical validation
↓
Visual review
↓
Runtime integration
↓
Runtime review
↓
Approved
↓
Archive
```

Reject:

```text
generate -> ship
```

## Pattern 2 — Reference bible before generation

A reference bible stores:

```text
shape language
proportions
palette
lighting
material rules
camera rules
approved examples
rejected examples
```

Benefits:

```text
less style drift
higher acceptance rate
better prompt consistency
```

## Pattern 3 — Asset confidence levels

Every AI asset should have a confidence state.

```text
production
approved
reviewed
prototype
reference
rejected
```

Rule:

```text
runtime assets must not use reference-only status
```

## Pattern 4 — Batch generation requires curation rate

Typical flow:

```text
Generate 100
↓
Keep 10-20
↓
Normalize
↓
Runtime test
↓
Archive approved assets
```

Track:

```text
generated count
accepted count
rejected count
rework percentage
approval reasons
```

## Pattern 5 — Asset lineage tracking

Track the full chain.

```yaml
asset_id:
source_file:
generator:
prompt_version:
model_version:
postprocess_steps:
runtime_location:
approval_status:
review_notes:
```

Benefits:

```text
regeneration
auditability
style consistency
future automation
```

## Pattern 6 — Style drift detection

Monitor:

```text
silhouette
palette
proportions
render style
line weight
material language
```

Review trigger:

```text
drift exceeds approved baseline
```

## Pattern 7 — Review lanes

Use separate lanes:

```text
Lane A: production ready
Lane B: requires cleanup
Lane C: reference only
Lane D: rejected
```

Never mix approval states in the same production folder.

## Pattern 8 — AI animation needs gameplay QA

Check:

```text
readability
anticipation
impact frame
pose continuity
weapon alignment
baseline stability
runtime cost
```

AI animation is not approved until it plays correctly in the actual game scene.

## Codex task seeds

```text
Create an AI asset manifest schema with asset_id, source_file, prompt_version, model_version, review_status, runtime_status, approval_notes, and regeneration history.
```

```text
Create a batch review queue that moves assets through Generated, Pending Review, Needs Fixes, Approved, Archived, and Rejected states.
```

```text
Create a style drift checklist comparing new AI assets against an approved baseline set for silhouette, palette, proportions, and material language.
```
