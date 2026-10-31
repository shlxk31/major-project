# DECISIONS.md

Each entry has a status: accepted, proposed, or superseded. Substitutions, assumptions and changes are added here, not edited away. A new decision that replaces an old one marks the old one as superseded.

## D1: Clean repository (accepted, 2026-11-01)

The project starts from an empty repository. Nothing from github.com/shlxk31/voiceguard is copied: no code, results, .env files, checkpoints, .pkl or .docx files, or audio. That repository is a reference only.

Reason: the project must not reuse results from an earlier project. Every result here must come from a script run in this project.

## D2: ECAPA-TDNN, used unchanged (accepted, 2026-11-01)

The speaker check uses a pretrained ECAPA-TDNN model with no changes to its weights or training.

Reason: the project tests existing checks. It does not build a new speaker model. Exact source and version are TODO and will be recorded when chosen. See [REFERENCES.md](REFERENCES.md).

## D3: AASIST, used unchanged (accepted, 2026-11-01)

The spoof check uses a pretrained AASIST model with no changes to its weights or training.

Substitution rule: a different spoof model may be used only if AASIST cannot run. Any substitution is logged here with the reason, the new model, and the date. No substitution has been made.

## D4: Thresholds chosen on dev only (accepted, 2026-11-01)

All thresholds are chosen on the dev split. They are written to results/ and frozen before the test run. If a threshold changes after a test run, the test results are marked stale and rerun.

Reason: choosing thresholds on test would make the results optimistic.

## D5: Severity fixed in configs (accepted, 2026-11-01)

Severity levels (low, medium, high) and their costs are fixed in [configs/severity.yaml](../configs/severity.yaml) before any test run. The values are TODO until the mentor agrees them. Each value is marked as agreed-in-advance only after agreement.

Reason: costs chosen after seeing results would bias the rule comparison.

## D6: Splits by speaker (accepted, 2026-11-01)

Dev and test splits contain different speakers. A speaker never appears in both.

Reason: if the same speaker is in both splits, the speaker check can look better than it is.

## D7: Simulated assistant and mock actions (accepted, 2026-11-01)

The assistant and its actions (unlock, send, pay) are simulated. No real accounts, devices or money are used. The demo writes to a mock action log in SQLite.

## D8: No model training (accepted, 2026-11-01)

No model is trained or fine-tuned. The project measures pretrained checks and rules.

## D9: Threat set from public corpora (accepted, 2026-11-01)

The threat set is built from public speaker corpora. Playback is simulated by convolving owner clips with room impulse responses and adding noise. Synthetic commands come from one declared TTS tool. No new recordings of real attacks are made.

Sources are TODO and listed in [data/README.md](../data/README.md) with licence checks pending.

## D10: Impostor trials added (proposed, 2026-11-01)

The first brief did not list impostor trials. This plan adds them: live speech from non-owners. They are needed to measure the speaker check on its own. See [EVALUATION.md](EVALUATION.md), section 1.

Status: proposed. Accepted when the mentor agrees.

## D11: Decision rules R1 to R7 (proposed, 2026-11-01)

The rules are defined in [EVALUATION.md](EVALUATION.md), section 4. R1 to R5 are baselines. R6 is the severity-conditioned rule. R7 is the three-way rule that includes CONFIRM.

Status: proposed. The exact form of R7, including the refuse floors, is fixed before any dev run.

## D12: Week-1 gate (accepted, 2026-11-01)

Before other work, check whether simulated playback fools the owner speaker check. If it does not, stop and revise the threat. Log the stop and the revision here.

Reason: if the threat cannot fool the check, the failure map has nothing to show.

## D13: RQ-C is optional (accepted, 2026-11-01)

The spoken challenge (RQ-C) is started only after RQ-A and RQ-B have results. If the literature search shows the same challenge design already published, RQ-C is dropped as a claim and kept as an evaluated component.

Reason: keeps the main claims safe. See [RESEARCH_QUESTIONS.md](RESEARCH_QUESTIONS.md).

## D14: ASR for the challenge (proposed, 2026-11-01)

RQ-C uses an automatic speech recognition (ASR) model to check the spoken word. The ASR model is not chosen yet. It is recorded here when chosen, with the reason and the version.

Status: proposed. Only relevant if RQ-C runs.

## D15: Streamlit demo and SQLite log (accepted, 2026-11-01)

The demo is built with Streamlit. It writes mock actions to SQLite. No real action runs.

## D16: Results come only from scripts (accepted, 2026-11-01)

Every number in the project is written by a script to results/. Documents link to those files. No number is typed into a document by hand.

## D17: Claims limited to results (accepted, 2026-11-01)

The project does not claim a new detector, a new real-attack dataset, state-of-the-art results, or "first" for any method. "First" is used only if the literature search verifies it. See [PROJECT.md](PROJECT.md).

## D18: Second severity scale for sensitivity (proposed, 2026-11-01)

The severity-weighted analysis is repeated with a second severity scale. The second scale is TODO and will be agreed with the mentor.

Status: proposed.

## D19: Bootstrap over speakers (accepted, 2026-11-01)

Confidence intervals resample speakers, not trials. Trials from the same speaker stay together.

Reason: trials from one speaker are not independent.

## D20: Language scope (accepted, 2026-11-01)

English speech only. Multilingual coverage is out of scope.
