# Dattorro Plate Reverb — Pure Data

A Pure Data implementation of Jon Dattorro's plate reverb algorithm, built as part of an exploration into how digital reverberation creates a perceived sense of acoustic space.

> *"Do spaces exist inside or outside our head? How do we get a sense of space using digital reverb?"*

---

## About the Algorithm

The Dattorro reverb is a classic plate-style artificial reverb described in Jon Dattorro's 1997 paper. It models the acoustic behaviour of a vibrating metal plate using:

1. **Input diffusion** — four all-pass filters that smear the transient attack of the input signal
2. **Decay tank** — two coupled feedback delay lines with modulated all-pass filters, creating a dense, evolving tail
3. **Modulation** — two LFOs slightly detune the all-pass delays, adding shimmer and preventing metallic resonances

See [history.md](history.md) for the full lineage from Schroeder (1962) through the hardware era to Dattorro (1997).

---

## Files

| File | Description |
|---|---|
| `dattorro-reverb.pd` | Main Dattorro plate reverb abstraction |
| `dattorro-reverb-algorithm.pd` | Internal algorithm subpatch |
| `digital-not-so-reverb.pd` | 8-channel FDN reverb (contrast/comparison) |
| `reverb-test.pd` | Demo patch with GUI controls |
| `Presentation.pd` | Presentation — "Imaginary Spaces" |
| `questions.pd` | Questions subpatch for the presentation |
| `more-questions.pd` | Extended questions |
| `reflections.pd` | Reflections subpatch |
| `other-applications.pd` | Other applications of the algorithm |
| `Lexicon.pd` | Lexicon-style reverb reference |
| `Disclaimer.pd` | Disclaimer subpatch |
| `history.md` | History of the Dattorro reverb and its predecessors |

---

## Patch Interface

`dattorro-reverb.pd` is a Pure Data abstraction — drop it into any patch as an object.

### Audio Inlets (signal `~`)

| Inlet | Name | Description |
|---|---|---|
| 1 | `audio-l` | Left channel input |
| 2 | `audio-r` | Right channel input |

### Control Inlets (message)

| Inlet | Name | Range | Default | Description |
|---|---|---|---|---|
| 3 | `predelay` | 4 – 400 ms | 0 | Delay before reverb onset |
| 4 | `mod-speed` | 0.2 – 20 Hz | 0.5 | LFO speed for tank modulation |
| 5 | `mod-amt` | 0 – 20 | 3 | LFO depth / shimmer amount |
| 6 | `decay` | 0.01 – 1 | 0.5 | Tail length (near 1 = very long) |
| 7 | `low-cut` | 30 – 30000 Hz | 50 | High-pass on input |
| 8 | `hicut` | 0.01 – 1 | 0.7 | Low-pass damping coefficient |
| 9 | `dry/wet` | 0 – 1 | 1 | 0 = dry, 1 = wet |

### Audio Outlets (signal `~`)

| Outlet | Name | Description |
|---|---|---|
| 1 | `audio-l` | Left channel output |
| 2 | `audio-r` | Right channel output |

### Internal Parameters (send/receive)

| Receiver | Default | Description |
|---|---|---|
| `gain-apf12` | 0.75 | Input diffusion stages 1 & 2 gain |
| `gain-apf34` | 0.625 | Input diffusion stages 3 & 4 gain |
| `gain-apf5` | −0.7 | Decay diffusion gain (right tank) |
| `gain-apf6` | 0.5 | Decay diffusion gain (left tank) |
| `damping` | 0.0005 | Tank lowpass damping coefficient |
| `filter-coefficient` | 0.55 | Hi-cut filter gain |

---

## Requirements

- [Pure Data](https://puredata.info/) (vanilla Pd or Pd-extended)

Open `reverb-test.pd` to try the reverb with live microphone input.

---

## Research Paper

> Dattorro, J. (1997). **Effect Design Part 1: Reverberator and Other Filters**. *Journal of the Audio Engineering Society*, 45(9), 660–684.

The paper describes the complete signal flow, all delay lengths (in samples at 29.761 kHz), and the mathematical rationale for each stage.

---

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

Public domain — [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/). Do whatever you want with it.

See [LICENSE](LICENSE) for full terms.

---

## Contributing

Open to contributions. Some directions:

- Fix the `vd~` crackle when mod-depth changes at runtime
- Add stereo width control
- Port to Max/MSP, SuperCollider, or RNBO
- Implement Dattorro Part 2 (chorus/flanger from the same 1997 paper)

No formal process — open an issue or pull request.
