# AGENTS.md — layer-wacli

Standalone candy repo for the `wacli` layer — the wacli WhatsApp CLI, installed
via `go install`. The candy lives in `charly.yml` at the repo root: the
`require:` on `layer-golang`, the `GOPATH` env and `path_append`, the `go install`
`run:` step, the `check:` assertions, and the embedded `skill:` entity projected
into the marketplace corpus as `/charly-selkies:wacli`.

Canonical files:

- `charly.yml` — the `wacli:` candy entity and the `wacli-skill:` skill entity.
- `CHANGELOG/` — per-CalVer release history.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-selkies:wacli` — the owning skill. The Go-install path, the
  `~/go/bin` location, and the WhatsApp interface. Load before editing or
  troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:` and `agent-check:`, per-distro `distro:`
  arms, package/repo sections, service declarations). Load before editing any
  entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence. The binary
  check and the `agent-check` (the interface presents instead of crashing) are
  the live proof.
- The install runs `go install …@latest`; the `go clean -cache` in the same step
  keeps the image lean.

## Modify this repo

- Edit the `wacli:` candy entity AND the `wacli-skill:` skill entity in
  `charly.yml` together. The skill is the projected usage source, so a behaviour
  change not mirrored in the skill leaves the corpus stale.
- The `require:` pins `layer-golang`; a change to the Go toolchain contract must
  move that pin.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
