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

- **Logo** — `FOURIER.SERIES` in the Orbitron monospace font, styled in the site's primary green.
- **Nav Links** — Six anchor links (Concept, Wave Builder, Noise Cancel, Freq Domain, Quiz) that smooth-scroll to each module section. Links are uppercase, spaced, and subtle — they highlight green on hover.
- **Background** — Semi-transparent dark background with `backdrop-filter: blur(12px)`, so the animated hero canvas shows through as you scroll past it.

### Hero Section

The full-viewport hero contains:

- A **live animated canvas** spanning the full width — it continuously draws three individual harmonic sine waves (at frequencies 1, 3, and 5) in green, cyan, and amber at low opacity, then overlays their composite sum as a bold bright-green wave on top. The entire wave system scrolls continuously in real time.
- A **status badge** (blinking green dot + "INTERACTIVE LEARNING PLATFORM · SIGNAL PROCESSING") to immediately communicate the site's purpose.
- The **headline** — "Decompose / Any Signal / with Fourier Series" — rendered in the large Orbitron display font with the second line as an outline-only stroke effect.
- **Two CTA buttons** — "Launch Wave Builder" (filled green) and "Learn the Theory" (outlined green).
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

Below the main formula, the three Fourier coefficient integrals are displayed:

| Coefficient | Formula |
|---|---|
| a₀ | (1/L) ∫_c^(c+2L) f(x) dx |
| aₙ | (1/L) ∫_c^(c+2L) f(x) cos(nπx/L) dx |
| bₙ | (1/L) ∫_c^(c+2L) f(x) sin(nπx/L) dx |

The interval is defined as **[c, c + 2L]** where L is the half-period and c is the start of the interval.

The formula card uses a dark background with `border: 1px solid rgba(32,200,140,0.15)`, with coefficients color-coded: a₀ in green, aₙ in amber, bₙ in cyan.

#### Live Decomposition Canvas

A side-by-side canvas next to the formula continuously animates the individual harmonics (n = 1 through 5) as faded colored lines with their composite sum drawn as a bold green overlay — showing in real time how harmonics combine.

#### Three-Step Process Cards

Three cards explain the conceptual pipeline:
1. **Identify the Period** — fundamental frequency f₀ = 1/T
2. **Compute Coefficients** — role of aₙ and bₙ
3. **Reconstruct or Filter** — retain or cancel harmonics

Each card has a large faded step number as a background decoration, a small icon box, and a short explanation paragraph.

#### Harmonic Decomposition Demo

A tabbed canvas demo at the bottom of the section shows a **square wave being approximated by Fourier Series** with selectable harmonic counts:

| Tab | Terms Used |
|---|---|
| n=1 | Fundamental only |
| n=3 | First 2 odd harmonics |
| n=5 | First 3 odd harmonics |
| n=9 | First 5 odd harmonics |
| n=19 | First 10 odd harmonics |
| n=49 | First 25 odd harmonics |

The canvas simultaneously draws:
- Individual harmonics as faded cyan lines
- The ideal square wave as a dim ghost
- The Fourier approximation as a bright green line

Clicking a tab immediately switches the number of terms, making the Gibbs phenomenon and convergence visible in real time.

---

### Module 02 — Wave Builder

**Tag:** `Module 02 — Interactive`

The Wave Builder is a **real-time Fourier synthesizer**. Users adjust harmonic amplitudes and instantly see the resulting composite waveform and frequency spectrum.

#### Layout

Two-column layout:
- **Left column** — Controls (harmonic sliders, wave type selector, stats, THD info)
- **Right column** — Live canvases (composite wave + spectrum)

#### Wave Type Presets

Four preset buttons at the top of the controls column:

| Preset | Harmonics | Amplitude Rule |
|---|---|---|
| **Square** *(default)* | n = 1, 3, 5, 7, 9 | 1/n |
| **Sawtooth** | n = 1 through 10 | 1/n |
| **Triangle** | n = 1, 3, 5, 7, 9 | 1/n² |
| **Custom** | Starts at n=1 only | User-defined |

