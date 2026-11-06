# Part A — Attack

## List of The Top 10:2021
- A01 Broken Access Control
- A02 Cryptographic Failures
- A03 Injection
- A04 Insecure Design
- A05 Security Misconfiguration
- A06 Vulnerable and Outdated Components
- A07 Identification and Authentication Failures
- A08 Software and Data Integrity Failures
- A09 Security Logging and Monitoring Failures
- A10 Server Side Request Forgery (SSRF)

We are tasked with: SQLi, XSS, and command injection


Status: pending. Keep attack implementations, payloads, raw responses, and result
CSVs in this directory. Use the shared `seclab.attack` harness for oracle checks.

For SQLi, reflected XSS, and command injection, record:

- Target version, endpoint, Medium filter configuration, and how to reproduce.
- Exact payload and the case, comment, encoding, or other filter bypass needed.
- Oracle-confirmed effect, trial counts, successes, and benign/negative controls.
- OWASP 2021 mapping; discuss whether Broken Access Control is reachable for at
  least one finding.

For the supplied `vuln-web`, SQLi evidence is a canary leak, the prescribed XSS
check is an unescaped script element, and cmdi reports a simulated shell effect.
Describe those oracle limits precisely. Resolve the missing Medium mode noted
in the [main README](../README.md) before collecting graded evidence.

Suggested CSV fields:
`id,family,endpoint,level,payload,bypass,trials,successes,confirmed,evidence_file`.
Populate rows only from actual experiments and preserve failures as well as
successes.
