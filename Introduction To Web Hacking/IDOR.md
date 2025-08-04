# Room: IDOR 

## Key Concepts
- Insecure direct object reference 
- Type of access control vulnerability
- Can occur when a web server recieves user-supplied input to retrieve objects.
- Too much trust placed on input data with no validation on server to confirm requested object belongs to the user.

**Encoded IDs:**
- Most sites use base64 to encode
- If a site contains a encoded user ID, it can be decoded, tampered with then encoded to be resubmitted to the site
- This could potentially allow you to gain access to another users account

**Hashed IDs:**
- Sites like "https://crackstation.net/" conain a list of billions of hash value results, so if we get a hash we can see if it has a potential value

