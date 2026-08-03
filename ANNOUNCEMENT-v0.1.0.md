# UDI Tiny-Model Pilot v0.1.0

**Published:** 2026-08-03

**By:** James Tuttle (aka V), Alpha Data Omega, LLC

We published the UDI Tiny-Model Pilot v0.1.0.

We tested two tiny language models on 18 LongMemEval questions and 18 synthetic cases. We compared ordinary BM25 retrieval with a bounded evidence gate that chooses `COMMIT`, `REFUSE`, or `NULL`.

On the synthetic cases, the evidence gate improved decision accuracy by 27.8 percentage points for both models and reduced unsupported commits. On LongMemEval, Qwen improved slightly while SmolLM2 did not improve. The graph step hurt Qwen, one planned comparison could not be estimated, and the locked analysis did not produce uncertainty intervals.

This is an exploratory result—not a benchmark win, proof of significance, or independent validation.

We are publishing the evidence so others can inspect the method, reproduce the run, find errors, and design stronger tests. We want falsification and improvement, not endorsement.

[Read the public evidence bundle](benchmark-pilot-v0.1.0/UDI-TINY-MODEL-PILOT-REPORT.md).

The public bundle excludes protected questions, per-item traces, raw model and judge outputs, private prompts, graph topology, protected geometry, THV0 identities or live state, credentials, service addresses, and executable source.
