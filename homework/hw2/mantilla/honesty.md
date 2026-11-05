# Where our attack, defense, or comparison might be unfair.

Status: pending experiments and personal review. Replace the prompts below with
at least three substantive critiques tied to actual evidence.

1. **Ground-truth independence:** When and how was the target reviewed by hand?
   What source evidence supports each label? Disclose use of instructor reference
   data and any tool output seen before finalizing the labels.
2. **Unequal tuning:** How much effort went into the WAF, scanner rules, and LLM
   prompt? Which choices could favor one side?
3. **Benign-corpus realism:** Which provided input groups match the defended
   endpoint? Report the denominator, any exclusions, and legitimate requests
   blocked. Does the corpus represent realistic use?
4. **Reproducibility and target fidelity:** Which finding is least reliable?
   Record the Medium configuration, target revision, controls, and the limitations
   of any simulated command effect or XSS oracle.
5. **LLM variability:** Record backend, model, prompt, settings, cache use, and
   changes across at least two reviews. Explain the limits of a mock if used.
