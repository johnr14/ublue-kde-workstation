# My personal operating system based on Fedora Linux

Be sure to have knowledge of [Silverblue](https://fedoraproject.org/atomic-desktops/silverblue/) or [Kinoite](https://fedoraproject.org/atomic-desktops/kinoite/) before trying this !

This is using [BlueBuild](https://blue-build.org/) and the power of Github !
The current base image is [Bazzite](https://bazzite.gg/) but it could change.

This is my attempt to have a fully descriptive operating system without the hassle of [NixOS](https://nixos.org/).

# Atomic

From [Project Atomic]() 
> a set of packages [[...]] pulled together with [rpm-ostree](https://coreos.github.io/rpm-ostree/) to create a filesystem tree that can be deployed, and updated, as an atomic unit. This means that the entire base OS is updated simultaneously, and (just as with Docker containers) can be rolled back if needed.

# Flavor

This is currently a KDE desktop for my computers (laptop and desktop).
In the future it should contain [Hyprland](https://hyprland.org/). 

# Boot and run

I try to incorporate all I need into the image itself.

# Fork and make your own !

1) Just follow those [instructions](https://blue-build.org/how-to/setup/) to setup Github for automated builds with Actions.
2) Copy what you want from here !

# Recipe

The recipe.yml is an abstraction layer of [Universal Blue](https://universal-blue.org/)'s Containerfiles.

This configuration is split as follow :
- Files (copy)
- Rpm-ostree (packages install)
- Flatpak (flatpak install/remove)
- Systemd (manage services)
- Scripts (run scripts and snippets)
- Chezmoi (fetch your dotfiles and system configuration)
- Fonts (install NerdFonts and GoogleFonts)

TODO:
  [] __Distrobox premade containers__
  [] __Fix my dotfiles repository and enable it here__
  [] __Hardening__
  [] __on_first_run create VMs__
  [] __on_first_run create users and groups__

# After install

I hope you have encrypted your drive !

1) Enroll TPM keys as LUKS
    - `ujust setup-luks-tpm-unlock`
  A) to remove or reinstall : 
    - `sudo sh /usr/libexec/luks-disable-tpm2-autounlock`

2) SecureBoot 
  - `unjust enroll-secure-boot-key`


# TODO :
[] FingerPrint
[ ] Ansible
  [] users
  [] dotfiles
  [] sysconfig
[] Distrobox
  [] Custom images
[] QEMU
  [] Windows
[] Waydroid
  [] Backup/Restore
  [] Install applications


# ublue-kde-workstation

See the [BlueBuild docs](https://blue-build.org/how-to/setup/) for quick setup instructions for setting up your own repository based on this template.

After setup, it is recommended you update this README to describe your custom image.

## Installation

> **Warning**  
> [This is an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable), try at your own discretion.

To rebase an existing atomic Fedora installation to the latest build:

- First rebase to the unsigned image, to get the proper signing keys and policies installed:
  ```
  rpm-ostree rebase ostree-unverified-registry:ghcr.io/johnr14/ublue-kde-workstation:latest
  ```
- Reboot to complete the rebase:
  ```
  systemctl reboot
  ```
- Then rebase to the signed image, like so:
  ```
  rpm-ostree rebase ostree-image-signed:docker://ghcr.io/johnr14/ublue-kde-workstation:latest
  ```
- Reboot again to complete the installation
  ```
  systemctl reboot
  ```

The `latest` tag will automatically point to the latest build. That build will still always use the Fedora version specified in `recipe.yml`, so you won't get accidentally updated to the next major version.

## ISO

This template includes a simple Github Action to build and release an ISO of your image.

To run the action, simply edit the `boot_menu.yml` by changing all the references to startingpoint to your repository. This should trigger the action automatically.

The Action currently uses [ublue-os/isogenerator-old](https://github.com/ublue-os/isogenerator-old) and works in a similar manner to the official Universal Blue ISO. If you have any issues, you should first check [the documentation page on installation](https://universal-blue.org/installation/). The ISO is a netinstaller and should always pull the latest version of your image.

Note that this release-iso action is not a replacement for a full-blown release automation like [release-please](https://github.com/googleapis/release-please).

## Verification

These images are signed with [Sigstore](https://www.sigstore.dev/)'s [cosign](https://github.com/sigstore/cosign). You can verify the signature by downloading the `cosign.pub` file from this repo and running the following command:

```bash
cosign verify --key cosign.pub ghcr.io/blue-build/legacy-template
```
