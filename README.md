# Emerge (artix version)
Little script for compiling source packages and keep them update (Emerge like) on Artix Linux!

## Goals
- [x] PKGBUILD downloading from Artix repos
- [x] Ask to edit PKGBUILD and install automated
- - [x] Keep track of compiled packages & Update them.
- [ ] Have UseFlags

## Installation
Make sure to install `base-devel` and `git` packages.
```
sudo pacman -S --needed git base-devel
git clone https://github.com/notVoid3/emerge-artix.git
cd emerge-artix && chmod +x build update
```
For better implementation I recommend moving the files to `.local/bin` or any PATH environment, and using them with an alias.
```
alias update="update && yay -Syu"

yay() {
  if [[ $1 == "--build" ]]; then
    shift
    build "$@"
  else
    command yay "$@"
  fi
}
```
> This is part of my `zshrc` file, I use an alias to update and a function to integrate build to yay.

## Use & How it works.
- Use `build {package}`. It will clone PKGBUILD to `$HOME/.cache/yay`, will ask for editing PKGBUILD and proceed to compile & install. The package's packager will be custom signed for updates.
  
- Once instalattion is completed without errors, the package name will be write on a log file on HOME (.compiled_packages.log). This file is needed for updates.
  
- **When updating use `update` FIRST**. Otherwise pacman will update the package replacing them with binaries instead of rebuilding them.
  - This scripts checks if all packages in `.compiled_packages.log` are signed by custom Packager's in case you decided to uninstall or replace them with a binary and overwrites the LOG.
  - Then it lists upgradeable packages with `pacman -Syu` and rebuilds them using `build`.
