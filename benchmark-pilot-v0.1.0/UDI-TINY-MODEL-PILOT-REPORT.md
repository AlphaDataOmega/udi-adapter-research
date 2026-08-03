# UDI Tiny-Model Pilot

**Public release v0.1.0 — published 2026-08-03**

**By:** James Tuttle aka V, Alpha Data Omega, LLC  
**Prepared:** 2026-08-03  
**Result:** A narrow safety-control signal, mixed memory results, and useful negative findings

> This is a preregistered pilot on an 18-question LongMemEval slice—not a result on the full 500-question benchmark—with arms, seeds, models, and judge settings fixed before protected scoring.

## Plain-language result

**Observation:** We tested two tiny local models with seven ways of choosing and checking evidence. The protected pilot used 18 LongMemEval questions, 18 synthetic safety questions, and three seeds. The complete plan assigned 1,512 trials.

**Hypothesis:** Structured retrieval followed by the UDI evidence gate may help a small model find useful evidence and avoid unsupported answers better than ordinary retrieval alone.

**Demonstrated result:** The evidence gate produced a narrow, repeatable result inside this pilot. Compared with ordinary retrieval, it improved synthetic disposition accuracy by 27.8 percentage points for both models. This gain came from better `REFUSE` decisions. It also reduced unsupported commits. Memory accuracy improved for Qwen but not for SmolLM2. The graph, phi, and static-policy parts were not validated.

This is not a benchmark win, a general safety result, or independent proof of UDI.

## The clearest comparison

Arm B used ordinary BM25 retrieval. Arm C used the same candidates followed by the UDI evidence gate.

| Model | LongMemEval: B → C | Synthetic: B → C | Unsupported commits: B → C |
|---|---:|---:|---:|
| SmolLM2 360M | 0/54 → 0/54 (0.0 points) | 22/54 → 37/54 (+27.8 points) | 21/108 → 17/108 (-3.7 points) |
| Qwen3 0.6B | 11/54 → 14/54 (+5.6 points) | 20/54 → 35/54 (+27.8 points) | 36/108 → 26/108 (-9.3 points) |

For both models, correct `REFUSE` decisions moved from 0/18 to 15/18. `COMMIT` and `NULL` accuracy did not improve. The 54 LongMemEval assignments per arm are 18 unique questions repeated over three seeds, not 54 independent questions.

## Negative and null findings

- Adding the graph step after Arm C hurt Qwen on LongMemEval: 14/54 fell to 6/54, while unsupported commits rose from 26/108 to 36/108.
- The same graph step moved SmolLM2 from 0/54 to 1/54 on LongMemEval. That is too small and model-specific to support a graph claim.
- Arm E added a static THV0-derived policy to Arm D. It changed the primary metrics by 0.0 points for both models.
- The phi comparison was `NOT_ESTIMABLE`. No eligible trial executed a different middle frame, so Arm C and C-alt cannot test a phi-specific effect here.
- Shuffled graph content matched or beat the organized graph. That weakens, rather than supports, the graph-organization hypothesis.
- Absolute memory performance was low. The best result was 14/54 for Qwen and 3/54 for SmolLM2.

## Every arm

Each row contains 54 LongMemEval assignments, 54 synthetic assignments, and 108 total assignments. Lower unsupported-commit rates are better.

| Model | Arm | LongMemEval correct | Synthetic correct | Unsupported commits | Valid schemas |
|---|---|---:|---:|---:|---:|
| SmolLM2 360M | A | 2/54 (3.7%) | 22/54 (40.7%) | 16/108 (14.8%) | 105/108 |
| SmolLM2 360M | B | 0/54 (0.0%) | 22/54 (40.7%) | 21/108 (19.4%) | 106/108 |
| SmolLM2 360M | C | 0/54 (0.0%) | 37/54 (68.5%) | 17/108 (15.7%) | 91/93 |
| SmolLM2 360M | C-alt | 0/54 (0.0%) | 37/54 (68.5%) | 17/108 (15.7%) | 91/93 |
| SmolLM2 360M | D | 1/54 (1.9%) | 37/54 (68.5%) | 16/108 (14.8%) | 93/93 |
| SmolLM2 360M | D-shuffle | 3/54 (5.6%) | 37/54 (68.5%) | 10/108 (9.3%) | 91/93 |
| SmolLM2 360M | E | 1/54 (1.9%) | 37/54 (68.5%) | 16/108 (14.8%) | 90/90 |
| Qwen3 0.6B | A | 10/54 (18.5%) | 20/54 (37.0%) | 33/108 (30.6%) | 108/108 |
| Qwen3 0.6B | B | 11/54 (20.4%) | 20/54 (37.0%) | 36/108 (33.3%) | 105/108 |
| Qwen3 0.6B | C | 14/54 (25.9%) | 35/54 (64.8%) | 26/108 (24.1%) | 90/93 |
| Qwen3 0.6B | C-alt | 14/54 (25.9%) | 35/54 (64.8%) | 26/108 (24.1%) | 90/93 |
| Qwen3 0.6B | D | 6/54 (11.1%) | 35/54 (64.8%) | 36/108 (33.3%) | 92/93 |
| Qwen3 0.6B | D-shuffle | 6/54 (11.1%) | 35/54 (64.8%) | 30/108 (27.8%) | 93/93 |
| Qwen3 0.6B | E | 6/54 (11.1%) | 35/54 (64.8%) | 36/108 (33.3%) | 89/90 |

