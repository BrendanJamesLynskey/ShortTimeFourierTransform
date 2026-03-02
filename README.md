# The Short-Time Fourier Transform — A Visual & Mathematical Guide

An interactive, animated guide to the Short-Time Fourier Transform (STFT), the most fundamental time–frequency representation in signal processing. Built entirely with vanilla HTML Canvas and JavaScript — zero dependencies. Significantly more detailed than its companion guides, with full equation derivations and symbol-by-symbol breakdowns throughout.

**[→ View the Live Guide](https://brendanjameslynskey.github.io/STFT-Guide/)**

---

## What's Inside

Ten chapters take you from first principles to real-world applications, each with real-time animated Canvas visualisations and detailed mathematical exposition. Every equation is accompanied by a symbol definition panel explaining each term.

### 1 · Why Frequency Alone Isn't Enough

The classical Fourier Transform's fatal limitation: it destroys all temporal information. A side-by-side comparison shows the global FT spectrum (identical for signals with different timing) versus the STFT spectrogram (which reveals when each frequency occurs). Switch between sequential tones, simultaneous tones, and a linear chirp.

### 2 · The STFT Equation — Symbol by Symbol

Three equivalent formulations of the continuous STFT: the integral definition (Eq. 2.1), the windowed-Fourier interpretation (Eq. 2.2), and the inner-product / time–frequency atom form (Eq. 2.3). Interactive sliders let you drag the analysis window τ along a chirp signal and adjust its width, watching the extracted spectrum update in real time.

### 3 · Window Functions — Shape, Spectrum & Trade-offs

Full mathematical definitions for five window functions — Rectangular, Hann, Hamming, Blackman, and Gaussian (Eqs. 3.1–3.7) — with their Fourier Transforms and symbol tables. An interactive viewer shows each window's time-domain shape alongside its frequency response in dB. A comparison table summarises main lobe widths, sidelobe levels, decay rates, and recommended use cases.

### 4 · The Uncertainty Principle

The Heisenberg–Gabor inequality (Eq. 4.1) derived step-by-step via the Cauchy–Schwarz inequality, with every intermediate step annotated. An interactive visualisation tiles the time–frequency plane with STFT resolution cells — drag the window length slider to watch tiles reshape while their area stays constant. The minimum-uncertainty hyperbola (achieved only by the Gaussian window) is overlaid.

### 5 · Building the Spectrogram

The power spectrogram and its dB-scale form (Eqs. 5.1–5.2) defined precisely. An animated construction builds the spectrogram column by column as the analysis window hops through the signal, with the current window position highlighted on the waveform. Choose from three signal types and adjust the window size to see the resolution trade-off live.

### 6 · Discrete STFT & the DFT Connection

The discrete STFT equation (Eq. 6.1) with every index and parameter explained. Frequency resolution (Eq. 6.2), time resolution (Eq. 6.3), and the distinction between zero-padding interpolation and true resolution improvement (Eq. 6.4). Toggle N\_FFT and zero-padding factor to see DFT bins appear on the spectral envelope — more points on the same curve, not a narrower main lobe.

### 7 · Overlap, Hop Size & Time Resolution

The overlap formula (Eq. 7.1) and the Constant Overlap-Add (COLA) condition (Eq. 7.2) — the requirement for perfect reconstruction. An interactive checker draws overlapping windows and computes their sum in real time: green when COLA is satisfied, red when it isn't. Switch between Hann, rectangular, and Hamming windows to discover which overlap ratios work for each.

### 8 · Magnitude, Power & Phase Spectra

The polar decomposition of the complex STFT (Eq. 8.1), three common spectrogram scales (Eq. 8.2), and instantaneous frequency estimation from the phase derivative (Eq. 8.3) with phase unwrapping explained. Switch between magnitude, phase, and instantaneous frequency views of the same chirp signal to see the structured patterns hidden in the phase.

### 9 · Inverse STFT & Perfect Reconstruction

The Overlap-Add synthesis equation (Eq. 9.1), the Weighted Overlap-Add condition (Eq. 9.2), and Parseval's energy conservation relation (Eq. 9.3). An animated reconstruction shows individual IDFT frames being windowed, overlapped, and summed — converging to the original signal. The residual error (magnified 100×) confirms numerical precision.

### 10 · Real-World Applications

Welch's method for power spectral density estimation (Eq. 10.1). A continuously-running spectrogram displays three selectable signals: a pitched melody (horizontal bands), percussive transients (vertical splashes), and a mixed signal showing both patterns — demonstrating why the STFT is the universal front-end for audio, speech, vibration, radar, and biomedical signal analysis.

---

## Deploy to GitHub Pages

The project is a single `index.html` file.

**Option A — Repository root:**

```
git init stft-guide && cd stft-guide
# copy index.html into this directory
git add index.html
git commit -m "Initial commit"
git remote add origin git@github.com:yourusername/stft-guide.git
git push -u origin main
```

Then go to **Settings → Pages → Source → Deploy from a branch → `main` / `/ (root)`**.

**Option B — `docs/` folder:**

```
mkdir docs
cp index.html docs/
git add docs/
git commit -m "Add guide to docs/"
git push
```

Set Pages source to `main` / `/docs`.

Your guide will be live at `https://yourusername.github.io/stft-guide/`.

---

## Technical Details

* **Zero dependencies.** Pure HTML + Canvas + vanilla JavaScript. No npm, no bundler, no framework.
* **All maths computed in real-time.** DFT spectra, STFT spectrograms, phase unwrapping, instantaneous frequency, COLA sums, and inverse STFT reconstructions are all calculated on the fly — no pre-baked data.
* **20+ numbered equations.** Every equation includes a labelled symbol table explaining each variable, operator, and constant.
* **Responsive.** Canvases use `devicePixelRatio` scaling for crisp rendering on HiDPI displays.
* **Distinct design language.** Teal/cyan/emerald palette with Playfair Display + IBM Plex Mono typography, deliberately differentiated from the companion CQT guide (amber/coral) and Wavelet guide (blue/purple).

### What's computed where

| Visualisation | Algorithm | Notes |
| --- | --- | --- |
| Global FT spectrum | Naive DFT | O(N²), sufficient for N ≤ 2048 demo signals |
| STFT spectrogram | Windowed DFT with configurable hop | O(M·N·log N) equivalent; naive DFT used at demo scale |
| Window spectra | 1024-point DFT of zero-padded window | Centred, displayed in dB with configurable window type |
| Uncertainty tiling | Analytic Δt/Δf computation | Resolution cells drawn as rectangles; hyperbola from σ\_t·σ\_f = 1/(4π) |
| COLA verification | Sum of shifted windows | Real-time overlap-add sum with pass/fail indicator |
| Phase spectrogram | atan2(Im, Re) per STFT bin | HSL colour-mapped to [−π, π] |
| Instantaneous frequency | Phase difference + unwrapping | Eq. 8.3: f\_k + Δφ/(2πΔt) |
| Inverse STFT | IDFT + windowed overlap-add | Frame-by-frame reconstruction with residual error display |
| Welch PSD | Averaged |S[m,f]|² across frames | Demonstrated conceptually in Chapter 10 |

### Key constants

| Parameter | Default | Purpose |
| --- | --- | --- |
| Sample rate (f\_s) | 2000–4000 Hz | Sufficient for demo frequency ranges |
| Window length (N) | 64–256 | Adjustable per chapter via slider |
| Hop size (H) | N/4 (75% overlap) | Default for Hann window; adjustable in Ch. 7 |
| DFT size (N\_FFT) | N to 4N | Configurable zero-padding in Ch. 6 |
| Spectrogram dB range | 40 dB | Log-scale colour mapping |

### Equations index

| Eq. | Description | Chapter |
| --- | --- | --- |
| 1.1 | Fourier Transform | 1 |
| 2.1 | Continuous STFT (analysis equation) | 2 |
| 2.2 | STFT as Fourier Transform of windowed excerpt | 2 |
| 2.3 | STFT as inner product with time–frequency atoms | 2 |
| 3.1 | Rectangular window | 3 |
| 3.2 | FT of rectangular window (sinc) | 3 |
| 3.3 | Hann window | 3 |
| 3.4 | Gaussian window | 3 |
| 3.5 | FT of Gaussian window | 3 |
| 3.6 | Hamming window | 3 |
| 3.7 | Blackman window | 3 |
| 4.1 | Heisenberg–Gabor uncertainty inequality | 4 |
| 5.1 | Power spectrogram | 5 |
| 5.2 | Spectrogram in decibels | 5 |
| 6.1 | Discrete STFT | 6 |
| 6.2 | Frequency resolution | 6 |
| 6.3 | Time resolution | 6 |
| 6.4 | Zero-padding (interpolation, not resolution) | 6 |
| 7.1 | Overlap ratio | 7 |
| 7.2 | COLA condition | 7 |
| 8.1 | Polar form of the STFT | 8 |
| 8.2 | Three common spectrogram scales | 8 |
| 8.3 | Instantaneous frequency from phase difference | 8 |
| 9.1 | Inverse STFT (overlap-add) | 9 |
| 9.2 | WOLA perfect reconstruction condition | 9 |
| 9.3 | Parseval's relation for the STFT | 9 |
| 10.1 | Welch's method | 10 |

---

## Customisation

**Change the colour scheme** — edit the CSS custom properties:

```css
:root {
  --accent:  #4dd0b8;   /* teal         */
  --accent2: #2ecadf;   /* cyan         */
  --accent3: #7be87b;   /* green        */
  --accent4: #e8c84d;   /* gold         */
  --accent5: #e86b6b;   /* coral-red    */
}
```

**Change demo signals** — modify the `makeSignal()` function. Add new signal types by extending the `if/else` chain:

```javascript
else if (type === 'mySignal') {
  for (let i = 0; i < N; i++) {
    const t = i / fs;
    s[i] = /* your signal here */;
  }
}
```

**Add a chapter** — copy an existing `<section>` block, give it a new `id`, add a matching `<a>` in the table of contents, and write a new canvas + drawing IIFE at the bottom of the script.

**Change window functions** — extend `windowFunc()` with new types (Kaiser, flat-top, Tukey, etc.).

---

## Companion Guides

This guide is part of a series on signal transforms:

* [**The Constant-Q Transform — A Visual Guide**](https://brendanjameslynskey.github.io/ConstantQ-Transform/) — logarithmic frequency resolution for music analysis
* [**The Wavelet Transform — A Visual Guide**](https://brendanjameslynskey.github.io/WaveletTransformGuide/) — multiresolution analysis, DWT filter banks, and denoising

Each guide uses the same interactive approach (zero-dependency Canvas visualisations with real-time computation) but with a distinct colour palette and focus area.

---

## License

MIT — use it however you like. Attribution appreciated but not required.
