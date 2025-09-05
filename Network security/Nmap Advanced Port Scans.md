# Room: Nmap Advanced Port Scans

## Null Scan (-sN)

- Sends **no flags** in TCP packet
- **Open port** → no response
- **Closed port** → responds with **RST**
- Results: `open|filtered` (cannot confirm if blocked by firewall)
- Effective against **stateless firewalls**
- Requires **sudo/root**

## FIN Scan (-sF)

- Sends packet with **FIN flag set**
- **Open port** → no response
- **Closed port** → responds with **RST**
- Results: `open|filtered`
- Similar to Null scan, bypasses some firewalls

## Xmas Scan (-sX)

- Sets **FIN + PSH + URG** flags (like Xmas lights)
- **Open port** → no response
- **Closed port** → responds with **RST**
- Results: `open|filtered`
- Works against **stateless firewalls**, useless against **stateful firewalls**

## TCP Maimon Scan (-sM)

- Sets **FIN + ACK** flags
- Some BSD systems → drop packet on **open ports** (reveals them)
- Most modern systems → always reply **RST**, making it ineffective
- Rarely useful, but important historically

## TCP ACK Scan (-sA)

- Sends packet with **ACK flag set**
- Always responds with **RST** → cannot find open ports
- Used for **firewall rule discovery**:
 - **Unfiltered** → response received
 - **Filtered** → no response

## TCP Window Scan (-sW)

- Similar to ACK scan, but checks **TCP Window field** in RST replies
- Some systems reveal difference between open/closed ports
- Useful for **firewall analysis**, not service discovery

## Custom Flag Scans (--scanflags)

- Allows creating **custom TCP flag combinations**
- Example: `--scanflags RSTSYNFIN`
- Used for **bypassing firewalls** or testing **IDS reactions**

## Spoofing & Decoys

### IP Spoofing (-S)

- Sends packets with a **fake source IP**
- Only useful if attacker can **see responses** (same subnet)
- Needs `-e` (interface) + `-Pn` (disable ping)

### MAC Spoofing (--spoof-mac)

- Changes **source MAC address**
- Only works on **same Ethernet/WiFi network**

### Decoy Scan (-D)

- Makes scan appear from **multiple IPs**
- Example:
 - `nmap -D 10.10.0.1,10.10.0.2,ME target`
 - Attacker's IP hidden among decoys

## Firewall & IDS

- **Firewall**: Blocks/allows packets based on **rules** (IP + port)
- **IDS**: Inspects **headers + payloads**, detects suspicious patterns

## Evasion Techniques

- **Fragmentation** (`-f`, `--mtu`) → split packets into small pieces
- **Packet padding** (`--data-length NUM`) → make traffic look normal

### Fragmented Packets

- `-f` → split into **8-byte fragments**
- `-ff` → split into **16-byte fragments**
- `--mtu NUM` → set custom multiple of 8
- Example:
 ```bash
 nmap -sS -p80 -f target
 ```

## Idle Scan (Zombie Scan) (-sI)
- Uses an idle host (zombie) to hide attacker's IP
- Relies on IP ID field in zombie's responses
- Steps:
    1. Record zombie's current IP ID
    2. Send spoofed probe (appearing from zombie) to target
    3. Check if zombie's IP ID increments (reveals open port)
- Results:
    - Difference = 1 → closed/filtered
    - Difference = 2 → open
- Must use a truly idle system (e.g., printer)

## Verbose & Debug Options
--reason → shows why Nmap reached conclusion (e.g., syn-ack)
-v / -vv → verbose output
-d / -dd → debugging details (very verbose)

# Summary
- Null, FIN, Xmas scans → good for bypassing stateless firewalls
- Maimon scan → mostly obsolete, educational
- ACK & Window scans → map firewall rules, not services
- IP/MAC spoofing & decoys → hide source of scan
- Fragmentation & padding → evade IDS/firewalls
- Idle scan → stealthy, but requires idle zombie host
- --reason, -v, -d → improve output detail