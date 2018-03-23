# NIX

## FAQ

* Official cheatsheet?

    [Cheatsheet](https://nixos.wiki/wiki/Cheatsheet)
* How to update?  ```
    nix-channel --update
    nix-env -u
    ```
* Where do I find nix channel status?

    [channel status](http://howoldis.herokuapp.com/)
* How to update nix?

    ```
    ```


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

* universal interface to nix is string

```
> nix-instantiate example.nix
... is equivalent to ...
> nix-instantiate --expr 'import ./example.nix'
```

* producing nix expression (consistently)

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


    
    











