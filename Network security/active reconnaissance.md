# Room: Active Reconnaissance  

## Key Concept  

**Active reconnaissance:**  
- Involves **direct connections** to the target machine.  
- These connections may leave logs (client IP, timestamp, duration, etc).  
- Goal: gather info while blending in with normal traffic.  
  - Example: Using a **web browser** — activity appears like normal user browsing.  
- Red team: aims to avoid detection by blue team (defenders).  

**Transport layer behavior:**  
- Connects to:  
  - **TCP port 80** → HTTP (default)  
  - **TCP port 443** → HTTPS (default)  
- Ports 80/443 are hidden in the address bar by default.  
- Custom ports must be explicitly specified.  
  - Example: `https://127.0.0.1:8834/` → connects to localhost on port 8834 over HTTPS.  

**Developer Tools (DevTools):**  
- Shortcut:  
  - Windows/Linux: `Ctrl + Shift + I`  
  - Mac: `Option + Command + I (⌥ + ⌘ + I)`  
- Capabilities:  
  - Inspect & modify JavaScript files.  
  - View cookies set by the server.  
  - Analyze network requests/responses.  
  - Explore folder structure of site content.  

**Useful browser extensions:**  
- **FoxyProxy**  
  - Quickly switch between proxy servers.  
  - Useful when working with **Burp Suite** or rotating proxies.  

- **User-Agent Switcher & Manager**  
  - Spoof browser/OS identity.  
  - Example: Appear as if browsing from an iPhone instead of Firefox.  

- **Wappalyzer**  
  - Detects technologies used on a website (CMS, frameworks, analytics, etc).  
  - Useful for passive tech stack discovery while browsing.  

  ## Tools
  ### Ping
- Tests if the target system is **online & reachable**.  
- Works by sending an **ICMP Echo Request** and expecting an **ICMP Echo Reply**.  
- Useful to verify the target is up before performing deeper scans.  
- Example (Linux):  
  ```bash
  ping -c 5 10.10.231.84
- Example (Windows):
  ```bash
  ping -n 5 10.10.231.84
  If replies are received → target is online.
  ```
- If no replies:
    - Target is off/unresponsive.
    - Network issue or faulty device in path.
    - Firewall blocking ICMP (Windows firewall blocks ping by default).

### Traceroute
- Purpose: Find the path packets take from your system to the target.
- Reveals: 
  - Routers/hops between you and the target.
  - Number of hops in total.
  
#### Syntax
**Linux/macOS:**
- ```bash
  traceroute <IP or HOSTNAME>
  ```

**Windows:**
- ```bash
  tracert <IP or HOSTNAME>
  ```

**How It Works?**
- Uses the TTL (Time To Live) field in IP packets.
- Each router decrements TTL by 1.
- When TTL = 0 → router drops packet & sends back an ICMP Time Exceeded message.
- Traceroute starts with TTL=1, then TTL=2, and so on → reveals each hop.

### Telnet
- Developed in 1969 for remote CLI access.
- Default port: 23.
- Security issue: Sends everything in cleartext (including usernames & passwords).
  Makes it easy for attackers to sniff credentials.
- Secure alternative: SSH (Secure SHell).

**Usage in Reconnaissance:**
- Telnet is a simple TCP client.
- Can be used to connect to any TCP service → useful for banner grabbing.
  Example:
  ```
  telnet <IP> <PORT>
  ```
- After connecting:
  - You can manually interact with the service.
  - If the service is unencrypted, you can exchange messages.

### Netcat (nc)
- Supports TCP and UDP
- Can act as 
  - client -> connects to a port
  - server -> listens on a port

#### Banner grabbing:
- Works similar to Telnet.
- **Command:**
  ```
  nc MACHINE_IP PORT
  ```
- Example (HTTP request):
  ```
  GET / HTTP/1.1
  host: netcat
  ```
- Output may reveal server type and version:
  ```
  Server: nginx/1.6.2
  ```

#### Netcat as a Listener (Server Mode)
- **Command:**
  ```
  nc -vnlp 1234
  ```

**Options:**
- -l: Listen mode
- -p: Specify port number
- -n: Numeric only (no DNS lookup)
- -v: Verbose output
- -vv: Very verbose
- -k: Keep listening after client disconnects

**Notes:**
- -p must appear right before the port number.
- -n avoids DNS lookups.
- Ports <1024 require root privileges.

#### Netcat as a Client
- Command:
  ```
  nc MACHINE_IP PORT
  ```

**Example echo test:**
- On server:
  ```
  nc -vnlp 1234
  ```

- On client:
  ```
  nc MACHINE_IP 1234
  ```

Whatever is typed on one side is echoed on the other.