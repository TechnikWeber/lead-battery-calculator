# Lead battery life calculator (VRLA)

<p>
  <a href="https://technikweber.github.io/lead-battery-calculator/"><img alt="Live demo" src="https://img.shields.io/badge/live%20demo-open-brightgreen"></a>
  <img alt="Single HTML file, offline" src="https://img.shields.io/badge/single%20HTML%20file-offline-blue">
  <img alt="UI language: German" src="https://img.shields.io/badge/UI-Deutsch-informational">
  <a href="LICENSE"><img alt="License: MIT" src="https://img.shields.io/badge/license-MIT-green"></a>
</p>

A self-contained HTML tool for planning, evaluating and sanity-checking tests on lead-acid
VRLA batteries (AGM/gel). One file, nothing to install, runs offline in any modern browser.

👉 **[Open the calculator](https://technikweber.github.io/lead-battery-calculator/)**

## Background

The life of lead-acid batteries depends heavily on operating and test conditions. Data
sheet values usually apply at 25 °C — standard tests to IEC 60254-1, on the other hand, run
at 33–43 °C, which seemingly cuts cycle counts drastically (rule of thumb: life halves for
every ~7 °C above 25 °C).

Whoever tests batteries against each other in the lab also keeps comparing between:

- 25 °C marketing values
- 40 °C IEC compliance values
- their own test conditions

This is where the tool helps: it makes the relevant formulas (rule of thumb, IEC λ
correction, East Penn temperature compensation, IEEE 1188 Kt factors) usable directly in
the browser.

## What it does

Twelve modules, grouped by task:

### Life / cycle count

- **▶ Forward** — convert a data sheet value (25 °C) to a target temperature. Optionally
  including the effect of a charge voltage that is not lowered.
- **◀ Backward** — convert a test value at elevated temperature back to its 25 °C equivalent.
- **⊕ Combined** — vary depth of discharge, C-rate and temperature at the same time,
  starting from a known data sheet value.
- **≈ Estimate** — absolute estimate without a data sheet, anchored on a quality class or
  your own reference.

### Test and evaluation aids

- **⏱ Test duration** — cycles × time per cycle + utilisation → test duration. Shows the time
  saved by a higher test temperature directly.
- **⚡ Charge voltage** — temperature-compensated charge voltage according to East Penn
  (3 mV/°C/cell).
- **🌡 Capacity (Kt)** — capacity correction according to IEEE 1188 (East Penn appendix F,
  interpolated in between).
- **↻ C-rate** — conversion between Ah, hours and amperes.

### Specifically for ISO 7176-25 / IEC 60254-1

- **λ Capacity (IEC)** — IEC 60254-1 capacity correction `Ca = C / [1 + λ₁(t₀−tr)]`
  (λ₁ = 0.006/°C for a 5 h discharge).
- **⚠ Charge voltage check** — compares the charge voltage prescribed by the standard with
  the compensated target and estimates the damage factor from overcharging (current doubles
  per 0.05 V/cell).
- **✓ Acceptance** — checks Ca against all CN criteria (≥ 0.85·CN at cycle 1, ≥ 1.00·CN by
  cycle 10, stop below 0.80·CN, ≥ 300 cycles).
- **⏲ ISO test time** — projection with 50-cycle series and measurement overhead.

## How to use it

Open the [live version](https://technikweber.github.io/lead-battery-calculator/), or
download `index.html` and open it in a browser (double-click is enough). At the top, set
the halving interval and reference temperature, choose a tab, calculate.

All inputs are evaluated live. Every module shows the calculation and a range, so that it
stays visible that these are approximations. The interface is in German.

## Formula sources

| Module | Source |
|---|---|
| Temperature rule of thumb | Arrhenius simplification, Battery University, EMSYS |
| Charge voltage temperature compensation | East Penn *8A & 8G Battery Installation Instructions*, appendix B |
| Kt factors | IEEE 1188-2005, East Penn appendix F |
| λ capacity correction | IEC 60254-1 |
| Acceptance criteria | ISO 7176-25:2013, IEC 60254-1 section 5.5 |
| Overcharge model | Multitel, EMSYS (current doubles per +0.05 V/cell or +10 °C) |

## Limitations

- Rules of thumb are approximations, not exact constants of nature — cell type and
  manufacturer affect the real values.
- For a relative comparison of several batteries under identical conditions, no conversion
  is needed — the tool does not replace a test, it puts results into context.
- Applies to lead-acid VRLA (AGM/gel). For lithium-ion see the sister tool
  [liion-cell-calculator](https://github.com/TechnikWeber/liion-cell-calculator).
- Calendar ageing is a separate, parallel life limit and not part of the cycle formulas.

## Technology

- One HTML file (~30 KB)
- Vanilla JavaScript, no build, no dependencies
- No tracking, no external calls — runs fully offline

## License

[MIT](LICENSE). Use at your own risk; the tool does not replace a professional review.
