# System Tools Deployment

## Overview

This repository is for deploying general purpose system software that is used on HPC targets, in this case gadi@NCI utilising the [build-cd](https://github.com/ACCESS-NRI/build-cd) infrastructure.

## Supported tools

### ncdu
[NCurses Disk Usage (NCDU)](https://dev.yorhel.nl/ncdu) is a disk usage analyzer with a text-mode user interface. It is designed to find space hogs on a remote server where you don’t have an entire graphical setup available, but it is a useful tool even on regular desktop systems. Ncdu aims to be fast, simple, easy to use, and should be able to run on any POSIX-like system.

### gh
[GitHub CLI (gh)](https://cli.github.com) is GitHub on the command line. It brings pull requests, issues, and other GitHub concepts to the terminal next to where you are already working with git and your code.

### openssh 
[OpenSSH](https://www.openssh.com/) is the premier connectivity tool for remote login with the SSH protocol. It encrypts all traffic to eliminate eavesdropping, connection hijacking, and other attacks. In addition, _OpenSSH_ provides a large suite of secure tunneling capabilities, several authentication methods, and sophisticated configuration options.
The OpenSSH suite consists of the following tools:
  - Remote operations are done using `ssh`, `scp`, and `sftp`.
  - Key management with `ssh-add`, `ssh-keysign`, `ssh-keyscan`, and `ssh-keygen`.
  - The service side consists of `sshd`, `sftp-server`, and `ssh-agent`.

> [!WARNING]
> OpenSSH system-tool was added primarily for [git signing on Gadi](https://github.com/ACCESS-NRI/dev-docs/wiki/Git-and-Github#sign-commits-on-gadi).
> For all other use cases, we recommend using Gadi's system OpenSSH distribution, as ACCESS-NRI has not tested or validated this library configuration for security compliance.

### pinentry
[pinentry](https://www.gnupg.org/related_software/pinentry/index.html) is a small collection of dialog programs that allow GnuPG to read passphrases and PIN numbers in a secure manner. 

> [!WARNING]
> pinentry system-tool was added primarily for [mosrs-auth simple password input dialog](https://github.com/ACCESS-NRI/dev-docs/wiki/MOSRS#mosrs-auth-with-a-simple-password-input-dialog).
> For all other use cases, we recommend using Gadi's system pinentry, as ACCESS-NRI has not tested or validated this library configuration for security compliance.

## How to use

**Requirements**: you must be a member of [`vk83`](https://my.nci.org.au/mancini/project/vk83).

The software is accessible through the environment module system on `gadi`, e.g.
```
module use /g/data/vk83/modules
module load system-tools/gh
```
will load the most recent version of `gh`.

To discover what tools and versions are available:
```
module avail system-tools
```
or a specific tool
```
module avail system-tools/ncdu
```
It also works without the `system-tools` namespace:
```
module avail ncdu
```

## Support

This repository and the software deployed from it are supported by ACCESS-NRI.

If you encounter any issues, or would like other tools added to this repository either [make an issue](https://github.com/ACCESS-NRI/system-tools/issues), or request support through the [ACCESS-Hive Forum](https://forum.access-hive.org.au/t/access-help-and-support/908).
