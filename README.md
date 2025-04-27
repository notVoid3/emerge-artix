# Emerge (artix version)
Little script for compiling source packages and keep them update (Emerge like) on Artix Linux!

## Goals
- [x] PKGBUILD downloading from Artix repos
- [x] Ask to edit PKGBUILD and install automated
- - [x] Keep track of compiled packages & Update them.
- [ ] Have UseFlags

## Use & How it works.
**Install `base-devel` and `git` packages.** I recommend moving the files to `.local/bin` or any PATH environment, and/or using them with an alias: 
> I'm using `yay --build` for build and `update && yay -Syu` for update both compiled, binaries and AUR. You can choose different aliases.

- Use `build {package}`. It will clone PKGBUILD to `$HOME/.cache/yay`, will ask for editing PKGBUILD and proceed to compile & install. The package's packager will be custom signed for updates.
  
- Once instalattion is completed without errors, the package name will be write on a log file on HOME (.compiled_packages.log). This file is needed for updates.
  
- **When updating use `update` FIRST**. Otherwise pacman will update the package replacing them with binaries instead of rebuilding them.
  - This scripts checks if all packages in `.compiled_packages.log` are signed by custom Packager's in case you decided to uninstall or replace them with a binary and overwrites the LOG.
  - Then it lists upgradeable packages with `pacman -Syu` and rebuilds them using `build`.
