# ezsetup

Ready-made machine setups for [eznix](https://github.com/kalken/eznix), to import from its System menu.

## 📁 Layout

- `nixos/` — NixOS systems
- `macos/` — macOS (nix-darwin)
- `linux/` — any other Linux (standalone home-manager)

Each folder has an `index.json` listing its setups and a zip per setup: a whole flake, `flake.nix` at its top.

## 🚀 Use

Name a folder's index as a source of eznix's `system` plugin:

```nix
services.eznix.plugins.system.sources = [
  {
    name = "Start from a template";
    url = "https://raw.githubusercontent.com/kalken/ezsetup/main/nixos/index.json";
  }
];
```

Then choose one under **System ▸ Start from a template**. An import replaces the flake (a backup is taken first).

## ➕ Adding a setup

Put its zip in the folder and an entry in that folder's `index.json` (`name`, `url`, `path`, `description`).

A good setup works on any CPU, x86_64 or ARM, by taking it from the machine instead of naming it:

- **NixOS**: leave `system` out of `nixosSystem`. `hardware-configuration.nix`, made on the machine by the setup's `generate` step, sets `nixpkgs.hostPlatform`.
- **Options that exist on one CPU only**: make them a condition, as ezvm does for 32-bit graphics: `"enable32Bit": { "_expr": "pkgs.stdenv.hostPlatform.isx86_64" }`.
- **Where the CPU has to be named** (standalone home-manager, which has no hardware file): say which in the setup's `description`.

*Part of the ez family: eznix, ezinstall, ezsetup.*
