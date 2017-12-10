# Notes
if [ -z ${var+x} ]; then echo "var is unset"; else echo "var is set"; fi ==> check if variable is set in bash

nix ==> nix-channel --add https://nixos.org/channels/nixpkgs-unstable
nix ==> nix-env -q ==> list all installed
nix ==> nix-env -qas ==> status of available package
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

## NIX packages management

```
nix-env -qaP | grep python3-3 --> search nix
nix-env -q  ---> list all installed packages nix
nix-env -i python3-3.3.3
nix-env -iA nixpkgs.python3 --> using attribute name nix
nix-env -uA nixpkgs.python3 --> update nix
nix-env -u --> update all nix
nix-env -e python3-3.3.3 --> uninstall nix
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
    builder = "${bash}/bin/bash";
    args = [ ./builder.sh ];
    baseInputs = [ gnutar gzip gnumake gcc binutils coreutils gawk gnused
                   gnugrep];
    buildInputs = [];
    system = builtins.currentSystem;
  };
  in
  derivation (defaultAttrs // attrs) ```

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


If you are under MACOS, also add:

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
