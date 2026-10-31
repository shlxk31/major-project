# PROJECT.md

Title: Voice-Authorised Actions Under Playback and Synthetic Attacks

## Problem

A voice assistant performs sensitive actions for its owner: unlock a door, send a message, make a payment. Two kinds of attack can reach it:

- Playback: a recording of the owner's voice, played through a speaker in a room.
- Synthetic: a text-to-speech (TTS) or voice-converted command.

The assistant combines two checks and one decision rule:

- Speaker check: is this the enrolled owner? Uses ECAPA-TDNN, pretrained, unchanged.
- Spoof check: is this live human speech, or a playback or synthetic fake? Uses AASIST, pretrained, unchanged. A substitute is allowed only if AASIST cannot run, and the substitution is recorded in [DECISIONS.md](DECISIONS.md).
- Decision rule: for each command, choose EXECUTE, CONFIRM or REFUSE. Thresholds depend on the action's severity (low, medium, high).

Each kind of error has a cost. That cost depends on how severe the action is.

## Research questions

Full definitions and measurements are in [RESEARCH_QUESTIONS.md](RESEARCH_QUESTIONS.md).

- RQ-A: Where do the speaker check and the spoof check fail? Do their failures complement each other under simulated playback and synthetic attacks?
- RQ-B: Does a severity-conditioned rule reduce severity-weighted loss, compared with a single fixed threshold and with simple AND and OR rules?
- RQ-C (optional): Does a spoken challenge at the CONFIRM step stop attacks that pass the first two checks? Only after RQ-A and RQ-B work.

## Threat model

The attacker:

- Can play a recording of the owner through a loudspeaker in the same room as the device. This is simulated, not done live.
- Can send a synthetic command made by one declared TTS tool.
- Cannot change the assistant's code, models or thresholds.
- Does not know the random challenge word in RQ-C, if RQ-C is run.

Attacks are simulated from public corpora. No new recordings of real attacks are made.

## Scope

In scope:

- Simulated assistant with mock actions. No real accounts or devices.
- English speech only.
- Pretrained models used unchanged. No training.
- A constructed threat set built from public corpora.
- Severity levels fixed in advance in [configs/severity.yaml](../configs/severity.yaml).
- A Streamlit demo that writes a mock action log to SQLite.

Out of scope:

- Model training.
- Real accounts or devices.
- Live microphones.
- Multilingual coverage.
- LLM tool use.
- New recordings of real attacks.

## Claims we may make

Only if the results support them:

- An empirical failure map of two pretrained checks under simulated playback.
- A severity-conditioned evaluation of standard decision rules, with a sensitivity analysis.
- A challenge-response step evaluated under the same protocol, with its threat model stated.

## Claims we must not make

- A new detector.
- A new real-attack dataset.
- State-of-the-art performance.
- "First" for any method, unless the literature search verifies it. See [REFERENCES.md](REFERENCES.md).

## Related documents

- [PLAN.md](PLAN.md): weekly milestones and gates
- [EVALUATION.md](EVALUATION.md): trial labels, split rule, metrics
- [DECISIONS.md](DECISIONS.md): choices, substitutions and reasons
- [LIMITATIONS.md](LIMITATIONS.md): known limits
- [REFERENCES.md](REFERENCES.md): sources and verification status
- [PROGRESS.md](PROGRESS.md): what is done