The active preset is highlighted in the site's green. The site opens on **Square** by default.

#### Harmonic Amplitude Sliders

Each harmonic is rendered as a row inside a card:

- **Harmonic label** — shows `n = X` in the harmonic's assigned color
- **Amplitude value** — displayed as a decimal (e.g. `0.33`) and updates live as the slider moves
- **Range slider** — spans 0.00 to 1.00
- **Remove button (✕)** — appears on every row except when only one harmonic remains; clicking removes that harmonic from the list

Each harmonic is assigned a color from a rotating 5-color palette: green → cyan → amber → purple → red. This same color is used in the composite wave canvas to draw the individual harmonic component.

#### Add Harmonic Button

A dashed-border button below the slider list adds the **next missing harmonic** in sequential order (n = 1, 2, 3, … 10). If a harmonic has been removed from the middle, clicking Add inserts the first missing value and keeps the list sorted. When all 10 harmonics (n = 1 through n = 10) are present, the button is replaced by a note: *"Maximum 10 harmonics reached"*.

#### Stats Row

Three stat pills display live metrics:

| Pill | Color | Metric |
|---|---|---|
| Harmonics | Green | Count of active harmonics |
| Peak Amp | Cyan | Maximum amplitude across all harmonics |
| THD % | Amber | Total Harmonic Distortion percentage |

#### THD % Info Card

Directly below the stats row, an amber-tinted card explains Total Harmonic Distortion:

- Plain-language definition
- Formula: `THD = √(a₂² + a₃² + … + aₙ²) / a₁ × 100%`
- Four reference thresholds (0%, <1%, 1–5%, >10%) so students can interpret the live number

#### Composite Wave Canvas

