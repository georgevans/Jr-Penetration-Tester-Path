# Room: Protocols and Servers

## Telnet

- Application layer protocol for remote terminal access.
- Default port: **23**.
- Workflow:
 1. Connect → prompt for username.
 2. Prompt for password (hidden when typed).
 3. On success, gain shell access (non-root unless specified).
- **Security Issue**:
 - Communication is **cleartext** (including usernames/passwords).
 - Easy for attackers to sniff traffic and recover credentials.
- **Status**: Deprecated for remote administration.
- **Secure Alternative**: SSH.

## HTTP

- Protocol used for transferring web content (HTML, images, forms, files).
- Default port: **80**.
- Workflow example:
 - Client requests `/index.html`.
 - Server responds with requested page.
- **Key points**:
 - Data is **cleartext**.
 - Can use `telnet <IP> 80` or `nc <IP> 80` to manually send HTTP requests.
 - Example request:
    ```bash
    GET /index.html HTTP/1.1
    ```
- Common Web Servers:
  - Apache (open-source).
  - Nginx (open-source).
  - IIS (Microsoft, closed-source).
- Common Browsers: Chrome, Edge, Firefox, Safari.

## FTP

- File Transfer Protocol, used for transferring files between systems.
- Default ports: **21** (control), **20** (active mode data).
- Communication is **cleartext**.
- Basic commands (via Telnet/Netcat or FTP client):
  - `USER <username>` → send username.
  - `PASS <password>` → send password.
  - `STAT` → status info.
  - `SYST` → system type.
  - `PASV` → passive mode.
  - `TYPE A` (ASCII), `TYPE I` (binary).
- Two Modes:
  - **Active Mode**: server initiates data connection from port 20.
  - **Passive Mode**: client initiates data connection (port > 1023).
- FTP software:
  - Servers: vsftpd, ProFTPD, uFTP.
  - Clients: FileZilla, Linux `ftp` command, some browsers.
- **Security Issue**: Sends credentials/files in cleartext.

## SMTP (Simple Mail Transfer Protocol)

- Used to **send emails**.
- Default port: **25**.
- Communicates with the **Mail Transfer Agent (MTA)**.
- Commands (via Telnet):
  - `helo <hostname>` → identify client.
  - `mail from:` → sender.
  - `rcpt to:` → recipient.
  - `data` → begin email content. End with `.` on its own line.
- **Security Issue**: Cleartext communication.
- Workflow Components:
  - **MUA** (Mail User Agent) → email client (e.g., Outlook, Thunderbird).
  - **MSA** (Mail Submission Agent) → validates outgoing email.
  - **MTA** (Mail Transfer Agent) → routes emails between servers.
  - **MDA** (Mail Delivery Agent) → stores email for recipient.

## POP3 (Post Office Protocol v3)

- Used to **download emails** from the server (MDA).
- Default port: **110**.
- Workflow:
  - Authenticate (`USER <username>` / `PASS <password>`).
  - `STAT` → shows number/size of messages.
  - `LIST` → lists available messages.
  - `RETR <id>` → retrieve message.
  - `QUIT` → close connection.
- **Default behavior**: Deletes messages after download (can be changed).
- **Limitation**: Difficult to sync emails across multiple devices.
- **Security Issue**: Cleartext communication → credentials easily intercepted.

## IMAP (Internet Message Access Protocol)

- Alternative to POP3.
- Default port: **143**.
- **Advantages over POP3**:
  - Messages remain on the server.
  - Allows synchronization across multiple clients/devices.
  - Supports advanced features like folders and message states (read/unread).
- **Security Issue**: Like POP3/SMTP, unencrypted by default.