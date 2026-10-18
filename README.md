# chinmina/nixpkgs

Nix packages for projects published by Chinmina. Release automation publishes package definitions here.

## Install

Install a tool by name from the flake:

```sh
nix profile install github:chinmina/nixpkgs#<tool>
```

Run it without installing:

```sh
nix run github:chinmina/nixpkgs#<tool>
```

Upgrade to the latest release with `nix profile upgrade`.

To add the packages to a NixOS, nix-darwin or home-manager configuration, apply `overlays.default`. The packages then build from that configuration's own nixpkgs:

```nix
# flake inputs
inputs.chinmina-nixpkgs.url = "github:chinmina/nixpkgs";

# configuration
nixpkgs.overlays = [ inputs.chinmina-nixpkgs.overlays.default ];
```

Without flakes, install from `default.nix`:

```sh
nix-env -f https://github.com/chinmina/nixpkgs/archive/main.tar.gz -iA <tool>
```

The flake exposes `x86_64-linux`, `aarch64-linux`, `x86_64-darwin` and `aarch64-darwin`, built from nixpkgs 26.05.

## How packages arrive

Each project's goreleaser `nix:` publisher commits `pkgs/<tool>/default.nix` to `main` on every non-prerelease release. The recipe downloads the release archive for the current system and checks it against the hash computed at release time.

`flake.nix` and `default.nix` expose every directory under `pkgs/`, so a new tool needs no edit here. Nothing runs at publish time: Nix evaluates the flake when a user installs from it.

The `nixpkgs` flake input only supplies the build helpers the recipes use. Nothing here is published to nixpkgs itself.

Do not edit files under `pkgs/` by hand: the next release overwrites them.
