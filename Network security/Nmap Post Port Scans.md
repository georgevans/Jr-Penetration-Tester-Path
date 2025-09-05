# Room: Nmap Post Port Scans

# Nmap Post Port Scan Notes

## Service Detection

- After finding open ports, Nmap can probe them to identify running services and versions.
- Useful for finding known vulnerabilities in services.
- Use `-sV` to enable service/version detection.
  - Intensity can be set with `--version-intensity LEVEL` (0–9).
  - `--version-light` = 2, `--version-all` = 9.
- `-sV` forces a full TCP 3-way handshake (stealth SYN `-sS` not possible with `-sV`).
- Example:
  - `22/tcp open ssh` → guessed.
  - `22/tcp open ssh OpenSSH 6.7p1 Debian ...` → confirmed via banner grabbing.

## OS Detection

- Use `-O` (uppercase O) for OS detection.
- Relies on responses and behavior to guess OS and version.
- Needs at least one open and one closed port for accuracy.
- Accuracy can be impacted by:
  - Virtualization.
  - Fingerprint limitations.
- Example: Linux 3.13 guessed, but actual was 3.16.

## Traceroute

- Add `--traceroute` to map hops between scanner and target.
- Works differently from normal traceroute:
  - Standard: starts with low TTL and increases.
  - Nmap: starts with high TTL and decreases.
- Some routers block ICMP Time-to-Live exceeded messages → may not show all hops.

## Nmap Scripting Engine (NSE)

- Nmap includes ~600+ scripts (in Lua).
- Located under `/usr/share/nmap/scripts/`.
- Script categories include:
  - **auth** → authentication checks.
  - **broadcast** → host discovery via broadcasts.
  - **brute** → brute-force password auditing.
  - **default** → safe default scripts (`-sC`).
  - **discovery** → gather info like DB tables, DNS names.
  - **dos** → DoS vulnerabilities.
  - **exploit** → attempt exploits.
  - **external** → third-party checks (e.g., Virustotal).
  - **fuzzer** → fuzzing attacks.
  - **intrusive** → intrusive scans (e.g., brute-force).
  - **malware** → detect backdoors.
  - **safe** → non-intrusive, won't crash services.
  - **version** → retrieve service versions.
  - **vuln** → check for known vulnerabilities.
- Can run:
  - Default scripts: `-sC` or `--script=default`.
  - Specific: `--script "script-name"`.
  - Pattern: `--script "ftp*"` (runs all ftp-related).
- Example:
  - `--script "http-date"` → retrieves server's HTTP date/time.
- Caution: some scripts are intrusive and may crash/exploit targets.

## Saving Output

- Always save scan results. Use consistent naming conventions.
- Formats:
  1. **Normal** (`-oN`) → human-readable, similar to console output.
  2. **Grepable** (`-oG`) → optimized for `grep`. Each line is self-contained.
  3. **XML** (`-oX`) → structured, for parsing with other tools.
  4. **All** (`-oA`) → saves in normal, grepable, and XML.
- Example:
  - ```bash
    nmap -sS -sV -O -oA target_scan 10.10.132.219
