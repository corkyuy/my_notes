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
