# Room: Passive reconnaissance 

## Key Concept
**Passive reconnaissance:**
- rely on publicly available knowledge
- includes many activities, for instance:
    - Looking up DNS records of a domain from a public DNS server
    - Reading news articles about the target company

**Active reconnaissance:**
- requires direct engagement with the target
- Examples of active reconnaissance activities include:
    - Connecting to one of the company servers such as HTTP, FTP, and SMTP
    - Calling the company in an attempt to get information
    - Entering company premises pretending to be a repairman

**Whois:**
- a request and response protocol that follows the RFC 3912 specification
- A WHOIS server listens on TCP port 43 for incoming requests
- Using the whois server we can learn:
    - Registrar: Via which registrar was the domain name registered?
    - Contact info of registrant: Name, organization, address, phone, among other things
    - Creation, update, and expiration dates: When was the domain name first registered? When was it last updated? And when does it need to be renewed?
    - Name Server: Which server to ask to resolve the domain name?

**Subdomain discovery:**
- `nslookup` and `dig` cannot find subdomains on their own.
- Subdomains (e.g., `wiki.example.com`, `webmail.example.com`) can reveal sensitive information or expose outdated/vulnerable services.
- Discovery methods:
  - Using multiple search engines to collect publicly known subdomains (requires digging deep into results).
  - Brute-forcing DNS queries to enumerate existing subdomains.
  - Using online services like **DNSDumpster** for detailed DNS information.

**DNSDumpster:**
- Online service for discovering subdomains and DNS records.
- Provides results in tables and visual graphs.
- Can return:
  - Subdomains (e.g., `blog.tryhackme.com`)
  - DNS servers
  - IP addresses and geolocation
  - MX records with IPs, owner, and location
  - TXT records
- Efficient: a single query can retrieve a wide set of DNS information.

**Shodan.io:**
- A search engine for internet-connected devices (not just websites).  
- Collects banners and metadata from exposed services when they respond.  
- Useful for passive reconnaissance without directly connecting to the target.  
- Can reveal information such as:  
  - IP address  
  - Hosting company  
  - Geographic location  
  - Server type and version  
- Defensive use: organizations can monitor their own exposed assets.  


## Tools / commands
**whois**  
- Usage: `whois DOMAIN_NAME`  
- Reveals registrar info, contact info, registration dates, and name servers.

**nslookup**  
- Usage: `nslookup -type=TYPE DOMAIN_NAME SERVER`  
- Common types:
  - `A` → IPv4 address  
  - `AAAA` → IPv6 address  
  - `MX` → Mail servers  
  - `CNAME` → Canonical name  
  - `TXT` → Text records (e.g., SPF, DKIM, security policies)  
- Examples:
    - ```bash
        nslookup -type=A tryhackme.com              # Lookup A records
        nslookup -type=MX tryhackme.com 1.1.1.1     # Lookup MX records via Cloudflare DNS
        nslookup -type=TXT tryhackme.com            # Lookup TXT records
        ```

**dig**  
- More advanced DNS querying tool.  
- Usage: `dig @SERVER DOMAIN_NAME TYPE`  
- Provides detailed info including TTL values.  
- Examples:
    - ```bash
    dig tryhackme.com A              # Lookup A records
    dig @1.1.1.1 tryhackme.com MX    # Lookup MX records via Cloudflare DNS
    dig tryhackme.com TXT            # Lookup TXT records
    ```

