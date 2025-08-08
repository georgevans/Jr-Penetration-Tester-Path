# Room: Race conditions

## Key Concept
- Race condition is a situation where the timing of events influences the behaviour and outcome of the program
- Typically when a variable gets accessed and modified by multiple threads
- Often due to lack of proper lock mechanisms and synchronization between different threads.


## Tools / commands

### Burp Suite
#### Tool - Repeater (Race Condition Testing)
- Used to manually resend HTTP requests to test for vulnerabilities.
- To exploit a race condition:
  1. Capture a request that triggers a sensitive action (e.g. transfer, booking).
  2. Send it to Repeater.
  3. Quickly send multiple copies of the same request to test for inconsistent behavior (e.g. duplicate processing or bypassing limits).
- Look for signs like double charges, duplicated responses, or unexpected success.

