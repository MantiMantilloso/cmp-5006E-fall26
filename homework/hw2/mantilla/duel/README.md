# Part C — LLM vs. scanner

Status: pending. The [ground_truth.json](ground_truth.json) file is deliberately
empty and is not a completed ground truth. Fill it from a hand review of the
exact target before inspecting the scored tool findings. Record the source
revision, rationale, and review process; disclose any instructor reference data
used. Normalize `(rule, location)` keys consistently with `seclab.scan`.

Run a real classical scanner (`semgrep` on source or `sqlmap` on the endpoint)
and an LLM reviewer against that same target. The Week 6 regex scanner is a studio
stand-in, not the graded classical scanner. Preserve commands, versions,
configuration, raw output, parsed findings, and scoring CSVs here.

Run the LLM review at least twice. Preserve the prompt, backend/model, temperature,
seed, cache status, raw response, and parsed findings for each run. If using the
assignment's documented mock option, label it and explain its limitations. Do not
count the same cached completion as a second independent review.

Use `seclab.scan` to report precision, recall, and F1 against the hand-reviewed
ground truth. Discuss the strongest observed hallucination, worst scanner miss,
changes between LLM runs, and any access-control finding. If an expected error
does not occur, report the observed outcome honestly.

Suggested scoring CSV fields:
`tool,run,target_revision,tp,fp,fn,precision,recall,f1,raw_output_file`.
