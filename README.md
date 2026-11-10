# Orbyn Forge — releases

Orbyn Forge manages Windows and Linux fleets from one control plane: nodes,
agents, durable jobs, audit and a web console. It is free to download and use.

This repository holds the **official builds** only. Each release has:

| File | For |
| --- | --- |
| `forge-<version>-x86_64-linux.tar.gz` | Linux x86_64 with systemd |
| `forge-<version>-aarch64-linux.tar.gz` | Linux arm64 with systemd |
| `forge-<version>-x86_64-windows.zip` | Windows Server 2019+ / Windows 10, 11 |
| `SHA256SUMS`, `SHA256SUMS.sig` | Checksums and their Ed25519 signature |
| raw binaries | Used by Forge's built-in updater |

**→ [Installation guide](INSTALL.md)** · [All releases](https://github.com/Orbyn-Technologies/forge-releases/releases)

Releases marked *Pre-release* are for testing; installed servers only
update themselves to full releases.

## Is it safe?

- Every release is built by Orbyn's CI and its `SHA256SUMS` is signed with the
  Orbyn Forge release key. Public key:

  ```
  bFMJk9wv3XFOhOtPya-idWiatNVVVcktN62sOnQr29U
  ```

- Forge's updater refuses any update whose signature does not match this key,
  so a tampered download cannot be installed through it.
- Download only from this repository's releases page.

Security issues: please report privately to the Orbyn team rather than in a
public issue.
