# Room: SQL Injection

## Key Concept
**In-Band SQL Injection**  
Uses the same channel to exploit and receive results. Easiest to detect and exploit.

**Error-Based SQL Injection**  
  Relies on database error messages being displayed on the page.  
  Useful for gathering information about the database structure.  
  Triggered by breaking queries with characters like `'` or `"`.

**Union-Based SQL Injection**  
  Uses the `UNION` SQL operator with `SELECT` to append extra results.  
  Common for extracting large amounts of data directly to the page.

