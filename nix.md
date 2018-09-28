# NIX


# NIX & Haskell project creation

> cabal init

```
# default.nix
# To pin to a specific version of nixpkgs, you can substitute <nixpkgs> with:
# `(builtins.fetchTarball "https://github.com/NixOS/nixpkgs/archive/<nixpkgs_commit_hash>.tar.gz")`
{ pkgs ? import <nixpkgs> {} }: pkgs.haskellPackages.developPackage
  { root = ./.;
    overrides = self: super:
      { # Don't run a package's test suite
        # foo = pkgs.haskell.lib.dontCheck pkgs.haskellPackages.foo;
        #
        # Don't enforce package's version constraints
        # bar = pkgs.haskell.lib.doJailbreak pkgs.haskellPackages.bar;
        #
        # To discover more functions that can be used to modify haskell
        # packages, run "nix-repl", type "pkgs.haskell.lib.", then hit
        # <TAB> to get a tab-completed list of functions.
      };
    source-overrides =
      { # Use a specific hackage version
        # optparse-applicative = "0.14.0.0";
        #
        # Use a particular commit from github
        # my-private-package = pkgs.fetchFromGitHub
        #   { owner = "my-github-username";
        #     repo = "my-private-package";
        #     rev = "561de381bcccfe6792f2908a5022449a05ae0050";
        #     sha256 = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa";
        #   };
      };
  }
```

```
# shell.nix
(import ./. {}).env

```
env is for the environment you use. This can be disregarded


```
nix-build
or 
nix-shell
```


```
> ghcid -c "cabal v1-repl exe:hw8"
```



















## Info

* nix-env uses ~/.nix-defexpr
    - others uses NIX\_PATH

* Using unstable
    ```
    $ nix-channel --add https://nixos.org/channels/nixos-unstable nixos
    $ nix-channel --remove nixpkgs
    $ nix-channel --update
    ```

## Workflow


## FAQ

