# Room: Nmap Live Host Discovery

## Why Live Host Discovery?

- Before port scanning, it's essential to first identify **which hosts are online**
- Scanning offline hosts wastes time and creates unnecessary noise on the network
- Nmap supports multiple techniques for host discovery across different layers of the TCP/IP model

## TCP/IP Layers Used

- **Link Layer**: ARP
- **Network Layer**: ICMP
- **Transport Layer**: TCP, UDP

## Protocols Used in Discovery

### ARP (Address Resolution Protocol)

- Works only on the **same subnet**
- Maps an **IP address to a MAC address**
- Nmap sends ARP requests → live hosts reply with ARP responses
- **Command:**
 - `sudo nmap -PR -sn TARGETS` (ARP scan only)
- Example: `sudo nmap -PR -sn 10.10.210.6/24`
- **Alternative Tool:** `arp-scan`
 - `sudo arp-scan -l` → scans all valid IPs on the local subnet
 - `-I` can specify an interface, e.g., `sudo arp-scan -I eth0 -l`

### ICMP (Internet Control Message Protocol)

**Types commonly used in discovery:**
- Type 8 → Echo Request (ping)
- Type 0 → Echo Reply (pong)
- Type 13 → Timestamp Request
- Type 14 → Timestamp Reply
- Type 17 → Address Mask Request
- Type 18 → Address Mask Reply

#### ICMP Echo Scan

- Sends ICMP echo requests
- Replies indicate live hosts
- Can be blocked by firewalls
- **Command:** `sudo nmap -PE -sn TARGETS`

#### ICMP Timestamp Scan

- Uses ICMP Type 13/14 (Timestamp Request/Reply)
- **Command:** `sudo nmap -PP -sn TARGETS`

#### ICMP Address Mask Scan

- Uses ICMP Type 17/18
- **Command:** `sudo nmap -PM -sn TARGETS`
- Often blocked by firewalls; may return no results

### TCP-Based Discovery

Nmap can send specially crafted TCP packets to check if a host responds.

#### TCP SYN Ping

- Sends a SYN packet to a port (default: 80)
- Open port → SYN/ACK response
- Closed port → RST response
- Either response indicates host is up
- **Command:**
 - `sudo nmap -PS -sn TARGETS` (default port 80)
 - Specify ports:
   - `-PS21` (port 21)
   - `-PS21-25` (range 21–25)
   - `-PS80,443,8080` (list of ports)

#### TCP ACK Ping

- Sends a TCP packet with the ACK flag set
- Host responds with RST if alive
- Default port: 80 (but customizable)
- **Command:**
 - `sudo nmap -PA -sn TARGETS`

### UDP-Based Discovery

- Sends a UDP packet to target ports
- Open UDP port → often **no response**
- Closed UDP port → usually triggers **ICMP Port Unreachable**, confirming host is up
- **Command:**
 - `sudo nmap -PU -sn TARGETS`
 - Example: `sudo nmap -PU53,161,162 -sn TARGETS`

## Nmap Defaults (Host Discovery)

**Privileged users (root/sudo):**
- Local subnet → ARP requests
- External subnet → ICMP echo, TCP ACK (80), TCP SYN (443), ICMP timestamp

**Unprivileged users:**
- Uses full TCP 3-way handshake (SYN → SYN/ACK → ACK) on ports 80 and 443

**By default:** Nmap performs a ping scan first, then proceeds with port scanning.

To perform **host discovery only (no port scan):**
- Add `-sn` to your command

## Masscan (Alternative Tool)

- Very fast scanner, similar to Nmap but optimized for speed
- **Examples:**
 - `masscan MACHINE_IP/24 -p443`
 - `masscan MACHINE_IP/24 -p22-25`
 - `masscan MACHINE_IP/24 --top-ports 100`

## Useful Nmap Options

- `-sn` → Host discovery only (no port scan)
- `-n` → Skip DNS resolution
- `-R` → Reverse-DNS lookup for all hosts

## Summary of Nmap Discovery Scans

| Scan Type | Example Command |
|-----------|----------------|
| **ARP Scan** | `sudo nmap -PR -sn MACHINE_IP/24` |
| **ICMP Echo Scan** | `sudo nmap -PE -sn MACHINE_IP/24` |
| **ICMP Timestamp Scan** | `sudo nmap -PP -sn MACHINE_IP/24` |
| **ICMP Address Mask** | `sudo nmap -PM -sn MACHINE_IP/24` |
| **TCP SYN Ping** | `sudo nmap -PS22,80,443 -sn MACHINE_IP/30` |
| **TCP ACK Ping** | `sudo nmap -PA22,80,443 -sn MACHINE_IP/30` |
| **UDP Ping** | `sudo nmap -PU53,161,162 -sn MACHINE_IP/30` |