# Room: Command Injection

## Key Concept

**Command Injection:**
- Occurs when user input is used to execute system-level commands.
- Commands run with the same privileges as the app (e.g., user `joe`).
- Also known as **Remote Code Execution (RCE)**.
- Common payload: `whoami` — reveals app user.
- Often achieved using shell operators like `;`, `&&`, and `&` to chain commands.

---

**Types of Command Injection:**

| Type    | Description |
|---------|-------------|
| **Blind**   | No output shown; requires behavioural clues (e.g. delay, file creation). |
| **Verbose** | Command output shown in the application (e.g. `whoami` result displayed). |

---

**Detecting Blind Command Injection:**
- No immediate feedback — requires inference.
- Use delay-based payloads:
  - Linux: `ping -c 5 127.0.0.1`, `sleep 5`
  - Windows: `ping -n 5 127.0.0.1`, `timeout 5`
- Use output redirection to write results to a file:
  - Example: `whoami > test.txt` then `cat test.txt`
- Curl-based testing:
  - `curl http://target.site/process.php?search=The%20Beatles;whoami`

---

**Detecting Verbose Command Injection:**
- Easiest to spot.
- Application directly returns command output:
  - e.g., `whoami`, `ls`, `dir`, `ping` etc.

---

**Useful Payloads**

**Linux:**

| Payload | Purpose |
|--------|---------|
| `whoami` | Show current user. |
| `ls` | List directory contents. |
| `ping` | Create delay (blind testing). |
| `sleep` | Create delay (alternative to ping). |
| `nc` | Spawn reverse shell. |

**Windows:**

| Payload | Purpose |
|--------|---------|
| `whoami` | Show current user. |
| `dir` | List directory contents. |
| `ping` | Create delay (blind testing). |
| `timeout` | Create delay (alternative to ping). |


## Tools / commands
### Bypassing filters
- Applications will employ numerous techniques in filtering and sanitising data that is taken from a  user's input. These filters will restrict you to specific payloads; however, we can abuse the logic behind an application to bypass these filters
- an application may strip out quotation marks; we can instead use the hexadecimal value of this to achieve the same result:
    - "\x2f\x65\x74\x63\x2f\x70\x61\x73\x73\x77\x64"

