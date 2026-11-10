# Installing Orbyn Forge

Each package is self-contained: the server with the web console built in,
the plugin runner, the agent binaries your nodes download, an installer and
an example configuration. No Node.js, database server or web server needed —
Forge uses an embedded SQLite database.

Pick the latest version on the
[releases page](https://github.com/Orbyn-Technologies/forge-releases/releases)
and replace `<version>` below with it (for example `0.1.0`).

## Linux (systemd: Debian, Ubuntu, RHEL, Rocky, Alma, …)

```sh
V=<version>
curl -fLO https://github.com/Orbyn-Technologies/forge-releases/releases/download/v$V/forge-$V-x86_64-linux.tar.gz
curl -fLO https://github.com/Orbyn-Technologies/forge-releases/releases/download/v$V/SHA256SUMS
sha256sum -c SHA256SUMS --ignore-missing
tar xzf forge-$V-x86_64-linux.tar.gz
cd forge-$V-x86_64-linux
sudo ./install.sh --public-url http://forge.example.com:8080 --admin-email you@example.com
```

On arm64 use `aarch64` instead of `x86_64`. The installer asks for the
administrator password (or reads `FORGE_ADMIN_PASSWORD`).

| What | Where |
| --- | --- |
| Binaries | `/opt/forge` (agent binaries in `/opt/forge/dist`) |
| Configuration | `/etc/forge/forge.toml` |
| Data | `/var/lib/forge` |
| Service | `forge-server.service` — logs: `journalctl -u forge-server -f` |
| Firewall | opens TCP 8080 and 7443 (firewalld / ufw) |

Options: `--public-url`, `--admin-email` (first install only),
`--agent-host` (name or IP nodes use to reach the server), `--prefix`.

## Windows

In **PowerShell as Administrator**:

```powershell
$V = "<version>"
Invoke-WebRequest "https://github.com/Orbyn-Technologies/forge-releases/releases/download/v$V/forge-$V-x86_64-windows.zip" -OutFile "forge-$V-x86_64-windows.zip"
Get-FileHash "forge-$V-x86_64-windows.zip" -Algorithm SHA256   # compare with SHA256SUMS
Expand-Archive "forge-$V-x86_64-windows.zip" -DestinationPath .
cd "forge-$V-x86_64-windows"
.\install.ps1 -PublicUrl http://forge.example.com:8080 -AdminEmail you@example.com
```

| What | Where |
| --- | --- |
| Binaries | `C:\Program Files\Orbyn Forge` |
| Configuration and data | `C:\ProgramData\Orbyn Forge` |
| Startup | scheduled task **Orbyn Forge** (SYSTEM, at boot) |
| Firewall | inbound TCP 8080 and 7443 |

## After installing

1. Open the public URL and sign in as the administrator.
2. For production, put a TLS proxy (Caddy, Nginx, IIS) in front of port 8080,
   set `public_url = "https://…"` and `cookie_secure = true`, then restart.
3. Allow TCP **7443** from your nodes to the server (agents connect out over
   mutual TLS).
4. Add nodes from **Nodes → Add node**.

## Updating

Administrators see **Update to x.y.z** in the console when a new release is
out. Forge downloads it, checks every file against `SHA256SUMS` and its
signature, backs up the database, installs and restarts; if the new version
fails to start it rolls back. To update by hand, run the installer from a
newer package — configuration and data are kept.

## Uninstalling

- Linux: `sudo systemctl disable --now forge-server`, then remove `/opt/forge`,
  `/etc/forge` and (after backing up) `/var/lib/forge`.
- Windows: `Unregister-ScheduledTask -TaskName "Orbyn Forge"`, then remove
  `C:\Program Files\Orbyn Forge` and (after backing up) `C:\ProgramData\Orbyn Forge`.
