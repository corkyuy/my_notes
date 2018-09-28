# Cabal

## New Project

Use this workflow when starting a new project

    1. copy the fetchNixpkgs.nix
    2. cabal init
    3. cabal2nix . > default.nix
    4. create release.nix
        ```
        let
          fetchNixpkgs = import ./fetchNixpkgs.nix;
          nixpkgs = fetchNixpkgs {
              rev = "01204cf872fa199d92a303f64e2b39731e314473";
          };
          pkgs = import nixpkgs { config = {}; };
        in
          { eithert = pkgs.haskellPackages.callPackage ./default.nix { };
          }
        ```
    5. create shell.nix
        ```
          (import ./release.nix).ethert.env
        ```


## nix pin

[pinning](https://nixos.wiki/wiki/FAQ/Pinning_Nixpkgs)


```
pkgs = let
  hostPkgs = import <nixpkgs> {};
  pinnedPkgs = hostPkgs.fetchFromGitHub {
    owner = "NixOS";
    repo = "nixpkgs-channels";
    # nixos-unstable as of 2017-11-13T08:53:10-00:00
    rev = "ac355040656de04f59406ba2380a96f4124ebdad";
    sha256 = "0frhc7mnx88sird6ipp6578k5badibsl0jfa22ab9w6qrb88j825";
  };
in import pinnedPkgs {};
```

Create new version:
```
> nix-shell -p nix-prefetch-git
> nix-prefetch-git https://github.com/nixos/nixpkgs-channels.git refs/heads/nixos-unstable > nixpkgs-version.json
 
```

## Building

Building the project will be done inside the nix-shell.

  1. Do nix-shell
  1. Do cabal update (if necessary, especially for new nixpkgs revision)
  1. Do cabal build

## Debugging / Testing

Debugging / Testing will need to be inside the nix-shell as well.

  1. Do nix-shell
  1. cabal repl


* install new dependecies

  ```
  > cabal install --dependencies
  ```

* build

  ```
  > cabal build
  ```

* continuous build

  ```
  >ghcid
  ```

