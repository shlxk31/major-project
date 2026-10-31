# Data

Data is not committed to this repository. Files under data/raw and data/processed are ignored by git. Audio (*.wav) is also ignored.

Sources. Licence is not yet checked for any of them.

Speech and threat set:

- Owner and other speaker audio from a public English speech corpus. Not chosen. verify licence
- Room impulse responses from a public RIR set. Not chosen. verify licence
- Noise recordings from a public noise set. Not chosen. verify licence
- Synthetic commands from one declared TTS tool. Not chosen. verify licence

Models (weights are not committed):

- Pretrained ECAPA-TDNN speaker model. verify licence
- Pretrained AASIST spoofing model. verify licence
- ASR model for RQ-C, only if RQ-C runs. Not chosen. verify licence

Evaluation:

- Spoofing corpus for checking the spoof model, for example ASVspoof. Not chosen. verify licence

Each chosen source is recorded in [docs/REFERENCES.md](../docs/REFERENCES.md) and [docs/DECISIONS.md](../docs/DECISIONS.md).
