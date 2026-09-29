# dev-tools

Cross-distro developer CLI toolkit — search, edit, monitor, archive, and FUSE
utilities with a normalized `bat` binary path.

`dev-tools` installs a broad set of command-line developer utilities (ripgrep,
neovim, htop, bat, restic, rclone, yamllint, …) uniformly across Arch, Fedora,
Debian, and Ubuntu. A post-install `plan:` step normalizes Debian/Ubuntu's
`batcat` binary to `/usr/bin/bat` so the same paths and commands resolve on every
distro. Every claim is checkable against the built image: binaries land at fixed
paths and run.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `dev-tools` |
| Binaries | `/usr/bin/rg`, `/usr/bin/nvim`, `/usr/bin/htop`, `/usr/bin/bat`, `/usr/bin/fastfetch` (not on Ubuntu), `/usr/bin/gopls` (Arch only), `/usr/bin/golangci-lint` (Arch only) |
| Packages | `bat`, `bpftrace`, `fdupes`, `html2text`, `htop`, `mosh`, `neovim`, `podman-compose`, `rclone`, `restic`, `ripgrep`, `squashfuse`, `strace`, `sysstat`, `yamllint`, `yq`, `zoxide`, plus per-distro arms |
| Service / port | none |

`fastfetch` is not in Ubuntu noble main, and the distro-specificity cascade
unions packages (it cannot subtract), so it is simply omitted from every Ubuntu
level — the `fastfetch-binary` check carries `exclude_distro: [ubuntu]`.

## How to use it

Compose the layer as an inline list in a box body:

```yaml
my-dev:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-dev-tools:v2026.239.1624'
```

Then, inside the built image:

```bash
rg --version
nvim --version
bat --version        # resolves on every distro (native, or batcat symlink)
yamllint --version
```

## Layout

- `charly.yml` — the `dev-tools:` candy entity (the package + per-distro arms,
  the `bat`→`batcat` symlink `run:` step, the `check:`/`agent-check:`
  assertions) and the embedded `dev-tools-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-coder:dev-tools`
- Siblings: `/charly-coder:gh` (git/GitHub tooling — deliberately NOT installed
  here), `/charly-coder:devops-tools`, `/charly-coder:build-toolchain`
- Consumers: `/charly-coder:debian-coder`, `/charly-coder:ubuntu-coder` (bat→batcat
  symlink), `/charly-coder:fedora-coder`, `/charly-coder:arch-coder`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
