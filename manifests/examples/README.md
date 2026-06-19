# Examples

Ready-to-use manifest examples. Edit one of these or build your own from scratch, then commit it.

## Workflow

```bash
# validate
limen validate manifests/examples/logreg_binary.yaml

# profile — timing estimate and data quality check
limen profile manifests/examples/logreg_binary.yaml

# copy an example, set metadata.mode: production, then commit
limen commit manifests/my_strategy.yaml
# → prints: manifest://sha256:abc123

# browse committed manifests
limen ls

# run
limen run manifests/my_strategy.yaml          # development — local file
limen run manifest://sha256:abc123            # production — immutable

# iterate from an existing manifest
limen fork sha256:abc123
# → writes manifest locally with lineage.parent_id set

# inspect lineage
limen lineage sha256:abc123

# backup
limen backup
```

## Available Examples

**`logreg_binary.yaml`** — Logistic regression binary classifier with isotonic/sigmoid calibration,
threshold optimisation, and feature perturbation.
