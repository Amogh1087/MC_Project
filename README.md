# Fourier Series — Signal & Noise

An interactive, single-page educational website built for teaching **Fourier Series and Noise Cancellation** to First Year engineering students. Designed to be used by faculty during lectures, lab sessions, or self-study — the site combines animated visualizations, real-time signal manipulation, and a randomized quiz bank in a single HTML file with zero dependencies.

---

## Table of Contents

- [Design Language](#design-language)
- [Layout & Navigation](#layout--navigation)
- [Module Breakdown](#module-breakdown)
  - [Module 01 — Concept](#module-01--concept)
  - [Module 02 — Wave Builder](#module-02--wave-builder)
  - [Module 03 — Noise Cancellation](#module-03--noise-cancellation)
  - [Module 04 — Time vs Frequency Domain](#module-04--time-vs-frequency-domain)
  - [Module 05 — Real-World Applications](#module-05--real-world-applications)
  - [Module 06 — Faculty Quiz Bank](#module-06--faculty-quiz-bank)
- [Global UI Components](#global-ui-components)
- [Typography](#typography)
- [Color System](#color-system)

---

## Design Language

The website uses an **oscilloscope / signal-lab aesthetic** — a dark background reminiscent of a CRT display, phosphor-green waveforms, a fine grid overlay, and a subtle CRT scan-line effect layered over the entire page. The intent is to make the interface feel like the subject matter: a digital signal environment.

Every interactive component is designed around a single visual metaphor — **you are inside a signal processing lab**. Canvas-drawn waveforms animate in real time, frequency bars pulse with the signal, and phasors rotate continuously. The site never feels static.

Key design decisions:

- **No external JavaScript libraries** — all animations, canvases, and interactions are written in vanilla JS.
- **Single HTML file** — the entire site (HTML, CSS, JS) is self-contained. No build step, no server required. Open in any browser.
- **Retina-ready canvases** — all `<canvas>` elements scale using `devicePixelRatio` so waveforms are crisp on high-DPI screens.
- **Scroll-triggered fade-ins** — every section fades and slides up into view using `IntersectionObserver`, keeping the page feel dynamic without being distracting.

---

## Layout & Navigation

### Fixed Navigation Bar

A sticky top navigation bar is present at all times. It contains:

- **Logo** — `Fourier.Series` in the Orbitron monospace font, styled in the site's primary green with a `.` separator.
- **Nav Links** — Five anchor links (Concept, Wave Builder, Noise Cancel, Freq Domain, Quiz) that smooth-scroll to each module section. Links are uppercase, spaced, and subtle — they highlight green on hover.
- **Background** — Semi-transparent dark background (`rgba(8,12,18,0.85)`) with `backdrop-filter: blur(12px)`, so the animated hero canvas shows through as you scroll past it.

### Hero Section

The full-viewport hero is **fully centre-aligned** — badge, headline, description, and CTA buttons all stack on the central axis. It contains:

- A **live animated canvas** spanning the full width — it continuously draws three individual harmonic sine waves (at frequencies 1, 3, and 5) in green, cyan, and amber at low opacity, then overlays their composite sum as a bold bright-green wave on top. The entire wave system scrolls continuously in real time.
- A **status badge** (blinking green dot + "INTERACTIVE LEARNING PLATFORM · SIGNAL PROCESSING") to immediately communicate the site's purpose.
- The **headline** — "Decompose / Any Signal / with Fourier Series" — rendered in the large Orbitron display font.
- **Two CTA buttons**, centred, in order of intended student flow:
  1. **Learn the Theory** (filled green) — links to Module 01
  2. **Launch Wave Builder** (outlined green) — links to Module 02
- A **canvas label** in the top-left corner of the canvas reading "LIVE SIGNAL — ω(t)".

### Section Separators

Thin horizontal rules between every section use a linear gradient — transparent → green-tinted border → transparent — giving a soft, glowing divider effect that matches the theme.

### Footer

Minimal one-line footer with just the `FOURIER.SERIES` logo mark on a semi-transparent dark bar. No clutter.

---

## Module Breakdown

### Module 01 — Concept

**Tag:** `Module 01`

This section explains the mathematical foundation of Fourier Series at a visual and conceptual level.

#### Formula Card

A dedicated formula display card shows the full **general-interval Fourier Series formula**:

```
f(x) = a₀/2 + Σ [ aₙ cos(nπx/L) + bₙ sin(nπx/L) ]
```

Below the main formula, the "where" clause and three Fourier coefficient integrals are displayed with increased font sizes for readability:

| Coefficient | Formula |
|---|---|
| a₀ | (1/L) ∫_c^(c+2L) f(x) dx |
| aₙ | (1/L) ∫_c^(c+2L) f(x) cos(nπx/L) dx |
| bₙ | (1/L) ∫_c^(c+2L) f(x) sin(nπx/L) dx |

The interval is defined as **[c, c + 2L]** where L is the half-period and c is the start of the interval. A formula note below reads: *"L is the half-period · c is the start of the interval · a₀/2 is the DC (mean) value"*.

Coefficients are colour-coded: a₀ in green, aₙ in amber, bₙ in cyan. Integration bounds (sub/sup) are rendered at 0.75rem for legibility.

#### Live Decomposition Canvas

A canvas to the right of the formula card matches the formula card's full height and continuously animates harmonics n = 1 through 5 as faded coloured lines, with their composite sum drawn as a bold green overlay — showing in real time how harmonics combine into a complex waveform.

#### Three-Step Process Cards

Three cards explain the conceptual pipeline:
1. **Identify the Period** — fundamental frequency f₀ = 1/T
2. **Compute Coefficients** — role of aₙ and bₙ
3. **Reconstruct or Filter** — retain or cancel harmonics

Each card has a large faded step number as a background decoration, a small icon box (`〜`, `∑`, `◈`), and a short explanation paragraph.

#### Harmonic Decomposition Demos

Three separate animated canvas cards, stacked vertically, each independently demonstrating Fourier approximation for a different wave type. All canvases are 280px tall.

**Square Wave** (green sum line)
- Only odd harmonics (n = 1, 3, 5, …), amplitudes 1/n
- Tabs: n=1, n=3, n=5, n=9, n=19
- Shows Gibbs overshoot near the sharp jumps at higher n

**Sawtooth Wave** (amber sum line)
- All harmonics (n = 1, 2, 3, …), alternating-sign amplitudes (−1)^(n+1)/n
- Tabs: n=1, n=3, n=5, n=9, n=19
- Both odd and even harmonics build the ramp shape

**Triangle Wave** (purple sum line)
- Only odd harmonics (n = 1, 3, 5, …), amplitudes 1/n²
- Tabs: n=1, n=3, n=5, n=9, n=19
- Converges much faster than square — smooth shape needs fewer terms

Each card simultaneously draws:
- Individual harmonics as faded cyan lines
- The ideal target wave as a dim ghost
- The Fourier approximation as a bold coloured line

Each card has its own independent tab row — clicking a tab switches harmonic count only for that wave type.

---

### Module 02 — Wave Builder

**Tag:** `Module 02 — Interactive`

The Wave Builder is a **real-time Fourier synthesizer** laid out as a professional engineering dashboard. Users adjust harmonic amplitudes and instantly see the resulting composite waveform and frequency spectrum.

#### Layout

Two-column dashboard grid (`360px` fixed left, `minmax(0, 1fr)` right), `align-items: stretch` so both columns share the same total height. The right column uses `justify-content: space-between` to pin the THD accordion to the bottom, aligning it with the bottom of the Harmonic Amplitudes card on the left.

#### Left Column — Control Console

Two cards stacked with `1rem` gap:

**Wave Type Card**

Four preset buttons arranged in a **2×2 grid** — equal width, equal height, individually bordered, `8px` radius:

| Preset | Harmonics | Amplitude Rule |
|---|---|---|
| **Square** *(default)* | n = 1, 3, 5, 7, 9 | 1/n |
| **Sawtooth** | n = 1 through 10 | 1/n |
| **Triangle** | n = 1, 3, 5, 7, 9 | 1/n² |
| **Custom** | Starts at n=1 only | User-defined |

The active preset highlights in green. The site opens on **Square** by default.

**Harmonic Amplitudes Card**

Each harmonic is rendered as a compact row (`0.75rem 1rem` padding, `0.5rem` gap between rows):

- **Harmonic label** — `n = X` in the harmonic's assigned colour
- **Amplitude value** — decimal display (e.g. `0.33`), updates live
- **Range slider** — 0.00 to 1.00
- **Remove button (✕)** — present on all rows except when only one harmonic remains

Each harmonic is assigned a colour from a rotating 5-colour palette: green → cyan → amber → purple → red. The same colour is used in all visualisations.

Below the list, a full-width **+ ADD HARMONIC** dashed-border button adds the next missing harmonic in sequential order (n = 1–10). When all 10 are present it shows *"Maximum 10 harmonics reached"*.

#### Right Column — Visualization Panel

**Composite Wave Canvas** (`height: 220`)

Full-width canvas showing:
- Individual harmonic components as faded coloured lines
- The composite Fourier sum as a bright green line on top
- All waves scroll continuously in real time

Labelled **"COMPOSITE WAVE f(t)"** in green in the top-left corner.

**Frequency Spectrum Canvas** (`height: 88`)

Vertical bars representing each harmonic's amplitude. Bar fill uses the harmonic's assigned colour at low opacity; a solid colour cap tops each bar. Amplitude values are labelled above each bar (inside the bar if the bar is very tall); harmonic index labels (`n=X`) sit at the bottom. A fixed `48px` top padding zone and `22px` bottom padding zone ensure labels never overlap the bar tops regardless of amplitude.

Labelled **"FREQUENCY SPECTRUM"** in cyan in the top-left corner.

**Telemetry Stats Row**

Three equal-width cards in a `3-column grid`, each individually colour-tinted:

| Card | Accent | Metric |
|---|---|---|
| Harmonics | Green | Count of active harmonics |
| Peak Amp | Cyan (tinted border + bg) | Maximum amplitude across all harmonics |
| THD % | Amber (tinted border + bg) | Total Harmonic Distortion percentage |

**THD Accordion Panel** *(collapsible, default: collapsed)*

An amber-tinted accordion pinned to the bottom of the right column. Collapsed state shows a fully centred header:

```
About THD %
Signal distortion analysis
    ▼
```

Clicking expands with a smooth `max-height` + `opacity` CSS transition (0.45s / 0.35s). The ▼ arrow rotates 180° when open. Expanded content contains:

- Plain-language explanation of Total Harmonic Distortion
- Centred formula block with "FORMULA" sub-label:
  `THD = √(a₂² + a₃² + … + aₙ²) / a₁ × 100%`
- "QUALITY LEVELS" section label
- Four interpretation rows, each as an individual card with an amber percentage badge:

| Badge | Interpretation |
|---|---|
| 0% | Pure sine wave — only the fundamental is present |
| <1% | Excellent quality — precision audio and power systems |
| 1–5% | Acceptable for most consumer electronics |
| >10% | Clearly audible distortion — square and sawtooth waves |

---

### Module 03 — Noise Cancellation

**Tag:** `Module 03 — Core Application`

This is the primary demonstration module showing the **full noise cancellation pipeline**.

#### How It Works Banner

A full-width cyan-tinted info box above all controls explains the process:

> The noisy signal (red) is analyzed using Fourier coefficients. Frequencies belonging to the noise band are identified in the spectrum. An anti-phase signal is generated and added — canceling the noise and recovering the clean signal (green).

#### Layout

Two-column layout — left controls, right signal canvases.

#### Control Sliders

Four sliders inside bordered control-group cards:

| Slider | Range | Default | Effect |
|---|---|---|---|
| **Noise Amplitude** | 0.00 – 1.00 | 0.50 | How much noise is added to the clean signal |
| **Noise Freq Band** | 1 – 20 Hz | 5 Hz | The frequency at which noise is introduced |
| **Harmonics to Cancel** | 1 – 20 | 5 | How many Fourier terms model the noise |
| **Cancellation Strength** | 0 – 100% | 100% | How completely the anti-phase signal cancels the noise |

Each slider shows its current value in green next to the label and updates live.

#### SNR Meter

A live Signal-to-Noise Ratio display inside a card:

- Large numeric readout in Orbitron font (shows `∞` dB when noise is fully cancelled)
- A horizontal progress bar (dark track, green fill) showing SNR from −20 dB to +60 dB
- Scale labels at both ends (`-20dB` / `+60dB`)

#### Four Signal Canvases

Stacked vertically in the right column, each with a coloured dot label:

| Canvas | Dot | Height | What It Shows |
|---|---|---|---|
| **Clean Signal** | Green | 120px | The original, noise-free signal |
| **Noisy Signal** | Red | 120px | Clean signal + added noise |
| **Noise Component** | Cyan | 80px | The isolated noise wave |
| **Recovered Signal** | Green | 120px | After Fourier-based noise cancellation |

The Recovered Signal canvas has a green-dimmed border (`--green-dim`) to visually distinguish it as the output. All four canvases animate simultaneously and respond instantly to slider changes.

---

### Module 04 — Time vs Frequency Domain

**Tag:** `Module 04 — Visualization`

#### Side-by-Side Domain Canvases

Two cards side by side (`200px` tall each):

- **Time Domain** — Composite waveform (three harmonics) animating continuously. Description below explains why noise is hard to identify from this view alone.
- **Frequency Domain** — Vertical frequency bars for each harmonic (1 Hz, 3 Hz, 7 Hz), colour-coded and labelled. Description explains how noise appears as unexpected spikes.

#### Phasor Animation Card

A full-width card containing a **280×280px canvas** showing animated rotating phasors chained tip-to-tail:

- **Green** phasor — fundamental (n=1), amplitude 1.0
- **Cyan** phasor — 3rd harmonic (n=3), amplitude 0.333
- **Amber** phasor — 5th harmonic (n=5), amplitude 0.2

The endpoint of the last phasor traces a **red path** on the right of the canvas. A dashed line connects the phasor tip to its trace point in real time. Faint radius circles show each phasor's rotation envelope.

Controls beside the canvas:
- **Pause / Play** button — toggles animation
- **Reset** button — clears the trace and restarts from angle 0

A text panel beside identifies each phasor colour and explains the chained-vector concept.

---

### Module 05 — Real-World Applications

**Tag:** `Module 05`

Six application cards in a 3-column responsive grid. Each card has a hover effect — upward lift, a subtle green glow border, and a radial green gradient overlay from the top-left corner.

| Icon | Application | Key Point |
|---|---|---|
| 🎧 | **Active Noise Cancellation** | ANC headphones sample, analyze, and invert noise in real time |
| 📡 | **Radio & Wireless** | AM/FM modulation and channel filtering rely on Fourier frequency isolation |
| 🏥 | **Medical Imaging (MRI)** | k-space data reconstructed via inverse Fourier transform |
| 🎵 | **Audio Compression (MP3)** | MDCT discards inaudible frequency components — ~90% size reduction |
| 📸 | **Image Compression (JPEG)** | DCT applied to 8×8 pixel blocks; dominant patterns encoded, rest discarded |
| ⚡ | **Power Grid Analysis** | Harmonic distortion identified with Fourier analysis; active filters cancel it |

---

### Module 06 — Faculty Quiz Bank

**Tag:** `Module 06 — Test Knowledge`

A **randomized multiple-choice quiz** with 10 moderate-difficulty questions designed for First Year engineering students.

#### Randomization

Every time the quiz loads (or the **Reshuffle ↺** button is clicked), two levels of shuffling occur via Fisher-Yates:

1. **Question order** — all 10 questions are reshuffled; faculty see a different sequence every session.
2. **Option order** — within each question, the four answer choices are independently shuffled; the answer position (A/B/C/D) changes every time.

This prevents answer-pattern memorization and makes the quiz genuinely challenging on repeat.

#### Question Design

All 10 questions are at **moderate difficulty** — applying a concept rather than recalling a definition. Question types include:

- Numerical calculation (f = 1/T, nth harmonic frequency)
- Signal reading (identifying harmonics from a Fourier expression)
- Conceptual inference (effect of scaling amplitudes, anti-phase condition)
- Comparative reasoning (time domain vs frequency domain)
- Applied understanding (SNR change after noise reduction)

#### Quiz UI

- **Progress bar** — a row of coloured segments showing completed (dim green), current (bright green), and upcoming (dark) questions
- **Question counter** — `QUESTION X / 10` in Space Mono above the progress bar
- **Question text** — rendered at a slightly larger weight
- **Four option buttons** — clicking locks in the answer and disables all other options
- **Feedback panel** — hidden until an answer is selected; reveals a plain-English explanation
- **Score tracker** — `Score: X / Y` displayed live, updating after each answer
- **Next → button** — disabled (faded) until an answer is selected, then activates
- **Reshuffle ↺ button** — appears on the last question after answering; resets score and rebuilds the deck

---

## Global UI Components

### Back to Top Button

A fixed button in the bottom-right corner appears after scrolling 300px down:

- 42×42px, dark background, 2px solid green border
- Upward chevron SVG icon
- Green glow box-shadow
- Fades in and slides up from below when it appears; fades out when scrolling back to top
- Hover — green tint fills the background, glow intensifies
- Click — smooth scrolls to the top of the page

### Custom Scrollbar

Styled to match the site palette:
- **Track** — dark secondary background (`#0d1420`)
- **Thumb** — site green (`#20c88c`), 10px wide, rounded (`border-radius: 3px`)
- **Hover** — slightly brighter green (`#28e89e`)
- Supported via `scrollbar-color` (Firefox) and `::-webkit-scrollbar` (Chrome/Edge/Safari)

### CRT Scan-Line Overlay

A fixed `.scan-overlay` element covers the entire viewport with a repeating horizontal line pattern at very low opacity, reinforcing the oscilloscope aesthetic across all content.

### Grid Background

The page body `::before` pseudo-element draws a four-layer background grid:
- Two layers of 60×60px major grid lines in faint green (`rgba(32,200,140,0.06)`)
- Two layers of 12×12px minor grid lines at even lower opacity (`rgba(32,200,140,0.02)`)

### Scroll Fade-In Animations

Every section and most cards carry the `.fade-in` class. An `IntersectionObserver` watches all `.fade-in` elements and adds `.visible` when they enter the viewport at 10% threshold, triggering a CSS transition from `opacity: 0; transform: translateY(24px)` to `opacity: 1; transform: none`.

### Browser Tab Favicon

An inline SVG favicon in the `<head>` — a dark rounded-corner square (`rx=6`) with **FS** in bold green (`#20c88c`), matching the brand colour exactly.

---

## Typography

| Role | Font | Weight | Usage |
|---|---|---|---|
| Display / Headings | Orbitron | 400, 700, 900 | Hero title, section h2, logo, stat values, SNR readout |
| Monospace / Labels | Space Mono | 400, 700 | Section tags, canvas labels, formula, card titles, quiz counter |
| Body / UI | DM Sans | 300, 400, 500, 600 | Paragraphs, buttons, descriptions, card text, slider labels |

All three fonts are loaded from Google Fonts.

---

## Color System

All colors are defined as CSS custom properties on `:root`:

| Variable | Value | Usage |
|---|---|---|
| `--bg` | `#080c12` | Page background |
| `--bg2` | `#0d1420` | Canvas backgrounds, scrollbar track |
| `--bg3` | `#111927` | Tertiary backgrounds |
| `--green` | `#20c88c` | Primary accent — borders, labels, waveforms, buttons |
| `--green-dim` | `rgba(32,200,140,0.4)` | Hover borders, recovered signal canvas border, glow effects |
| `--cyan` | `#00e5ff` | Secondary accent — noise component, bₙ coefficient, spectrum label |
| `--cyan-dim` | `rgba(0,229,255,0.35)` | Cyan hover and tint states |
| `--amber` | `#ffb830` | Tertiary accent — aₙ coefficient, THD panel, sawtooth sum line |
| `--amber-dim` | `rgba(255,184,48,0.35)` | Amber hover and tint states |
| `--red` | `#ff4d6a` | Noisy signal canvas, wrong quiz answer, phasor trace path |
| `--red-dim` | `rgba(255,77,106,0.35)` | Red hover and tint states |
| `--purple` | `#b388ff` | 4th harmonic color, triangle wave sum line |
| `--text` | `#e8f0ea` | Primary text |
| `--text-muted` | `#7a9b8a` | Secondary text, descriptions, canvas explanations |
| `--text-dim` | `#3d5a47` | Disabled states, scale labels, progress bar background |
| `--border` | `rgba(32,200,140,0.15)` | Card and element borders |
| `--card` | `rgba(13,20,32,0.85)` | Card background with backdrop blur |
| `--card-border` | `rgba(32,200,140,0.12)` | Card-specific border (slightly dimmer than `--border`) |

---

*README covers UI and structure. Mathematical/functional documentation to follow.*
