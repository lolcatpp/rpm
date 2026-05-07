# lolcat++ RPM Repository

RPM repository for [lolcat++](https://github.com/lolcatpp/lolcatpp) — a C++ port of lolcat.

Hosted via GitHub Pages at <https://lolcatpp.github.io/rpm/>.

## Supported distributions

| Distribution | Subpath |
|---|---|
| Fedora 41 | `fedora-41` |
| Fedora 42 | `fedora-42` |
| RHEL 9 / Rocky / Alma 9 | `rhel-9` |
| openSUSE Leap 15.6 | `opensuse-leap-15.6` |

Each subpath is its own self-contained RPM repository with `repodata/` and a ready-to-use `lolcatpp.repo` file.

## Installing

### Fedora / RHEL / Rocky / Alma (dnf)

Pick the subpath matching your distro and run:

```bash
# Fedora 42 — change the URL to fedora-41, rhel-9, etc. as needed
sudo dnf config-manager addrepo --from-repofile=https://lolcatpp.github.io/rpm/fedora-42/lolcatpp.repo
sudo dnf install lolcat++
```

(On RHEL 9 / Rocky / Alma you may need `sudo dnf install dnf-plugins-core` first to get `config-manager`.)

### openSUSE Leap (zypper)

```bash
sudo zypper addrepo --gpgcheck https://lolcatpp.github.io/rpm/opensuse-leap-15.6/lolcatpp.repo lolcatpp
sudo rpm --import https://lolcatpp.github.io/rpm/pubkey.gpg
sudo zypper install lolcat++
```

## How packages get here

Packages are built and published automatically by the
[release workflow](https://github.com/lolcatpp/lolcatpp/blob/master/.github/workflows/release.yml)
in the main repository when a new tag is pushed. Each supported distribution
gets its own RPM built in a matching container, then `createrepo_c` regenerates
the metadata for that distro's subdirectory.

The repository metadata (`repomd.xml`) is signed with the GPG key in `pubkey.gpg`.
