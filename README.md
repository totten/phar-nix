# pharnix

Some nix packages that are useful for building PHARs.

## Example: Full profile

Create a `shell.nix` like this:

```nix
{ pkgs ? import <nixpkgs> {} }:

let

  pharnix = pkgs.callPackage (pkgs.fetchFromGitHub {
    owner = "totten";
    repo = "pharnix";
    rev = "v0.2.0";
    sha256 = "fixme";
  }) {};

in

  pkgs.mkShell {
    nativeBuildInputs = pharnix.profiles.full ++ [
      pkgs.bash-completion
    ];
    shellHook = ''
      source ${pkgs.bash-completion}/etc/profile.d/bash_completion.sh
    '';
  }
```

## Example: Pick and choose

Create a `shell.nix` like this:

```nix
{ pkgs ? import <nixpkgs> {} }:

let

  pharnix = pkgs.callPackage (pkgs.fetchFromGitHub {
    owner = "totten";
    repo = "pharnix";
    rev = "v0.2.0";
    sha256 = "fixme";
  }) {};

in

  pkgs.mkShell {
    nativeBuildInputs = [
      pharnix.php
      pharnix.lib.composer
      pharnix.lib.box
      pharnix.lib.phpunit8
      pkgs.bash-completion
      pkgs.gh
    ];
    shellHook = ''
      source ${pkgs.bash-completion}/etc/profile.d/bash_completion.sh
    '';
  }
```

## Properties

* General
    * `pharnix.php` (*preferred php interpreter*)
    * `pharnix.pkgs` (*the pinned release of nixpkgs*)
* Library (*Random PHARs needed for building things*)
    * `pharnix.lib.box`
    * `pharnix.lib.composer`
    * `pharnix.lib.phive`
    * `pharnix.lib.phpunit8`
    * `pharnix.lib.phpunit9`
    * `pharnix.lib.pogo`
* Profiles
    * `pharnix.profiles.base` (*some non-PHP packages that form typical unix-style workspace*)
    * `pharnix.profiles.full` (*the `base` plus everything *)
