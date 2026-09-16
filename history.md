# History of the Dattorro Reverb

---

## The Physical Plate (the thing Dattorro is imitating)

Before digital reverb existed, studios used physical plate reverberators. The most famous was the **EMT 140**, introduced in 1957 by Elektromesstechnik. It was a large steel plate (roughly 2m × 1m) suspended on springs inside a soundproof cabinet. A transducer drove vibrations into the plate; contact microphones picked them up at different positions. The result was a dense, smooth, characteristically bright reverb tail — the sound that defined records from the late 1950s through the 1970s. The EMT 140 weighed around 270 kg. Studios wanted that sound in a box that didn't need its own room.

---

## Schroeder (1962) — the mathematical foundation

Manfred Schroeder at Bell Labs published the foundational paper on artificial reverberation:

> Schroeder, M. R. (1962). **Natural Sounding Artificial Reverberation**. *Journal of the Audio Engineering Society*, 10(3), 219–223.

Schroeder showed that two signal processing primitives — the **comb filter** (a delay line with feedback) and the **all-pass filter** (which delays without coloring the frequency response) — could be combined to produce a dense, reverb-like decay. His proposed structure was a bank of parallel comb filters feeding a series of all-pass filters. It was computationally cheap and became the basis for nearly all digital reverb to follow.

The weakness: Schroeder's design produced audible metallic resonances (the comb filter peaks), making it sound artificial at low decay times.

---

## Moorer (1979) — absorptive comb filters

James Moorer extended Schroeder's work:

> Moorer, J. A. (1979). **About This Reverberation Business**. *Computer Music Journal*, 3(2), 13–28.

Moorer added a **one-pole low-pass filter inside each comb filter's feedback loop**, which caused high frequencies to decay faster than low frequencies — exactly what happens in real rooms. This dramatically improved naturalness. He also introduced early reflections as a separate stage before the late tail. Moorer's design became the template for commercial hardware reverbs.

---

## The Hardware Era (late 1970s – 1990s)

Several companies turned these algorithms into dedicated hardware units. These machines defined the sound of commercial music production:

| Unit | Year | Notes |
|---|---|---|
| EMT 250 | 1976 | First commercially available digital reverb |
| Lexicon 224 | 1978 | Became a studio standard; used on thousands of albums |
| AMS RMX16 | 1981 | Famous "ambience" preset; heavily used in 1980s pop |
| Lexicon 480L | 1986 | High watermark of hardware digital reverb quality |
| EMT 244 / 246 | 1978–1981 | Digital successors to the plate 140 |

These units were expensive (tens of thousands of dollars), proprietary, and their algorithms were trade secrets.

---

## Dattorro (1997) — the open algorithm

Jon Dattorro published a detailed, complete description of a high-quality plate reverb algorithm in the Journal of the Audio Engineering Society:

> Dattorro, J. (1997). **Effect Design Part 1: Reverberator and Other Filters**. *Journal of the Audio Engineering Society*, 45(9), 660–684.

At the time of writing, Dattorro was at **Stanford CCRMA** (Center for Computer Research in Music and Acoustics).

### What he described

Dattorro's reverb is built around a **"figure-of-eight" feedback delay network** — two coupled feedback loops that cross-feed each other. This is also called a **plate topology** because it loosely models how vibrations scatter and decay in a physical plate.

The signal path in order:

1. **Pre-delay** — a simple delay before the reverb begins, simulating distance between source and plate
2. **Input low/high-pass filtering** — rolls off the extremes before diffusion
3. **Input diffusion** — four all-pass filters that smear the attack transient into a smooth onset
4. **Decay tank** — the figure-of-eight core: two feedback loops, each with a modulated all-pass filter, a delay line, a one-pole low-pass filter (damping), another all-pass filter, and a longer delay. The loops cross-feed each other.
5. **Modulation** — two LFOs vary the delay times inside the tank all-pass filters, preventing metallic resonances
6. **Output taps** — the output is summed from multiple taps at different points in the tank

### What made it historically important

- Published **openly** in a peer-reviewed journal with exact delay lengths and gain coefficients — everything needed to implement it
- **Sounds competitive with commercial hardware**
- Gave researchers and developers a **reference implementation** to study, build on, and compare against

---

## Part 2 (same year)

> Dattorro, J. (1997). **Effect Design Part 2: Delay-Line Modulation and Chorus**. *Journal of the Audio Engineering Society*, 45(10), 764–788.

Covered chorus, flanging, and vibrato using the same delay-line modulation techniques.

---

## Legacy

The Dattorro algorithm became one of the most-implemented reverb algorithms in open-source audio:

- Basis of the **`plate` reverb** in Faust's standard library (`reverbs.lib`)
- Found in numerous SuperCollider, Max/MSP, and Pure Data patches — including `dattorro-reverb.pd` in this folder
- Used in several hardware pedals and software plugins
- A standard pedagogical example in audio DSP courses

The physical EMT 140 plates it was designed to emulate still exist in a handful of studios. A working one costs $10,000–$30,000 on the used market. The algorithm is free.

---

## Sources

1. **Dattorro, J. (1997).** Effect Design Part 1: Reverberator and Other Filters. *JAES*, 45(9), 660–684.
2. **Schroeder, M. R. (1962).** Natural Sounding Artificial Reverberation. *JAES*, 10(3), 219–223.
3. **Moorer, J. A. (1979).** About This Reverberation Business. *Computer Music Journal*, 3(2), 13–28.
4. **Zölzer, U. (2002).** *DAFX: Digital Audio Effects*. Wiley. (Chapter 7)
5. **Smith, J. O. (2010).** *Physical Audio Signal Processing*. CCRMA / W3K Publishing. (ccrma.stanford.edu/~jos/pasp/)
