# Room: File Inclusion 

## Key Concepts
- Often found in PHP code that is poorly written and implemented
- Often due to lack of input sanitization or validation
- Can be used to leak data or creds
- If attacker can write files to server, they could gain remote command execution (RCE)

**Path traversal:**
- allows attacker to read operating system resources

**Remote file inclusion (RFI):**
- technique to include remote files into a vulnerable application
- occurs when inputs are improperly sanitizing user input, allowing an attacker to inject an external URL into include function.
- One requirement for RFI is that the allow_url_fopen option needs to be on
- Higher risk compared to LFI as attacker can gain RCE on the server

## Tools / commands
### Common OS files 
- /etc/issue: a message or system identification to be printed before the login prompt
- /etc/profile: controls system-wide default variables, such as Export variables, File creation mask (umask), Terminal types, Mail messages to indicate when new mail has arrived
- /proc/version: specifies the version of the Linux kernel
- /etc/passwd: has all registered users that have access to a system
- /etc/shadow: contains information about the system's users' passwords
- /root/.bash_history: contains the history commands for root user

#### Command 