# RESEARCH_QUESTIONS.md

Each question lists what is measured, which script writes it, and what result would count for or against it. Numbers come only from scripts that write to results/.

## RQ-A: Where do the checks fail, and do the failures complement each other?

Question: Under simulated playback and synthetic attacks, where do the speaker check and the spoof check fail? Do their failures overlap, or does one catch what the other misses?

Measured:

- Speaker check failure on impostor and playback trials. A failure is a trial that should be refused and gets a speaker score above the speaker threshold.
- Spoof check failure on playback and synthetic trials. A failure is a trial with spoof_label = 1 that gets a spoof score above the spoof threshold.
- Failure overlap. Among attack trials, the share caught by the speaker check only, the spoof check only, both, and neither.
- Breakdowns by room and by attack type.

Written by: a failure-map script, to results/failure_map.csv (planned).

Counts for the question:

- The checks complement each other if many attacks are caught by one check and missed by the other.
- The checks do not complement each other if the attacks that pass one check mostly pass the other too.

Both outcomes are valid results. A "no complementary failures" finding is reported as is.

## RQ-B: Does a severity-conditioned rule reduce severity-weighted loss?

Question: Does a rule with thresholds set per severity level give lower severity-weighted loss than:

- a single fixed threshold pair used for all severities,
- an AND rule,
- an OR rule,
- each single check alone?

Measured:

- Severity-weighted loss for each rule, on the test split, with bootstrap confidence intervals over speakers.
- Attack success rate and false refusal rate for each severity level.

Written by: a rules-evaluation script, to results/rules_test.csv (planned).

Thresholds are chosen on the dev split only and frozen before test. See [EVALUATION.md](EVALUATION.md).

Counts for the question: the severity-conditioned rule has lower loss on test, with a confidence interval that does not overlap the best baseline. If the intervals overlap, the claim is not made.

Costs come from [configs/severity.yaml](../configs/severity.yaml). Those values are TODO until the mentor agrees them. RQ-B cannot be answered until they are agreed.

## RQ-C (optional): Does a spoken challenge stop attacks that pass the first two checks?

Question: At the CONFIRM step, the owner is asked to repeat a random word. The speaker check and an automatic speech recognition (ASR) check must both pass. Does this stop attacks that passed the speaker and spoof checks?

Measured:

- Attack success rate with and without the challenge, for the attacks that reach CONFIRM.
- False refusal rate of owner trials with the challenge.

Threat model note: the challenge helps only if the attacker cannot produce the random word in the owner's voice. A synthetic attack with live TTS of the word is a case the challenge may not stop. This is stated as a limitation, not hidden.

Gate: RQ-C is started only after RQ-A and RQ-B have results. It is dropped as a claim if the literature search shows the same challenge design already published. In that case it stays only as an evaluated component. See [PLAN.md](PLAN.md).
