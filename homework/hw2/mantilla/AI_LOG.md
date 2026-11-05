# AI assistance log

## 2026-11-05 — HW2 workspace preparation

**Tool:** OpenAI Codex.

**What I asked:** Prepare the local repository for an HW2 submission by pull
request under `homework/hw2/mantilla/`, making that folder the main working
directory for the homework.

**What I got:** The assistant inspected repository state and assignment policies,
refreshed the remote references, created `hw2-mantilla` from course `upstream/main`,
and drafted the directory layout, README, working instructions, and report
templates. It copied the instructor's benign corpus unchanged and created an
explicitly empty ground-truth template for later independent source review. It
identified that the supplied `vuln-web` has no Medium filter mode. No homework
attacks, defenses, scanner comparison, or result CSVs were produced during setup.

**What I did with it:** The scaffold is saved for my review and further work.
Earlier committed studio work remains on `main`; unrelated uncommitted work is
preserved. I still need to implement and run the experiments, complete the
reports, and review every claim before submission.

**Did I understand it?** Pending personal confirmation. I need to explain the
submission branch/base, why a Medium-level target is required, how each oracle
confirms an effect, and why ground truth must be established independently of
the scored tools.
