# MR.ROBOT OS

A lightweight Debian 13 live-build project with a local, full-screen MR.ROBOT-inspired interface. This project is based on the supplied Horizon OS source tree, with its account backend, Supabase configuration, account website, social features and game/Steam integration removed.

## Standalone by design

- No user account or online login is required to boot or use the local interface.
- No Supabase credentials or external account site are used.
- The launcher opens a local file (`/opt/mrrobot/index.html`); it does not call a remote web app.
- Internet access is optional at runtime. The ISO build does need internet to download Debian packages and build tools.
- The menu currently provides a Flipper-inspired local shell and links to local system functions. The separate MR.ROBOT Python OSINT application is not yet bundled into this source archive; it needs a follow-up integration step.

## Build ISO with GitHub Actions

1. Extract this ZIP and upload the **contents** of the `MR-ROBOT-OS` folder to the root of your repository (including the hidden `.github` directory).
2. Open **Actions → Build MR.ROBOT OS ISO → Run workflow**.
3. When the workflow succeeds, download the `MR-ROBOT-OS-amd64` artifact and extract the ISO.

The workflow requires no repository secrets. It uses GitHub Actions to install Debian live-build tools and create a hybrid ISO. The ISO has not been built or boot-tested in this environment, so review the workflow log and test the artifact in VirtualBox before relying on it.

## Local build

On a Debian 13 build host, install `live-build debootstrap squashfs-tools xorriso isolinux syslinux-common gettext po4a debian-archive-keyring`, then run:

```sh
cd os
chmod +x auto/config config/hooks/live/*.hook.chroot
sudo lb clean --purge || true
sudo ./auto/config
sudo lb build
```

The resulting hybrid ISO should be in the `os` directory.
