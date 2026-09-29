# AGENTS.md — layer-dev-tools

Standalone candy repo for the `dev-tools` layer — a cross-distro developer CLI
toolkit with a normalized `bat` path. The candy lives in `charly.yml` at the repo
root: the package + per-distro arms, the `bat`→`batcat` symlink `run:` step, the
`check:`/`agent-check:` assertions, and the embedded `skill:` entity projected
into the marketplace corpus as `/charly-coder:dev-tools`.

Canonical files:

- `charly.yml` — the `dev-tools:` candy entity and the `dev-tools-skill:` skill
  entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-coder:dev-tools` — the owning skill. The package set, the per-distro
  drops, the `bat`→`batcat` symlink, and the `exclude_distro:` test. Load before
  editing or troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`/`run:`, per-distro `distro:` arms, package
  sections). Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence: `rg`, `nvim`,
  `htop`, the `/usr/bin/bat` symlink, `yamllint --version`, and the
  `exclude_distro:`-gated `fastfetch`/`gopls`/`golangci-lint` binaries. They must
  stay valid on every distro arm they run on.
- Scope a distro-specific check in the check itself — the `exclude_distro:` field
  is how a check opts out of a distro (the check runner does not honour
  runner-level exclusion).

## Modify this repo

- Edit the `dev-tools:` candy entity AND the `dev-tools-skill:` skill entity in
  `charly.yml` together. The skill is the projected usage source, so a package or
  behaviour change not mirrored in the skill leaves the corpus stale.
- `fastfetch` must stay omitted from every Ubuntu level (the cascade cannot
  subtract); the `bat`→`batcat` symlink stays guarded and idempotent.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
