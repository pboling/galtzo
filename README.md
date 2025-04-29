# galtzo &nbsp; [![bluebuild build badge](https://github.com/pboling/galtzo/actions/workflows/build.yml/badge.svg)](https://github.com/pboling/galtzo/actions/workflows/build.yml)

See the [BlueBuild docs](https://blue-build.org/how-to/setup/) for quick setup instructions for setting up your own repository based on this template.

This image is based on `aurora-dx-hwe:latest`.  A new version / build is released daily.

The linux lineage of this spin therefore looks something like this:

```mermaid
mindmap
  root((Linux))
    Red Hat
      Fedora CoreOS
        Fedora Silverblue
          Universal Blue
            Bluefin
              Aurora
                Aurora-DX
                  Aurora-DX-HWE
                    Galtzo <=
    Debian
      Ubuntu
    SLS
      Slackware
    Jurix
      SuSE
    Enoch
      Gentoo
    Arch
```

If you are unfamiliar with Universal Blue Linux, or Atomic Fedora,
start your journey at [universal-blue.org](https://universal-blue.org/).

If you want to jump in (the water is fine!) start with the [Blue Build Workshop](https://workshop.blue-build.org/),
a Web tool that will create a repo like this one and build your first image.

This particular configuration layers the following onto `aurora-dx-hwe`:

- NordVPN (also added to systemd) (config taken from [jlandahl/aurora](https://github.com/jlandahl/aurora))
- 1Password
- Ruby build dependencies (fedora specific)
  - autoconf
  - gcc
  - rust
  - patch
  - make
  - bzip2
  - openssl-devel
  - libyaml-devel
  - libffi-devel
  - readline-devel
  - gdbm-devel
  - ncurses-devel
  - perl-FindBin # Because of OpenSSL!

## Installation

| ⚠️ **Warning**️ | [This is an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable), try at your own discretion. |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------|

To rebase an existing atomic Fedora installation to the latest build:

- First rebase to the unsigned image, to get the proper signing keys and policies installed:
  ```
  rpm-ostree rebase ostree-unverified-registry:ghcr.io/pboling/galtzo:latest
  ```
- Reboot to complete the rebase:
  ```
  systemctl reboot
  ```
- Then rebase to the signed image, like so:
  ```
  rpm-ostree rebase ostree-image-signed:docker://ghcr.io/pboling/galtzo:latest
  ```
- Reboot again to complete the installation
  ```
  systemctl reboot
  ```

The `latest` tag will automatically point to the latest build. That build will still always use the Fedora version specified in `recipe.yml`, so you won't get accidentally updated to the next major version.

## ISO

If build on Fedora Atomic, you can generate an offline ISO with the instructions available [here](https://blue-build.org/learn/universal-blue/#fresh-install-from-an-iso). These ISOs cannot unfortunately be distributed on GitHub for free due to large sizes, so for public projects something else has to be used for hosting.

## Verification

These images are signed with [Sigstore](https://www.sigstore.dev/)'s [cosign](https://github.com/sigstore/cosign). You can verify the signature by downloading the `cosign.pub` file from this repo and running the following command:

```bash
cosign verify --key cosign.pub ghcr.io/pboling/galtzo
```