Full-width canvas showing:
- Individual harmonic components as faded colored lines (matching each slider's color)
- The composite Fourier sum as a bright green line on top
- All waves scroll continuously in real time

Labeled **"COMPOSITE WAVE f(t)"** in the top-left corner.

#### Frequency Spectrum Canvas

Below the wave canvas, vertical bars represent each harmonic's amplitude at its harmonic index. Bar fill uses the harmonic's assigned color at low opacity; a solid color cap tops each bar. The harmonic index (n=X) and amplitude value are labeled above and below each bar.

Labeled **"FREQUENCY SPECTRUM"**.

---

### Module 03 — Noise Cancellation

**Tag:** `Module 03 — Core Application`

This is the primary demonstration module. It shows the **full noise cancellation pipeline** — from a clean signal being polluted by noise, through Fourier analysis and anti-phase generation, to the recovered clean signal.

#### How It Works Banner

A full-width cyan-tinted info box above all controls explains the process in plain language:

> The noisy signal (red) is analyzed using Fourier coefficients. Frequencies belonging to the noise band are identified in the spectrum. An anti-phase signal is generated and added — canceling the noise and recovering the clean signal (green).

#### Layout

Two-column layout:
- **Left column** — Controls and SNR meter
- **Right column** — Four live signal canvases

#### Control Sliders

Four sliders inside bordered control-group cards:

| Slider | Range | Effect |
|---|---|---|
| **Noise Amplitude** | 0.00 – 1.00 | How much noise is added to the clean signal |
| **Noise Freq Band** | 1 – 20 Hz | The frequency at which noise is introduced |
| **Harmonics to Cancel** | 1 – 20 | How many Fourier terms are used to model the noise |
| **Cancellation Strength** | 0 – 100% | How completely the anti-phase signal cancels the noise |

Each slider shows its current value in green next to the label and updates live.

#### SNR Meter

A live Signal-to-Noise Ratio display inside a card:

- Large numeric readout in Orbitron font (shows `∞` when noise is fully cancelled)
- A horizontal progress bar (dark track, green fill) showing SNR from −20 dB to +60 dB
- Scale labels at both ends

The SNR is computed from the noise amplitude and cancellation strength in real time.

#### Four Signal Canvases

Stacked vertically in the right column, each with a colored dot label:

| Canvas | Dot Color | What It Shows |
|---|---|---|
| **Clean Signal** | Green | The original, noise-free signal |
| **Noisy Signal** | Red | Clean signal + noise added |
| **Noise Component** | Cyan | The isolated noise wave |
| **Recovered Signal** | Green | After Fourier-based noise cancellation |

The Recovered Signal canvas has a green-tinted border to visually distinguish it as the output. All four canvases animate simultaneously and respond instantly to slider changes.

---

### Module 04 — Time vs Frequency Domain

**Tag:** `Module 04 — Visualization`

This section demonstrates why the frequency domain is more powerful for signal analysis and noise identification.

#### Side-by-Side Domain Canvases

Two cards side by side:

- **Time Domain** — Shows a composite waveform (three harmonics) animating continuously. A description below explains why noise is hard to identify from this view.
- **Frequency Domain** — Shows vertical frequency bars for each harmonic (1 Hz, 3 Hz, 7 Hz). Each spike is color-coded and labeled with its frequency. A description explains how noise appears as unexpected spikes that are easy to identify and remove.

#### Phasor Animation Card

A wide card below the domain comparison contains:

- A **280×280 canvas** showing animated rotating phasors (vectors) chained tip-to-tail:
  - **Green** phasor — fundamental (n=1), amplitude 1.0
  - **Cyan** phasor — 3rd harmonic (n=3), amplitude 0.333
  - **Amber** phasor — 5th harmonic (n=5), amplitude 0.2
- The endpoint of the last phasor traces a **red path** on the right side of the canvas, showing how the Fourier series approximation is drawn by the combined rotation
- A dashed line connects the phasor tip to its trace point in real time
- Faint circles show the radius of each phasor's rotation

Controls:
- **Pause / Play button** — toggles animation
- **Reset button** — clears the trace path and restarts from angle 0

An explanatory text panel beside the canvas describes phasors in plain language, identifying each color's harmonic.

---

### Module 05 — Real-World Applications

**Tag:** `Module 05`

Six application cards in a 3-column grid, each linking a real-world technology to Fourier Series:

| Icon | Application | Key Point |
|---|---|---|
| 🎧 | **Active Noise Cancellation** | ANC headphones sample, analyze, and invert noise |
| 📡 | **Radio & Wireless** | AM/FM modulation and channel filtering |
| 🏥 | **Medical Imaging (MRI)** | k-space data reconstructed via inverse Fourier transform |
| 🎵 | **Audio Compression (MP3)** | MDCT discards inaudible frequency components |
| 📸 | **Image Compression (JPEG)** | DCT applied to 8×8 pixel blocks |
| ⚡ | **Power Grid Analysis** | Harmonic distortion identified and cancelled |

Each card has a hover effect — upward lift, a subtle green glow border, and a radial green gradient overlay from the top-left corner.

---

### Module 06 — Faculty Quiz Bank

**Tag:** `Module 06 — Test Knowledge`

A **randomized multiple-choice quiz** with 10 moderate-difficulty questions designed for First Year engineering students.

#### Randomization

Every time the quiz loads (or the **Reshuffle ↺** button is clicked), two levels of shuffling occur:

1. **Question order** — all 10 questions are shuffled using Fisher-Yates; faculty see a different sequence every session.
2. **Option order** — within each question, the four answer choices are independently shuffled; the answer position (A/B/C/D) changes every time.

This prevents answer-pattern memorization and makes the quiz genuinely challenging on repeat.

#### Question Design

All 10 questions are at **moderate difficulty** — they require applying a concept rather than just recalling a definition. Question types include:

- Numerical calculation (f = 1/T, nth harmonic frequency)
- Signal reading (identifying harmonics from a Fourier expression)
- Conceptual inference (effect of scaling amplitudes, anti-phase condition)
- Comparative reasoning (time domain vs frequency domain)
- Applied understanding (SNR change after noise reduction)

Answer choices are carefully written so no single option is obviously correct from its length alone.

#### Quiz UI

- **Progress bar** — a row of small colored segments at the top showing completed, current, and upcoming questions
- **Question text** — rendered in a slightly larger weight
- **Four option buttons** — each labeled A–D; clicking locks in the answer and disables all other options
- **Feedback panel** — hidden until an answer is selected; reveals a plain-English explanation of why the correct answer is right
- **Score tracker** — shows "Score: X / Y" live, updating after each answer
- **Next → button** — disabled (faded) until an answer is selected, then activates
- **Reshuffle ↺ button** — appears on the last question after answering; resets score and rebuilds the deck with a fresh shuffle

---

## Global UI Components

### Back to Top Button

A fixed button in the bottom-right corner appears after scrolling 300px down:

- 42×42 px, dark background, 2px solid green border
- Upward chevron arrow icon (SVG)
- Green glow box-shadow
- Fades in and slides up from below when it appears; fades out when scrolling back to top
- On hover — green tint fills the background, glow intensifies
- On click — smooth scrolls to the top of the page

### Custom Scrollbar

The browser scrollbar is styled to match the site palette:
- **Track** — dark secondary background (`#0d1420`)
- **Thumb** — site green (`#20c88c`), 10px wide, rounded
- **Hover** — slightly brighter green (`#28e89e`)
- Supported via both `scrollbar-color` (Firefox) and `::-webkit-scrollbar` (Chrome/Edge/Safari)

### CRT Scan-Line Overlay

A fixed `::before` pseudo-element with `position: fixed; inset: 0` covers the entire viewport with a subtle repeating horizontal line pattern at very low opacity. It adds depth and reinforces the oscilloscope aesthetic without being distracting.

### Grid Background

The page body has a four-layer background grid:
- Two layers of 60×60px major grid lines in faint green
- Two layers of 12×12px minor grid lines at even lower opacity

### Scroll Fade-In Animations

Every section and most cards carry the `.fade-in` class. An `IntersectionObserver` watches all `.fade-in` elements and adds `.visible` when they enter the viewport, triggering a CSS transition from `opacity: 0; transform: translateY(24px)` to `opacity: 1; transform: none`. Threshold is 10% visibility.

### Browser Tab Favicon

An inline SVG favicon is embedded in the `<head>` — a dark rounded-corner square with "**FS**" in bold green, matching the website's brand color exactly.

---

## Typography

| Role | Font | Weight | Usage |
|---|---|---|---|
| Display / Headings | Orbitron | 400, 700, 900 | Hero title, section h2, logo, stat values |
| Monospace / Labels | Space Mono | 400, 700 | Section tags, canvas labels, formula, quiz progress |
| Body / UI | DM Sans | 300, 400, 500, 600 | Paragraphs, buttons, descriptions, card text |

All three fonts are loaded from Google Fonts.

---

## Color System

All colors are defined as CSS custom properties on `:root`:

| Variable | Hex | Usage |
|---|---|---|
| `--bg` | `#080c12` | Page background |
| `--bg2` | `#0d1420` | Canvas backgrounds, scrollbar track |
| `--bg3` | `#111927` | Tertiary backgrounds |
| `--green` | `#20c88c` | Primary accent — borders, labels, waveforms, buttons |
| `--green-dim` | `rgba(32,200,140,0.4)` | Hover borders, glow effects |
| `--cyan` | `#00e5ff` | Secondary accent — noise component, bₙ coefficient |
| `--amber` | `#ffb830` | Tertiary accent — aₙ coefficient, THD |
| `--red` | `#ff4d6a` | Noisy signal, wrong quiz answer, phasor trace |
| `--purple` | `#b388ff` | 4th harmonic color |
| `--text` | `#e8f0ea` | Primary text |
| `--text-muted` | `#7a9b8a` | Secondary text, descriptions |
| `--text-dim` | `#3d5a47` | Disabled states, scale labels |
| `--border` | `rgba(32,200,140,0.15)` | Card and element borders |

---
