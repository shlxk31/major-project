# PLAN.md

Weeks are project weeks. Each week ends with a check against the list below. A gate that fails stops the work that depends on it.

## Week 1: licences, corpora, and the Week-1 gate

Tasks:

- Check the licence of every source in [data/README.md](../data/README.md). Record the result in [REFERENCES.md](REFERENCES.md).
- Choose the speaker corpus, RIR set, noise set, and TTS tool. Record each choice in [DECISIONS.md](DECISIONS.md).
- Run the literature search on playback detection and challenge-response voice authentication. Record each result as a row in the table in [REFERENCES.md](REFERENCES.md): paper, year, claim.

Week-1 gate:

> Does simulated playback (owner clip convolved with a room impulse response, plus noise) fool the owner speaker check?
>
> If not, stop and revise the threat. Log the stop and the revision in [DECISIONS.md](DECISIONS.md) before any other work.

Gate output: a results file from a script, with the speaker score for the owner clip and for its simulated playback. The gate result is a number from that file, not from a hand count.

## Week 2: threat set, split, and score cache

Tasks:

- Build the threat set generator. Each trial carries the fields in [EVALUATION.md](EVALUATION.md), section 1.
- Build the dev and test split by speaker, with a fixed seed.
- Run both models on every trial. Cache scores to results/cache/.

Check: every trial has all required fields. No speaker is in both splits.

## Week 3: fixed rules and model checks

Tasks:

- Implement rules R1 to R5 from [EVALUATION.md](EVALUATION.md), section 4.
- Check each pretrained model against its published benchmark. Record any gap between our number and the published number in [DECISIONS.md](DECISIONS.md). A gap is not hidden.
- Choose thresholds for R1 to R5 on dev only. Write them to results/.

Check: thresholds are frozen before any test run.

## Week 4: severity-conditioned rules, RQ-B, RQ-A

Tasks:

- Implement R6 and R7. Choose their thresholds on dev only.
- Run RQ-B on test: loss, attack success, false refusal, with confidence intervals.
- Run the RQ-A failure map: overlap, room and attack breakdowns.
- Start RQ-C only if RQ-A and RQ-B have results and time allows.

Check: the RQ-C decision is recorded. If the literature search shows RQ-C already published in this form, it is dropped as a claim and kept only as an evaluated component.

## Week 5: sensitivity, demo, and write-up

Tasks:

- Repeat the severity-weighted analysis with the second scale.
- Build the Streamlit demo. It accepts an uploaded clip, lets the user pick an action, and shows both scores, the rule applied, the decision, and a mock action log in SQLite. No real actions run.
- Draft the write-up. Every number in the draft is copied from a file in results/ by a script or a checked link, not typed.

## Decision points

- Week-1 gate fails (playback never fools the owner check): stop and revise the threat. Log it in [DECISIONS.md](DECISIONS.md).
- Literature search shows RQ-C already published in this form: drop it as a claim and keep it as an evaluated component.
- Failure map shows no complementary failures: report that as a valid result. Do not hide it.
- Mentor has not agreed the severity values: RQ-B has no loss result yet. Other work continues.
