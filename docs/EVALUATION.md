# EVALUATION.md

This file defines how trials are labelled, how data is split, which decision rules are compared, and which metrics are reported. The scripts that compute these must follow this file. If a script differs, this file is updated first and the change is logged in [DECISIONS.md](DECISIONS.md).

## 1. Trial labels

Every trial has these fields. A missing field is an error, and the trial is not used.

| Field | Values | Meaning |
|---|---|---|
| trial_id | text | Unique id |
| speaker_id | text | Speaker in the source corpus |
| is_owner | 0 or 1 | 1 if the speaker is the enrolled owner |
| spoof_label | 0 or 1 | 1 if the audio is a playback or synthetic fake, 0 if live |
| attack_type | none, impostor, playback, synthetic | Type of trial (see below) |
| room | text | Room impulse response used, or "none" for live trials |
| severity | low, medium, high | Severity of the action the command requests |
| source | text | Corpus or tool the audio came from |

Attack types:

- none: live owner speech. is_owner = 1, spoof_label = 0. Should EXECUTE.
- impostor: live speech from a non-owner. is_owner = 0, spoof_label = 0. Should REFUSE.
- playback: recording of the owner, played through a simulated room. is_owner = 1, spoof_label = 1. Should REFUSE.
- synthetic: TTS or voice-converted command. spoof_label = 1. Should REFUSE.

The impostor type is a decision of this plan, not in the first brief. It is needed to measure the speaker check on its own. It is recorded in [DECISIONS.md](DECISIONS.md) as D10.

A trial should be executed only if it is none-type. Any other trial that gets EXECUTE is an attack success.

## 2. Split rule

- Splits are made by speaker. No speaker_id appears in both dev and test.
- Dev split: used to choose thresholds, and only that.
- Test split: used once, after thresholds are frozen.
- Thresholds are written to a file under results/ with a timestamp before the test run. The test run reads that file and does not change it.
- If a threshold must change after test has been run, the test results are marked as stale and the run is repeated. The reason goes in [DECISIONS.md](DECISIONS.md).

Speaker assignment to dev or test is done once, with a fixed random seed. The seed is recorded.

## 3. Scores

- Speaker score: from ECAPA-TDNN, unchanged. Higher means more likely the owner.
- Spoof score: from AASIST, unchanged. Higher means more likely live speech. A substitute is recorded in [DECISIONS.md](DECISIONS.md) if one is used.
- Scores are cached to results/cache/ (git-ignored) and summarised to results/ (committed).

## 4. Decision rules

Each rule maps a trial's two scores and its severity to EXECUTE, CONFIRM or REFUSE.

- R1 Speaker only: EXECUTE if the speaker score passes its threshold, else REFUSE.
- R2 Spoof only: EXECUTE if the spoof score passes its threshold, else REFUSE.
- R3 AND: EXECUTE only if both checks pass. Otherwise REFUSE.
- R4 OR: EXECUTE if either check passes. Otherwise REFUSE. This rule is included to show its cost.
- R5 Fixed threshold: one speaker threshold and one spoof threshold, used for all severities. EXECUTE if both pass. REFUSE otherwise.
- R6 Severity-conditioned: one pair of thresholds per severity level. EXECUTE if both pass the thresholds for that level. REFUSE otherwise.
- R7 Three-way (for RQ-B and RQ-C): EXECUTE if both scores pass the execute thresholds for the severity. REFUSE if either score is below its refuse floor. CONFIRM for all other trials. RQ-C tests the challenge only on CONFIRM trials.

R1 to R5 use two outcomes only. R6 and R7 are the only rules that can return CONFIRM. Rule R7 is the rule that uses the CONFIRM step. Its exact form is decided in [DECISIONS.md](DECISIONS.md) before dev runs.

Thresholds for every rule are chosen on dev only, then frozen.

## 5. Costs and severity-weighted loss

For a trial i with severity s and decision d:

- If the trial should EXECUTE and the decision is REFUSE, this is a false refusal. Its cost is cost_false_reject[s].
- If the trial should not EXECUTE and the decision is EXECUTE, this is a false accept. Its cost is cost_false_accept[s].
- CONFIRM decisions are costed as configured in severity.yaml, once the mentor agrees the values. Until then CONFIRM is reported as a separate count and not costed.
- Correct decisions cost 0.

Severity-weighted loss for a rule is the mean cost over all test trials. Costs come from [configs/severity.yaml](../configs/severity.yaml). Those values are TODO until agreed, so no loss number is reported before that.

## 6. Metrics

Reported per severity level, unless stated otherwise:

- Attack success rate: share of should-not-EXECUTE trials that get EXECUTE.
- False refusal rate: share of should-EXECUTE trials that get REFUSE. CONFIRM is reported separately.
- Severity-weighted loss, as in section 5.
- Failure overlap for RQ-A: among attack trials, share caught by the speaker check only, the spoof check only, both, and neither.

Breakdowns:

- By room.
- By attack type.

Uncertainty:

- Bootstrap confidence intervals, resampling speakers (not trials), so that trials from the same speaker are kept together.
- The number of bootstrap resamples and the interval level are set in a config file, marked TODO until agreed. They are not chosen by looking at results.

## 7. Sensitivity analysis

- The severity-weighted analysis is repeated with a second severity scale.
- The second scale is in [configs/severity.yaml](../configs/severity.yaml) under sensitivity_scale. It is TODO until agreed.
- Both scales are reported. If the ranking of rules changes between them, that is reported.

## 8. Outputs

All written by scripts to results/:

- Scores summary per trial type.
- Failure map (RQ-A).
- Thresholds for each rule, from dev.
- Rule results on test (RQ-B).
- Breakdowns by room and attack type.
- Sensitivity results.
- Challenge results, if RQ-C is run.

No number in any document is typed by hand. Documents link to the results file.
