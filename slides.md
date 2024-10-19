## The Nix Package Manager

beCamp 2024

https://github.com/edyburn/beCamp-2024-nix/

---

### What is Nix?

- A package manager
- A pure, lazy, functional programming language
- A Linux distribution (NixOS)

- Developed as part of a PhD thesis; initially released in 2003

---

### Why use Nix?

- Reproducible builds
- Reproducible development environments
- Run programs with different versions of a shared dependency
- Atomic upgrades and rollbacks to previous configs

---

### Why use Nix? cont.

- Manage your home configuration files and environment with [home-manager](https://github.com/nix-community/home-manager/)
- Manage the configuration of a whole Linux system with [NixOS](https://nixos.org/manual/nixos/stable/)
- Manage the configuration of your Mac with [nix-darwin](https://github.com/LnL7/nix-darwin/)
- Build VM and Docker images with [nixos-generators](https://github.com/nix-community/nixos-generators)

---

### Nixpkgs - graph of package repository size vs freshness

![700](./map_repo_size_fresh.svg)
from https://repology.org/repositories/graphs

---

### Nixpkgs

- Over 100k packages
- Cross-platform, and cross-architecture
	- x86_64-linux, aarch64-linux, x86_64-darwin and aarch64-darwin

---

### How does it work?

- Builds are performed in an isolated environment (sandbox)
- Hashes inputs and outputs of a build
- Artifacts are kept in the nix store (usually `/nix/store`)
- References to files in the nix store (e.g. `$PATH` or symlinks)

---

### Nix language - basic data types

```nix
{
  boolean = true;
  integer = 42;
  float = 6.28;
  string = "something";
  list = [  1  2 "a" ];
  attributeSet = {
    a = true;
    b = 2;
    c.d = "nested";
  };
};
```

---

### Nix language - more strings, paths, comments

```nix
{
  multiLineString = ''
    first
    second
  '';
  interpolation = "Hello ${name}";
  path = ./README.md;
  # comment
  /* multi-line comment */
};
```

---

### Nix language - functions

```nix
{
  vars = let x = 1; y = 2;
	     in x + y;
  function = x: x + 1;
  functionCall = function  0;
  attrArgument =
    args@{ a, b ? 2, ... }:
    a + b + args.c;
};
```

---

### Nix language - other

- Recursive attribute sets (`rec {}`)
- `with`
- `inherit`
- Function libraries e.g. `builtins`
- `import`

- Run `nix repl` to try expressions interactively

---

## Channels and Flakes

- Channels are the original mechanism for referencing package repositories like nixpkgs
- Flakes are considered an "experimental" feature, but are widely used
- A flake is a directory with a `flake.nix` file, which has a specific structure
- A `flake.lock` file is generated to track the specific versions of inputs used

- I'll be using flakes in the examples/demos.

---

## Ad-hoc tool usage

- `nix search nixpkgs neovim` (or https://search.nixos.org/packages)
- `nix run nixpkgs#neovim`
- `nix shell nixpkgs#hello nixpkgs#neovim`

---

### Reusable development environment with a flake

- `nix flake init` or `nix flake init --template "$URL"` to create a flake
- https://github.com/the-nix-way/dev-templates/ offers templates for many languages
- `nix develop` to start a shell with the "default" config in the flake
- [direnv](https://direnv.net/) can automatically activate the environment when you enter the directory
- `nix flake update` to update packages

---
## Demos

- python
- rust
- nix-darwin

---

### Issues

- Ecosystem fragmentation, e.g. nix was [forked](https://lix.systems/), code formatting style only standardized earlier this year
- Steep learning curve
- Changes to commonly used dependencies will cause many packages to be rebuilt, e.g. after the xz backdoor earlier this year it took several days to rebuild nixpkgs (citation needed)

---
### Thank you!

Questions?

https://github.com/edyburn/beCamp-2024-nix/

---
### Extras

- Dev environment tools built on top of nix that might be easier to start with:
    - https://devenv.sh/
    - https://www.jetify.com/devbox/
- Garbage collection with `nix-collect-garbage`
- The nix store is readable by everyone on the system, so generally you want to avoid secrets ending up in there...
	- https://nixos.wiki/wiki/Comparison_of_secret_managing_schemes
- Other nix search sites
	- https://flakehub.com/
	- https://mynixos.com/
	- https://noogle.dev/
