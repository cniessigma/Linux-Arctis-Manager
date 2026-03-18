# Arch Packaging Notes

This repo can be packaged locally on Arch using the `PKGBUILD` in the repository root.

The current package is intended for local development and local installation of a fork, not as a drop-in copy of the existing AUR package. The upstream AUR recipe currently targets an older project layout and uses a different build flow.

## Why this package looks different from the AUR one

- The current repo is built from [pyproject.toml](../pyproject.toml)
- The build backend is `uv_build`
- The repo no longer matches the older `pipenv` + `pyinstaller` layout used by the current AUR package

For that reason, the local `PKGBUILD` builds a wheel and installs it into a pacman package instead of trying to reuse the old AUR recipe as-is.

## Build dependencies

Install the required Arch build tools:

```bash
sudo pacman -S --needed base-devel python-build python-wheel python-installer python-uv-build
```

Runtime dependencies are declared in the `PKGBUILD`.

## Build and install

From the repository root:

```bash
makepkg -si
```

This will:

- build `linux-arctis-manager-git`
- create a local package archive like `linux-arctis-manager-git-2.2.1-1-x86_64.pkg.tar.zst`
- install it with pacman

If the package archive already exists and you want to install it manually:

```bash
sudo pacman -U ./linux-arctis-manager-git-2.2.1-1-x86_64.pkg.tar.zst
```

## Rebuild after local code changes

After changing the code in this repo:

```bash
makepkg -fsi
```

Or, in two steps:

```bash
makepkg -f
sudo pacman -U ./linux-arctis-manager-git-*.pkg.tar.zst
```

## Important packaging details

- The package name is `linux-arctis-manager-git` to avoid conflicting with the public AUR package
- The `PKGBUILD` uses `/usr/bin/python` explicitly so `makepkg` does not accidentally pick up `.venv/bin/python`
- The build frontend is `python -m build`
- The build backend comes from `python-uv-build`, because the project declares `uv_build` in `pyproject.toml`

## Development vs packaging

Use `uv` for development tasks:

```bash
uv run lam-daemon
uv run lam-gui --no-enforce-systemd
uv run lam-cli
uv run pytest
```

Use `makepkg`/`pacman` for system installation:

```bash
makepkg -si
```

## Post-install steps

After installing the package, desktop entries and udev rules may still need to be written:

```bash
lam-cli desktop write
lam-cli udev write-rules --force --reload
```

## Useful pacman commands

Check the installed package:

```bash
pacman -Q linux-arctis-manager-git
```

List installed files:

```bash
pacman -Ql linux-arctis-manager-git
```

Remove the package:

```bash
sudo pacman -Rns linux-arctis-manager-git
```
