<h1 align="center">
  <br>
  <a href="https://github.com/Vaquum"><img src="https://github.com/Vaquum/Home/raw/main/assets/Logo.png" alt="Vaquum" width="150"></a>
  <br>
</h1>

<h3 align="center">limen-project-template: A starting point for manifest-driven Limen experiments.</h3>

<p align="center">
  <a href="#value-proposition">Value Proposition</a> •
  <a href="#quick-start">Quick Start</a> •
  <a href="#workflow">Workflow</a> •
  <a href="#project-structure">Project Structure</a> •
  <a href="#backup">Backup</a> •
  <a href="#license">License</a>
</p>
<hr>

# Value Proposition

Limen lets you define, run, and track machine learning experiments entirely in YAML — no boilerplate code, no bespoke experiment harnesses. This template gives you a ready-to-use project structure with a built-in manifest store, so every experiment you commit is immutable, content-addressed, and reproducible by anyone on any machine.

# Quick Start

```bash
pip install vaquum-limen
limen new my-project
cd my-project
limen init my_experiment --template logreg_binary
```

`limen list-templates` shows every available starting point.

# Workflow

```bash
# scaffold an experiment from a template (lands in manifests/)
limen init my_experiment --template logreg_binary

# check it
limen validate manifests/my_experiment.yaml

# timing estimate and data quality check
limen profile manifests/my_experiment.yaml

# set metadata.mode: production, then commit to the immutable store
limen commit manifests/my_experiment.yaml
# → prints: manifest://sha256:abc123

# browse committed manifests
limen ls

# run
limen run manifests/my_experiment.yaml        # development — local file
limen run manifest://sha256:abc123            # production — immutable

# iterate from a committed manifest (records lineage.parent_id)
limen fork sha256:abc123

# inspect the lineage chain
limen lineage sha256:abc123

# rebuild the index from committed files, if it ever drifts
limen reindex
```

# Project Structure

```
my-project/
  manifests/
    committed/         ← managed by limen, do not edit manually
    my_experiment.yaml ← your working experiments (from limen init / fork)
  results/             ← experiment outputs, one folder per run
    dev/               ← development runs (gitignored, local-only)
    <hash>/            ← committed-manifest runs (kept and backed up)
  limen.toml           ← project config: backup remote
  .gitignore
  LICENSE
  README.md
```

# Backup

Set `backup_remote` in `limen.toml`, then:

```bash
limen backup
```

Restoring on a new machine:

```bash
pip install vaquum-limen
limen new my-project --from git@github.com:user/my-project.git
```

The manifest store, project config, and committed-manifest results are backed up. Development runs under `results/dev/` are local-only and never pushed.

# License

MIT
