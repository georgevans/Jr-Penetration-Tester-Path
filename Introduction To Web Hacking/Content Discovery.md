# Room: Content Discovery 

## Key Concept

**Favicon:**
- Sometimes when frameworks are used to build a website, a favicon that is part of the installation gets leftover if the website developer doesn't replace this with a custom one
- this can give us a clue on what framework is in use

**Sitemap.xml:**
- gives a list of every file the website owner wishes to be listed on a search engine

**Google Dorking:**
Filters for web search:
- site filter, site:tryhackme.com returns only results from specified web address
- inrul, returns results that have specfied word in url
- filetype
- intitle

**Wappalyzer:**
- helps identify what technologies a site uses

**Wayback Machine:**
- View old versions of websites

## Tools / commands
- "curl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico | md5sum" - Downloads the favicon and gets the md5 hash value to lookup on: https://wiki.owasp.org/index.php/OWASP_favicon_database
- fluff: "ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://MACHINE_IP/FUZZ"
- dirb: "ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://MACHINE_IP/FUZZ"
- Gobuster: "gobuster dir --url http://MACHINE_IP/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt"