# Nmap Basic Port Scan

## Ports and Services
- An **IP address** identifies a host
- A **TCP/UDP port** identifies a service running on that host
- Examples:
 - HTTP → TCP/80
 - HTTPS → TCP/443
 - DNS → UDP/53
- One port can only be used by **one service** (per IP)

## Firewalls and Port States
Nmap considers **6 possible states** for ports:
1. **Open** – A service is listening on the port
2. **Closed** – No service is listening, but the port is accessible (not blocked)
3. **Filtered** – Nmap cannot determine open/closed; traffic is blocked (e.g., firewall)
4. **Unfiltered** – Port is accessible, but state cannot be determined (e.g., ACK scan)
5. **Open|Filtered** – Could be either; no definitive response
6. **Closed|Filtered** – Could be either; no definitive response

## TCP Header & Flags
TCP header (24 bytes) includes **flags** that Nmap manipulates for scanning.

**Important Flags:**
- **URG** → Urgent pointer significant
- **ACK** → Acknowledgment number significant
- **PSH** → Push data to application immediately
- **RST** → Reset connection
- **SYN** → Synchronize sequence numbers (start connection)
- **FIN** → Finish (no more data)

## TCP Connect Scan (-sT)
- Completes the **3-way handshake** (SYN → SYN/ACK → ACK)
- If port is open → handshake succeeds
- Nmap tears down connection immediately (RST/ACK)
- Works for **unprivileged users** (no root required)
- Example:
 ```bash
 nmap -sT TARGET
 ```
- By default, scans the 1000 most common ports
- Closed ports reply with RST/ACK
- Options:
    -F → Fast mode (100 ports instead of 1000)
    -r → Scan ports sequentially (not random)

## TCP SYN Scan (-sS)
- Default scan mode for privileged users (root/sudo)
- Sends SYN → waits for SYN/ACK
- Sends RST immediately (no full handshake)
- More stealthy (less likely to be logged)
- Faster than connect scan
- Example:
    ```bash
    sudo nmap -sS TARGET
    ```

## UDP Scan (-sU)
- UDP is connectionless (no handshake)
- Open ports → usually no response
- Closed ports → return ICMP port unreachable (Type 3, Code 3)
- Nmap interprets:
    - No response → open or filtered
    - ICMP unreachable → closed
- Slower than TCP scans
- Example:
    ```bash
    sudo nmap -sU TARGET
    ```
- Can be combined with TCP scans:
    - ```bash
        sudo nmap -sS -sU TARGET
        ```


## Port Specification
- Specific ports: -p22,80,443
- Range: -p20-25
- All ports: -p- (1–65535)
- Top N ports: --top-ports 10
- Fast scan: -F (100 most common ports)

## Scan Timing (-T)
Controls speed & stealth:
- -T0 → paranoid (very slow, stealthy)
- -T1 → sneaky
- -T2 → polite
- -T3 → normal (default)
- -T4 → aggressive (common for CTFs)
- -T5 → insane (fast, less reliable)

## Scan Rate & Parallelism
Control packet sending rate:
- --min-rate <num> → minimum packets/sec
- --max-rate <num> → maximum packets/sec

## Control probe parallelism:
- --min-parallelism <num>
- --max-parallelism <num>

## Quick Command Reference
| Scan Type         | Command Example                |
|-------------------|-------------------------------|
| TCP Connect       | `nmap -sT TARGET`            |
| TCP SYN           | `sudo nmap -sS TARGET`       |
| UDP               | `sudo nmap -sU TARGET`       |
| Port List         | `nmap -p22,80,443 TARGET`    |
| Port Range        | `nmap -p20-25 TARGET`        |
| All Ports         | `nmap -p- TARGET`            |
| Top 10 Ports      | `nmap --top-ports 10 TARGET` |
| Fast Scan (100)   | `nmap -F TARGET`             |
| Aggressive Timing | `nmap -T4 TARGET`            |