* Official cheatsheet?

    [Cheatsheet](https://nixos.wiki/wiki/Cheatsheet)

* How to update?  ```
    nix-channel --update
    nix-env -u
    ```
* Where do I find nix channel status?  [channel status](http://howoldis.herokuapp.com/) * How to update nix?

    ```
    ```

* How to generate a nixpkg.json? Getting the sha256 and stuff

    ```
    > nix-prefetch-git https://github.com/NixOS/nixpkgs.git 2c288548b93b657365c27a0132a43ba0080870cc
    ```

* Generate nix from cabal file

    ```
    > cabal2nix . > default.nix
    ```

* How to install haskellPackages?

    ```
    > nix-env -qaP -f "<nixpkgs>" -A haskellPackages | grep text
    ```

* How to start a new cabal project?

    Steps:
    1. copy the fetchNixpkgs.nix
    2. cabal init
    3. cabal2nix . > default.nix
    4. create release.nix
    ```
let
  fetchNixpkgs = import ./fetchNixpkgs.nix;
  nixpkgs = fetchNixpkgs {
    # tag: release-18.03
    rev = "120b013e0c082d58a5712cde0a7371ae8b25a601";
  };
  pkgs = import nixpkgs { config = {}; };
in
  { hb = pkgs.haskellPackages.callPackage ./hb-exercise.nix {compiler="ghcHEAD";};
  }
    ```

    5. create shell.nix
    ```
(import ./release.nix).hb
    ```

    6. create project.nix
    ```
{ nixpkgs ? import <nixpkgs> {}, compiler ? "default" }:
let
  inherit (nixpkgs) pkgs;
  f = import ./default.nix;
  haskellPackages = if compiler == "default"
                        then pkgs.haskellPackages
                        else pkgs.haskell.packages.${compiler};
  drv = haskellPackages.callPackage f {};
in

  if pkgs.lib.inNixShell then drv.env else drv
    ```

    7. nix-shell

    8. Update default.nix for haskell packages used, eg.

    ```
{ mkDerivation, base, stdenv, cabal, QuickCheck, ghc }:
mkDerivation {
  pname = "hb-exercise";
  version = "1.0.0";
  src = ./.;
  isLibrary = false;
  isExecutable = true;
  libraryHaskellDepends = [];
  executableHaskellDepends = [ base ghc QuickCheck ];
  testHaskellDepends = [base];
  homepage = "https://corky.com";
  license = stdenv.lib.licenses.bsd3;
}
    ```

* How to get Nixpkgs revision (version) you want to use?

  Go to https://github.com/NixOS/nixpkgs and get the version you want to use

* How to add more dependecies by in nix

  - Add to default.nix

      ```
      { mkDerivation, base, stdenv, mtl }:
      mkDerivation {
        ...
        isExecutable = true;
        executableHaskellDepends = [ base mtl ];
        isLibrary = true;
        libraryHaskellDepends = [ base mtl ];
      ```
  - Remake the shell

## Basics
nix ==> nix-channel --add https://nixos.org/channels/nixpkgs-unstable
nix ==> nix-env -q     ==> list all installed
nix ==> nix-env -qas   ==> status of available package
nix ==> nix-env -u vim ==> upgrade
nix ==> nix-env -e vim ==> uninstall

nix ==> default.nix
nix ==> shell.nix
vim ==> easymotion ==> search in line \l
vim ==> easymotion ==> /word<tab><tab>

nix-channel --add https://nixos.org/channels/nixpkgs-unstable  => update nix
nix-channel --update  => update nix

## Building nix store
nix-store -r /nix/store/xxxxxxxxx.drv ### build nix store


## NIX Best practise

* Always create one (or more) profile(s), to organise properly your different
  environments
* What is my current profile? ls -al ~/.nix-profile
* Create/switch between profiles? nix-env --switch-profile
  $NIX_USER_PROFILE_DIR/some_name
* What are my available profiles? ls -al $NIX_USER_PROFILE_DIR


## NIX packages management

```
nix-env -qaP | grep python3-3   --> search nix
nix-env -q                      --> list all installed packages nix
nix-env -i python3-3.3.3        --> nix install
nix-env -iA nixpkgs.python3     --> using attribute name nix
nix-env -uA nixpkgs.python3     --> update nix package
nix-env -u                      --> update all nix
nix-env -e python3-3.3.3        --> uninstall nix
nix-env -qs                     --> list installed package in profile
nix-env --list-generations      --> check entire profile history( link generations)
nix-env --rollback              --> rollback
nix-env --switch-generation 4   --> switch generation
```

## NIX generic builder script

```bash
set -e
unset PATH
for p in $buildInputs; do
  export PATH=$p/bin${PATH:+:}$PATH
done

tar -xf $src

for d in *; do
  if [ -d "$d" ]; then
    cd "$d"
    break
  fi
done

./configure --prefix=$out
make
make install
```

## NIX derivation

```
with (import <nixpkgs> {});
derivation {
  name = "hello";
  builder = "${bash}/bin/bash";
  args = [ ./builder.sh ];
  buildInputs = [ gnutar gzip gnumake gcc binutils coreutils gawk gnused gnugrep ];
  src = ./hello-2.9.tar.gz;
  system = builtins.currentSystem;
}
```


## NIX autotool derivation tool

**autotools.nix**

```
pkgs: attrs:
  with pkgs;
  let defaultAttrs = {
    builder = "${bash}/bin/bash"; args = [ ./builder.sh ];
    baseInputs = [ gnutar gzip gnumake gcc binutils coreutils gawk gnused
                   gnugrep];
    buildInputs = [];
    system = builtins.currentSystem;
  };
  in
  derivation (defaultAttrs // attrs) ```
```

**hello.nix**


```
let
  pkgs = import <nixpkgs> {};
  mkDerivation = import ./autotools.nix pkgs;
in mkDerivation {
  name = "hello";
  src = ./hello-2.9.tar.gz;
}
```

## NIX find store

```
nix-repl> :l <nixpkgs>
Added 3950 variables.
nix-repl> builtins.toString gnugrep
"/nix/store/g5gdylclfh6d224kqh9sja290pk186xd-gnugrep-2.14"
nix-repl> builtins.toString [ gnugrep gnused ]
"/nix/store/g5gdylclfh6d224kqh9sja290pk186xd-gnugrep-2.14 /nix/store/krgdc4sknzpw8iyk9p20lhqfd52kjmg0-gnused-4.2.2"
```


## Vagrant NIX

```
vagrant init nixos/nixos-16.09-i686
vagrant init nixos/nixos-16.09-x86_64
```

## BASH error output

```
blah blah blah > /dev/null 2>&1
```

## NIX remove all stuff

```
$ nix-channel --update
$ nix-env -u --always
$ rm /nix/var/nix/gcroots/auto/**
$ nix-collect-garbage -d
```

## Setup nix profile

```
cp /nix/var/nix/profiles/default/etc/profile.d/nix.sh ~/nix.sh
echo "export PATH=/nix/var/nix/profiles/default/bin:\$PATH" >> ~/nix.sh
echo "export NIX_USER_PROFILE_DIR=/nix/var/nix/profiles/per-user/\$USER " >>
~/nix.sh

# If under OSX

echo "export
NIX_SSL_CERT_FILE=/nix/var/nix/profiles/default/etc/ssl/certs/ca-bundle.crt" >>
~/nix.sh

source ~/nix.sh

ls -l ~/.nix-profile
lrwxr-xr-x  1 <your_login>  staff  29 15 nov  2016 .nix-profile -> /nix/var/nix/profiles/default

nix-env --switch-profile $NIX_USER_PROFILE_DIR/tuto-jdev

ls -l ~/.nix-profile
lrwxr-xr-x ... .nix-profile -> /nix/var/nix/profiles/per-user/<your-login>/tuto-jdev
```

## Nix build phases

Test using nix-shell --pure ./hello.nix

* echo $out
* echo $PATH
* unpackPhase
* cd $sourceRoot
* patchPhase
* configurePhase
* buildPhase
* installPhase


## nixpkgs

git clone git://github.com/NixOS/nixpkgs.git

alternative to above:

```
cp -a ~/.nix-defexpr/channels/nixpkgs/ .
find . -type d -exec chmod 755 {} \; 
```

## HASKELL & NIX

```
nix-env -i cabal2nix
nix-env -i nix-prefetch-git
nix-env -i cabal-install
```


### NIX-SHELL

```
nix-shell --attr env release0.nix
```


### Starting a cabal project

```
cabal init
 +-> follow the instructions
```


## NIX under the hood

Reference: [youtube](https://www.youtube.com/watch?v=GMQPzv3Sx58&list=PLCFAF6AA590059A23&index=4&t=321s)

### Source code

#### universal interface to nix is string

```
> nix-instantiate example.nix
... is equivalent to ...
> nix-instantiate --expr 'import ./example.nix'
```

#### producing nix expression (consistently)

    - Save as [fetchNixpkgs.nix](https://nixos.wiki/wiki/How_to_fetch_Nixpkgs_with_an_empty_NIX_PATH) in your project direcotory
    - Derive from fetchNixpkgs (example of installing hello.nix)

        ```
        # hello.nix

        let
          fetchNixpkgs = import ./fetchNixpkgs.nix;

          nixpkgs = fetchNixpkgs {
            rev = "76d649b59484607901f0c1b8f737d8376a904019";
            sha256 = "01c2f4mj4ahir0sxk9kxbymg2pki1pc9a3y6r9x6ridry75fzb8h";
          };

          pkgs = import nixpkgs { config = {} }; # config = {} makes it more reliable

        in
          pkgs.hello
        ```

#### debugging nix expression

Evaluating an expression

* nix-instantiate --eval --expr  ( shows you the records )

  ```
  > nix-instantiate --eval --expr 'let pkgs = import <nixpkgs> {}; in pkgs.hello;'
  ```

  This will show the derivation of 'hello' package. (it is derivation _if_ type
  = "derivation")

* nix-repl
* implicitly part of another command (nix-build)

#### Transform nix expression into a derivation "Instantiation"



* nix-instantiate

  ```
  > nix-instantiate --expr 'let pkgs = import <nixpkgs> {}; in pkgs.hello'
  ```

### Derivation (/nix/store/*.drv files)

Language-independent

* path - predefined path
* reproducable
* deterministic

```
> fold /nix/store/xxxx.drv
...
> nix-shell
```

inputSrc -> binary source


Copying derivation
```
> 
```

Consuming a derivation

* nix-store --realise
* nix-store --query
* nix-store --gc


### Build products

Products = Anything in the /nix/store/ that is not _derivation_

* nix-store --realise
* nix-store --add

Transform

> nix-copy-closure --from xxxxx.


### All together

nix-build = nix-instantiate + nix-store --realise


# Haskell compiler

```
nix-env -f "<nixpkgs>" -qaP -A haskell.compiler
```

