![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

# Development Workstation Setup with Ansible

## Table of Contents
[[TOC]]

## Overview

This repository contains an Ansible playbook to set up a development workstation with the following tools:

- Initial full system update of all packages from the installation
- Common packages
  - base-devel
  - discord
  - flatpak
  - freecad
  - git
  - jq
  - kicad
  - kicad-library
  - kicad-library-3d
  - kodi
  - obs-studio
  - python-pexpect
  - steam
  - vim
  - vivaldi
  - vivaldi-ffmpeg-codecs
  - yakuake
  - zip
- AUR packages
  - bauh
  - visual-studio-code-bin
- NVIDIA drivers
- SDKMAN and tools that it can install
  - Java 17
  - Gradle
  - Maven
- JetBrains Toolbox
- Balena Etcher
- Gnupg configuration
  - Set up a user systemd service to start the gpg-agent


## Usage

Ensure that everything is backed up before starting this process.  It is a good idea to back up the entire home
directory.  You can use `rsync` to do this.  For example:

```bash
rsync -avh \
  --progress \
  --exclude='/.cache' \
  --exclude='/.config/chromium' \
  --exclude='/.config/google-chrome' \
  --exclude='/.gradle/buildOutputCleanup' \
  --exclude='/.gradle/caches' \
  --exclude='/.gradle/daemon' \
  --exclude='/.gradle/wrapper' \
  --exclude='/.idea' \
  --exclude='/.local/share/Trash' \
  --exclude='/.local/share/akonadi' \
  --exclude='/.local/share/baloo' \
  --exclude='/.local/share/containers' \
  --exclude='/.local/share/flatpak' \
  --exclude='/.local/share/gvfs-metadata' \
  --exclude='/.local/share/Steam' \
  --exclude='/.local/share/Zeal' \
  --exclude='/.local/state' \
  --exclude='/.m2/repository' \
  --exclude='/.mozilla' \
  --exclude='/.npm' \
  --exclude='/.pnpm-store' \
  --exclude='/.vscode' \
  --exclude='/lost+found' \
  --exclude='/node_modules' \
  ~ /mnt/backup-drive/
```

This, of course, assumes that you have something mounted at `/mnt/backup-drive` to accept the backup content.  It would
be even better to set up a cron job to rsync your data to another host on the network so that you always have a backup,
and so that this re-provisioning and setup process is ready to go at any time.

This playbook is designed to be run on a fresh installation of Arch Linux, or one of its derivatives, like Manjaro or
EndeavourOS.  Here is the procedure:

1. Back up all of your data, which includes (but is not limited to) your home directory, as discussed above
2. Create a bootable USB drive with the Arch Linux ISO (or the ISO of your chosen derivative)
3. Boot from the USB drive, and install the OS
4. Log in as the user you created during the installation process
5. Using pacman, install `ansible` and `git`:
    ```bash
    sudo pacman -Syu ansible git
    ```
6. Clone this repository:
    ```bash
    git clone git@github.com:Steve973/devbox-ansible.git
    ```
7. Change into the repository directory:
    ```bash
    cd devbox-ansible
    ```
8. Run the playbook, using -K to prompt for the "become" password:
    ```bash
    ansible-playbook -K playbook.yml
    ```
9. When the playbook is installing NVIDIA drivers, it will prompt you for your sudo password so that it can make changes
10. When the playbook is finished, it will issue a reboot of the system.
11. Log back in, and set up all online accounts, etc.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.