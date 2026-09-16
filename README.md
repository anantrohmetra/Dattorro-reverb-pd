# Dattorro Plate Reverb — Pure Data Implementation

A faithful implementation of Jon Dattorro's plate reverb algorithm in Pure Data, built as part of an exploration into how digital reverberation creates a perceived sense of acoustic space.

---

## About the Algorithm

The Dattorro reverb is a classic plate-style artificial reverb described in Jon Dattorro's 1997 paper. The algorithm models the acoustic behavior of a vibrating metal plate using a signal flow of:

1. **Input diffusion** — four all-pass filters that smear the transient attack of the input signal
2. **Decay tank** — two coupled delay lines with modulated all-pass filters, creating the dense, evolving tail characteristic of plate reverb
3. **Modulation** — two low-frequency oscillators slightly detune the all-pass delays, adding subtle pitch variation that prevents metallic resonances and gives the tail a natural shimmer

---

## Patch Interface

`dattorro-reverb.pd` is a Pure Data abstraction. Drop it into any patch as an object and connect the following inlets and outlets.

### Audio Inlets (signal `~`)

| Inlet # | Name | Description |
|---|---|---|
| 1 | `audio-l` | Left channel input |
| 2 | `audio-r` | Right channel input |

### Control Inlets (message)

| Inlet # | Name | Range | Default | Description |
|---|---|---|---|---|
| 3 | `predelay` | 4 – 400 ms | 0 | Delay before reverb onset — simulates distance from source to plate |
| 4 | `mod-speed` | 0.2 – 20 Hz | 0.5 | LFO speed for tank modulation |
| 5 | `mod-amt` | 0 – 20 | 3 | LFO depth — larger values add more pitch shimmer, too high causes crackle |
| 6 | `decay` | 0.01 – 1 | 0.5 | How long the tail sustains (close to 1 = very long reverb) |
| 7 | `low-cut` | 30 – 30000 Hz | 50 | High-pass filter on the input — rolls off low frequencies before diffusion |
| 8 | `hicut` | 0.01 – 1 | 0.7 | Low-pass filter coefficient inside the decay loop — controls high-frequency damping |
| 9 | `dry/wet` | 0 – 1 | 1 | 0 = fully dry, 1 = fully wet |

### Audio Outlets (signal `~`)

| Outlet # | Name | Description |
|---|---|---|
| 1 | `audio-l` | Left channel output |
| 2 | `audio-r` | Right channel output |

### Internal Parameters (send/receive)

These are not exposed as inlets but can be tuned by sending to the named receivers directly:

| Receiver | Default | Description |
|---|---|---|
| `gain-apf12` | 0.75 | Input diffusion stages 1 & 2 gain |
| `gain-apf34` | 0.625 | Input diffusion stages 3 & 4 gain |
| `gain-apf5` | −0.7 | Decay diffusion gain (right tank) |
| `gain-apf6` | 0.5 | Decay diffusion gain (left tank) |
| `damping` | 0.0005 | Damping coefficient inside the tank lowpass filters |
| `filter-coefficient` | 0.55 | Hi-cut filter gain (alternative to the `hicut` inlet) |

---

## Files

| File | Description |
|---|---|
| `dattorro-reverb.pd` | Main reverb abstraction (load as an object in your patch) |
| `reverb-test.pd` | Demo patch with GUI controls wired to the abstraction |
| `Presentation.pd` | Presentation patch — "Imaginary Spaces" |

---

## Inspiration

> *"Do spaces exist inside or outside our head? How do we get a sense of space using digital reverb?"*

This patch was built to peek under the hood of digital reverb — to understand what mathematical structures produce the perception of a room, a hall, or a plate. The Dattorro algorithm is a clean, well-documented example that balances simplicity with high sonic quality, making it ideal for studying the theory.

---

## Research Paper

This implementation is based on:

> Dattorro, J. (1997). **Effect Design Part 1: Reverberator and Other Filters**. *Journal of the Audio Engineering Society*, 45(9), 660–684.

The paper describes the complete signal flow diagram, all delay lengths (in samples at 29.761 kHz), and the mathematical rationale for each stage. Highly recommended reading for anyone wanting to understand the patch.

---

## Requirements

- [Pure Data](https://puredata.info/) (vanilla Pd or Pd-extended)

Open `reverb-test.pd` to try the reverb with a live microphone input. Open `dattorro-reverb.pd` directly to inspect the internals.

---

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

This project is dedicated to the **public domain** under [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/).

Do whatever you want with it — no attribution required, no restrictions, no conditions.

See [LICENSE](LICENSE) for the full terms.

---

## Contributing

Contributions are welcome. Some directions worth exploring:

- Fix or experiment with the modulation (currently `vd~` causes crackle when mod-depth changes at runtime)
- Add a stereo width control
- Port to other environments (Max/MSP, SuperCollider, RNBO)
- Implement Dattorro Part 2 (chorus/flanger effects from the same paper)

Open an issue or submit a pull request. No formal process — just keep it tidy.
