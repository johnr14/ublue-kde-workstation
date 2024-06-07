# Custom modification

Image is set to rawhide from original Fedora Kinoite.

I added most applicastions I used and wanted available on my system. For the rest, using toolbox/distrobox is ok.
Also, trying to add hyprland along side KDE so it can be choosen.

#TODO
- upfwd
    ```
    # WARNING: Do this before secureboot and luks-tpm or it will break them
    sudo fwupdmgr refresh --force && \
    sudo fwupdmgr get-updates && \
    sudo fwupdmgr update
    ```

[X] ~~Enroll TPM keys as LUKS~~ with `ujust setup-luks-tpm-unlock`
  [x] to remove or reinstall : `sudo sh /usr/libexec/luks-disable-tpm2-autounlock`
[X] SecureBoot `unjust enroll-secure-boot-key` ?
[] systemd-boot 
[] FingerPrint
[] fstrim
  [x] working
  [] check timer
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
