# Room: The basics

## Key concepts
- **Purpose**: Burp Suite is a Java-based framework for web application penetration testing, considered the industry standard.
- **Scope**: Suitable for testing web, mobile, and API-based applications.
- **Core Function**: Intercepts and manipulates HTTP/HTTPS traffic between the browser and server.
- **Traffic Handling**: Requests can be routed to different Burp Suite components for detailed analysis.
- **Testing Advantage**:
  - View and modify requests before they reach the target server.
  - Alter responses before they reach the browser.
  - Enables detailed, manual testing of application behavior and vulnerabilities.

## Core Tools in Burp Suite Community

- **Proxy**:  
  - Most renowned feature of Burp Suite.  
  - Intercepts and modifies requests/responses while interacting with web apps.

- **Repeater**:  
  - Captures, modifies, and resends the same request multiple times.  
  - Useful for payload crafting (e.g., SQLi) or testing endpoint vulnerabilities.

- **Intruder**:  
  - Sends multiple requests to endpoints (rate-limited in Community edition).  
  - Commonly used for brute-force attacks or fuzzing.

- **Decoder**:  
  - Encodes/decodes data.  
  - Useful for transforming captured information or preparing payloads within Burp.

- **Comparer**:  
  - Compares two pieces of data (word or byte level).  
  - Can handle large data segments quickly via keyboard shortcut.

- **Sequencer**:  
  - Tests randomness of tokens (e.g., session cookies).  
  - Detects insecure random value generation that could lead to attacks.

