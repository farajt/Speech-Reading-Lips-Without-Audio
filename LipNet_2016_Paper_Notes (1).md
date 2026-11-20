# LipNet (2016) — Paper Notes

## Paper
- **Title:** LipNet: End-to-End Sentence-level Lipreading
- **Authors:** Assael, Shillingford, Whiteson, de Freitas — Oxford & DeepMind
- **Link:** https://arxiv.org/abs/1611.01599

## Problem
- Lip reading = understanding speech from lip movement only, no audio.
- Earlier deep models only picked **one word from a list**.
- LipNet: first model to go from **video → full sentence** in one network.

## Dataset — GRID
- 33 speakers, 1,000 sentences each (~32,700 videos).
- 3-second videos, 75 frames, studio recording.
- Fixed pattern: "place blue at F two now" → only **51 words**.
- **Unseen-speaker test:** 4 people never seen in training.
- **Overlapped-speaker test:** same people, new sentences.

## Pipeline
- Face detection (dlib) → 68 landmarks → mouth crop 100 × 50 px, colour.
- **3D convolution × 3** (32 → 64 → 96 filters) → lip shape + movement.
- **Bidirectional GRU × 2** (256 each way) → context before and after.
- Output per frame: 26 letters + space + blank.
- **CTC loss** → trains without frame-level labels.
- **Beam search + character language model** → autocorrect.

## Training
- Adam, learning rate 0.0001, batch 50, dropout 0.5.
- Augmentation: mirror video, single-word clips, random frame drop/duplicate.

## Results (WER — lower is better)

| Method | Unseen speakers | Overlapped speakers |
|---|---|---|
| Human lip-readers | 47.7% | — |
| Baseline-LSTM (older method) | 52.8% | 26.3% |
| Baseline-2D (no motion) | 26.7% | 11.6% |
| Baseline-NoLM (no language model) | 13.6% | 5.6% |
| **LipNet** | **11.4%** | **4.8%** |

- Accuracy: **95.2%** seen speakers, **88.6%** new speakers.
- ~4× fewer errors than human lip-readers.
- 3D conv (motion) gives the biggest gain: 26.7% → 11.4%.
- Language model helps a little: 13.6% → 11.4%.
- Errors mostly between look-alike sounds (p / b / m).

## Limitations
- Only 51 words, fixed sentence pattern → not real speech.
- Studio video, not real-world webcams.
- Only 4 unseen test speakers.
- Spoken aloud, not silent mouthing.
- British English only.
- Fixed 3-second clips, no real-time use.
- No pretraining, no official code released.
