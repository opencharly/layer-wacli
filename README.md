# layer-wacli

WhatsApp messaging and history sync from the command line for OpenCharly images.

The `wacli` candy installs the [wacli](https://github.com/openclaw/wacli) WhatsApp
CLI by `go install`-ing `github.com/openclaw/wacli/cmd/wacli@latest` with
`GOPATH=~/go`, so the `wacli` binary lands in the user's Go bin (`~/go/bin`,
already appended to `$PATH`). It requires the `golang` layer, which provides the
Go toolchain that builds and runs it.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `wacli` |
| Binary | `~/go/bin/wacli` |
| Env | `GOPATH=~/go`, PATH append `~/go/bin` |
| Requires | `layer-golang` |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-messaging-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-wacli:v2026.243.0410'
```

Then, inside the built image:

```bash
wacli            # presents the WhatsApp command interface / pairing prompt
```

The candy's `plan:` asserts the binary at `~/go/bin/wacli`, that it carries the
executable bit and can be invoked, an `agent-check` that invoking `wacli`
presents its WhatsApp interface instead of crashing, and the Go toolchain at
`/usr/bin/go`.

## Layout

- `charly.yml` — the `wacli:` candy entity (the `require:` on `layer-golang`,
  the `GOPATH` env and `path_append`, the `go install` `run:` step, the `check:`
  assertions) and the embedded `wacli-skill:` skill entity.
- `CHANGELOG/` — per-CalVer release history.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-selkies:wacli`
- `/charly-coder:golang` — Go toolchain dependency
- `/charly-openclaw:openclaw-full` — metalayer that bundles wacli
- `/charly-hermes:hermes` — companion messaging stack with a WhatsApp bridge
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
