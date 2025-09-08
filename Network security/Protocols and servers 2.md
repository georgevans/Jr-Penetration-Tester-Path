# Protocols and Servers 2

## Sniffing Attack

- Uses packet capture tools to intercept and analyze network traffic.
- Exploits **cleartext protocols** (e.g., Telnet, FTP, POP3, SMTP).
- Captured data may include:
 - Login credentials.
 - Private messages.
 - Other sensitive info.
- Tools:
 - **tcpdump** → CLI, filter by port, ASCII display (`-A`).
 - **Wireshark** → GUI, filter traffic (e.g., `pop`).
 - **tshark** → CLI alternative to Wireshark.
- Requirements:
 - Network access (wiretap, port mirroring, MITM).
 - Root/admin privileges.
- **Mitigation**: Encrypt traffic with TLS/SSL (HTTPS, FTPS, SMTPS, IMAPS, etc.).

## Man-in-the-Middle (MITM)

- Attacker intercepts communication between victim (A) and server (B).
- Can alter data in transit (e.g., change transaction values).
- Common when browsing over **HTTP** or using cleartext protocols.
- Tools: **Ettercap**, **Bettercap**.
- **Mitigation**:
 - Use TLS (HTTPS, SMTPS, IMAPS, etc.).
 - Validate certificates (PKI, trusted CAs).
 - Ensure proper authentication & integrity checks.

## TLS (Transport Layer Security)

- Successor to SSL; protects confidentiality & integrity.
- Upgrades cleartext protocols (HTTP → HTTPS, POP3 → POP3S, etc.).
- Works at **Presentation Layer** (encrypts data before transmission).
- Default secure ports:
 - HTTP → **443** (HTTPS).
 - FTP → **990** (FTPS).
 - SMTP → **465** (SMTPS).
 - POP3 → **995** (POP3S).
 - IMAP → **993** (IMAPS).
- TLS Handshake (simplified):
 1. ClientHello → capabilities.
 2. ServerHello → parameters + certificate.
 3. Key exchange → secret key agreed.
 4. Both sides switch to encryption.
- **Mitigation benefit**: Prevents sniffing & MITM attacks.

## SSH (Secure Shell)

- Secure alternative to Telnet for remote administration.
- Default port: **22**.
- Provides:
 - Authentication of remote server.
 - Encrypted communication.
 - Integrity protection.
- Authentication methods:
 - Username + password.
 - Public/private key pairs.
- Usage examples:
 - Connect: `ssh user@IP`.
 - Secure file transfer:
   - `scp file.txt user@IP:/path/` (to remote).
   - `scp user@IP:/path/file.txt .` (from remote).
- Related:
 - **SFTP** → FTP over SSH.
 - **FTPS** → FTP over TLS.
- **Mitigation**: Avoids MITM by verifying server fingerprint.

## Password Attacks

- Many protocols require authentication (POP3, SSH, IMAP, etc.).
- Common attack methods:
 - **Guessing** → based on personal info.
 - **Dictionary Attack** → use of leaked/common wordlists.
 - **Brute Force** → try all possible combinations.
- Tools:
 - **Hydra** → supports FTP, POP3, IMAP, SMTP, SSH, HTTP.
 - Syntax:
   - `hydra -l user -P wordlist.txt server service`
- Example:
 - `hydra -l frank -P rockyou.txt 10.10.107.195 ssh`
- Mitigation:
 - Strong password policy.
 - Account lockout after failed attempts.
 - Throttling login attempts.
 - CAPTCHA for GUI logins.
 - Certificates for authentication.
 - Two-Factor Authentication (2FA).