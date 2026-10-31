# Voice-Authorised Actions Under Playback and Synthetic Attacks

Final-year research project.

A voice assistant runs sensitive actions: unlocking a door, sending a message, making a payment. An attacker may reach it with a playback of the owner's voice, or with a synthetic voice.

The project asks three questions:

- RQ-A: Where do a speaker check and a spoof check fail, and do their failures complement each other?
- RQ-B: Does a rule with thresholds set by action severity reduce severity-weighted loss, compared with a single fixed threshold and with simple AND and OR rules?
- RQ-C (optional): Does a spoken challenge at the confirm step stop attacks that pass the first two checks?

Public pretrained models are used unchanged. No model is trained. The assistant and its actions are simulated. The threat set is built from public corpora. No new recordings of real attacks are made.

This project does not propose a new detector and does not claim state-of-the-art results.

## Documents

All in [docs/](docs/):

- [PROJECT.md](docs/PROJECT.md): problem, threat model, scope, claims
- [RESEARCH_QUESTIONS.md](docs/RESEARCH_QUESTIONS.md): RQ-A, RQ-B, RQ-C and how each is measured
- [EVALUATION.md](docs/EVALUATION.md): trial labels, split rule, decision rules, metrics
- [PLAN.md](docs/PLAN.md): weekly milestones and gates
- [DECISIONS.md](docs/DECISIONS.md): choices, substitutions and reasons
- [PROGRESS.md](docs/PROGRESS.md): what is done
- [LIMITATIONS.md](docs/LIMITATIONS.md): known limits
- [REFERENCES.md](docs/REFERENCES.md): sources and verification status

## Data

Data is not committed. See [data/README.md](data/README.md).

## Results

Every number comes from a script that writes to [results/](results/). Documents link to those files.

## Status

No code yet. Documents are the skeleton only. See [PROGRESS.md](docs/PROGRESS.md).
