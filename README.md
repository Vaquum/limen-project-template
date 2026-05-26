<h1 align="center">
  <br>
  <a href="https://github.com/Vaquum"><img src="https://github.com/Vaquum/Home/raw/main/assets/Logo.png" alt="Vaquum" width="150"></a>
  <br>
</h1>

<h3 align="center">limen-project-template: A starting point for YAML-driven ML experiments with Limen.</h3>

<p align="center">
  <a href="#value-proposition">Value Proposition</a> •
  <a href="#quick-start">Quick Start</a> •
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
source venv/bin/activate
```

See [manifests/examples/README.md](manifests/examples/README.md) for the full workflow.

# Project Structure

```
my-project/
  manifests/
    examples/          ← ready-to-use examples, edit or build your own
    committed/         ← managed by limen, do not edit manually
  results/             ← experiment outputs, one folder per run
  limen.toml           ← project config: Python version, backup remote
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

# License

MIT
