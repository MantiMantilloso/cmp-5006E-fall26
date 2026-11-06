# HW2 working instructions

- Treat `homework/hw2/mantilla/` as the main working directory for this homework.
  Run HW2 commands here and resolve shared repository paths from
  `git rev-parse --show-toplevel`.
- Read `../hw2-web.md` for the assignment. Keep submitted code, reports, fixtures,
  and evidence in this directory so the PR changes only this subtree.
- Work on `hw2-mantilla`. The branch starts at course `upstream/main`; the eventual
  submission PR goes from the `origin` fork to course `upstream/main`.
- Preserve unrelated working-tree edits and other students' submissions. Stage
  this subtree explicitly, then inspect the full staged diff before committing.
- Reuse root `seclab` helpers through `PYTHONPATH`. Put HW2 target adaptations and
  fixes here rather than editing shared lab or studio files.
- Part A uses the custom DVWA-style Medium layer in `attack/medium_app.py`.
  Preserve the distinction from actual DVWA Medium and the acceptance ambiguity
  documented in `REVIEW.md`. Part B's original results use the Low baseline;
  do not describe them as results for a WAF stacked with Medium.
- The separate `verify_readiness.py` follow-up evaluates Medium compositions,
  all Part-A inputs, all B1 bypasses, and `coverage_payloads.json`. Preserve the
  distinction between its `results/readiness/` evidence and the six original
  result CSVs. Run this checker after merging behavior-affecting group changes.
- Keep supplied benign inputs and their provenance. Build and document the
  graded ground truth by source inspection before examining scored tool output.
- Preserve real raw outputs and CSVs. Mark pending work explicitly; never invent
  results or present Week 6/7 studio evidence as a new HW2 experiment.
- Append material AI assistance to `AI_LOG.md` in line with
  `../../../resources/ai-policy.md`. Leave personal understanding/review fields
  for the student to confirm.
- Follow `../../../resources/ethics-and-scope.md`: use authorized local lab
  targets and synthetic data.