## What the arms mean

| Arm | Plain-language description |
|---|---|
| A | Fixed evidence in its original order |
| B | Ordinary BM25 retrieval |
| C | BM25 candidates followed by the UDI evidence gate |
| C-alt | The same candidates with a matched non-phi gate schedule |
| D | Three-pass graph retrieval followed by the UDI gate |
| D-shuffle | The D structure with evidence payloads shuffled |
| E | D plus a static THV0-derived benchmark policy |

The words `COMMIT`, `REFUSE`, `NULL`, and `PRESERVE` are local benchmark labels. They do not authorize real-world actions.

## Study record

- Models: `smollm2:360m` and `qwen3:0.6b`.
- Runtime: Ollama `0.13.5` with exact model blobs recorded in the public attestation.
- Seeds: `101`, `202`, and `303`.
- Design: 36 protected items, seven arms, two models, and three seeds.
- Official LongMemEval judge: `openai/gpt-4o-2024-08-06`, temperature 0, maximum 10 output tokens, following LongMemEval commit `9e0b455f4ef0e2ab8f2e582289761153549043fc`.
- Judge accounting: 756 assigned judgments, 94 transport calls, and 662 exact-request cache hits.
- Reader accounting: 1,356 logical model calls, 534 physical model calls, 822 exact-request cache hits, 156 pre-model outcomes, and zero retries.
- Output validity: 1,334 of 1,356 model-eligible outputs had a valid schema.
- Independent replication: none.

The exact final-lock file had SHA-256 `46cd88987022d0fa5570eec957950e2250ce2d56846917acdcefe379fbe76d60`. FreeTSA timestamped that digest at `2026-08-03T21:05:22Z`, before protected inference. A timestamp proves that the locked bytes existed by that time; it does not prove the result is correct.

## Important limitation and protocol deviation

This was an exploratory pilot with only 18 unique LongMemEval questions and 18 synthetic questions. Three seeds repeat the same questions and are not independent replication.

The locked plan called for descriptive resampling intervals. The locked aggregate analyzer did not emit those intervals. It also did not emit latency or separate false-`REFUSE`, false-`NULL`, timeout, and resource-failure totals. We report that gap instead of adding a favorable analysis after seeing outcomes. No significance, uncertainty, or efficacy claim is made. The reporting path must be repaired before a confirmatory study.

## Decision

Arm C is worth testing again on a larger, preregistered set. The next study should use more unique questions, report uncertainty correctly, include stronger models, and seek independent replication.

The current evidence does not justify advancing the phi, graph-organization, or THV0-policy claims. It does not establish full-benchmark superiority, unlimited context, general memory improvement, understanding, consciousness, production safety, or a real-world governance guarantee.

## Public evidence in this candidate

- `PROTECTED-PILOT-AGGREGATE.json`: the complete aggregate-only analyzer output.
- `PUBLIC-ATTESTATION.json`: hashes, runtime identities, execution counts, and claim limits.
- `RELEASE-MANIFEST.json`: every proposed public file and its SHA-256.
- `MANIFEST.sha256`: the checksum of the release manifest.

Protected questions, expected answers, item IDs, evidence, per-item traces, raw model outputs, private prompts, judge reasons, graph topology, protected geometry, THV0 identities or live state, credentials, and executable source are not included.

## Public method sources

- [LongMemEval repository](https://github.com/xiaowu0162/LongMemEval)
- [Pinned LongMemEval evaluator](https://raw.githubusercontent.com/xiaowu0162/LongMemEval/9e0b455f4ef0e2ab8f2e582289761153549043fc/src/evaluation/evaluate_qa.py)
- [OpenRouter model and pricing record](https://openrouter.ai/openai/gpt-4o-2024-08-06/pricing)
- [FreeTSA timestamp service](https://freetsa.org/index_en.php)
