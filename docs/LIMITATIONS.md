# LIMITATIONS.md

This file lists what the project does not show. It is updated as the work goes on.

## Threat model

- Attacks are simulated. Playback is a recording convolved with a room impulse response, plus noise. Real rooms, speakers and microphones will differ.
- Synthetic attacks come from one TTS tool. Other TTS and voice-conversion systems may behave differently.
- The attacker does not adapt to the decision rule. A real attacker might.
- RQ-C assumes the attacker does not have live TTS of the random challenge word in the owner's voice. If they do, the challenge may not stop them.

## Data

- All speech is English.
- Owner and other speakers come from a public corpus. The owner is a corpus speaker, not a real user.
- Licences of the sources are not yet verified. See [data/README.md](../data/README.md).
- The sample size of speakers is limited. Confidence intervals are wide when few speakers are used.

## Models

- Pretrained models are used unchanged. Their own training data and biases are not changed.
- The published benchmark numbers for each model may not match our setup. Any gap is recorded in [DECISIONS.md](DECISIONS.md).
- If AASIST cannot run and is replaced, the results apply to the replacement only.

## Evaluation

- Severity costs are fixed in advance but are only as good as the mentor's agreement. A different cost choice could change the ranking of rules. The sensitivity analysis checks one alternative scale only.
- Thresholds are chosen on dev. Results can change if dev and test differ in ways not covered by the speaker split.
- Results are for the stated threat model. They do not cover other attacks, such as replay over a phone line or attacks on the speech recognition step.

## Scope

- The assistant and its actions are simulated. Real accounts, devices, microphones and money are not used.
- No model is trained. No new detector is proposed.
- No state-of-the-art claim is made.
- Multilingual behaviour is not tested.
- LLM tool use is not tested.
