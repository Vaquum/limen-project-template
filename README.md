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

Backup pushes your project to a git remote you own. **Use a fresh, empty
repository** — a brand-new repo defaults to the `main` branch, which matches
your project, so backup and restore line up with no git fiddling.

**1. Create an empty repository.** Either is fine:

```bash
gh repo create my-project --private        # GitHub CLI — one command
```

…or make one in the browser at <https://github.com/new> (leave it empty — no
README, no .gitignore, no license).

**2. Point `backup_remote` at it** in `limen.toml`:

```toml
[store]
backup_remote = "git@github.com:user/my-project.git"
```

(You can also set this when you create the project:
`limen new my-project --backup-remote git@github.com:user/my-project.git`.)

**3. Back up** — this snapshots your project and pushes it:

```bash
limen backup
```

The committed manifest store, project config, and committed-manifest results
(`results/<hash>/`) are backed up. Development runs under `results/dev/` are
local-only and never pushed.

## Restore on a new machine

```bash
pip install vaquum-limen
limen new my-project --from git@github.com:user/my-project.git
```

## Using git directly

`backup_remote` is a normal git remote and your project is a normal git repo,
so you can always push and clone by hand instead:

```bash
git push git@github.com:user/my-project.git HEAD     # back up
git clone git@github.com:user/my-project.git         # restore
```

This is the escape hatch if a backup ever needs a force-push or a specific
branch — `limen backup` never force-pushes, so it hands those cases to you.

# License

MIT
