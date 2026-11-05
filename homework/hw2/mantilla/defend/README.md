# Part B — Defend, then bypass

Status: pending. Choose one Part-A vulnerability and implement both defenses
here, including any target adaptation needed to exercise the real sink.

## B1 — WAF blocklist

Record the rules, where the WAF is applied, and coverage on the Part-A payloads.
Find at least three same-intent payloads that pass the WAF and check their effects
with the attack oracle. Distinguish a filter evasion from an exploit that works.

Measure false positives on [benign_traffic.json](benign_traffic.json), copied
unchanged from `../duel-2-web/benign_traffic.json` relative to the submission root.
This instructor corpus contains 26 inputs grouped by login username, search/name,
and ping host. Document which groups reach the defended input, every exclusion,
the denominator, the FP rate, and a legitimate blocked request.

## B2 — Structural fix

Fix the same vulnerability through parameterized queries, contextual output
encoding, or input allowlisting as appropriate. Re-run all Part-A payloads and
B1 bypasses, recording each outcome and the scope of the fix. Include legitimate
controls so that rejecting every request cannot masquerade as a working fix.

State the guarantee and its conditions, and explain why the WAF cannot offer the
same guarantee. Keep the implementation, reproduction commands, raw evidence,
and actual result CSVs here.

Suggested CSV fields:
`id,corpus,payload,waf_blocked,waf_rule,oracle_before_fix,oracle_after_fix,evidence_file`.
