# Part A — Attack

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
