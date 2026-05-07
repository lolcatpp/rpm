# lolcat++ RPM Repository

RPM repository for [lolcat++](https://github.com/lolcatpp/lolcatpp) — a C++ port of lolcat.

Hosted via GitHub Pages at <https://lolcatpp.github.io/rpm/>.

## Supported distributions

| Distribution | Subpath | Architectures |
|---|---|---|
| Fedora 43 | `fedora-43` | x86_64, aarch64 |
| Fedora 44 | `fedora-44` | x86_64, aarch64 |
| RHEL 9 / Rocky 9 / Alma 9 | `rhel-9` | x86_64, aarch64 |
| RHEL 10 / Rocky 10 / Alma 10 | `rhel-10` | x86_64, aarch64 |
| openSUSE Leap 16.0 | `opensuse-leap-16.0` | x86_64, aarch64 |

Each subpath is its own self-contained RPM repository with `repodata/` and a ready-to-use `lolcatpp.repo` file. The `.repo` file uses `$releasever` so once added it will follow you across distro upgrades within a family (e.g. Fedora 43 → 44).

## Installing

### Fedora / RHEL / Rocky / Alma (dnf)

```bash
sudo dnf install -y dnf-plugins-core   # only needed on RHEL/Rocky/Alma
sudo dnf config-manager addrepo --from-repofile=https://lolcatpp.github.io/rpm/lolcatpp.repo
sudo dnf install -y lolcat++
```

The single `.repo` file uses `$ID` and `$releasever` so it resolves to the right subpath on every supported distro. It will also follow you across distro upgrades within a family (e.g. Fedora 43 → 44).

### openSUSE Leap (zypper)

zypper doesn't expand `$ID`, so the URL has to name the version explicitly:

```bash
. /etc/os-release
sudo zypper addrepo --gpgcheck \
  "https://lolcatpp.github.io/rpm/opensuse-leap-${VERSION_ID}/lolcatpp.repo" \
  lolcatpp
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
