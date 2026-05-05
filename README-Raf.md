# RAF Victim Compensation Calculator

A web-based actuarial compensation estimator built on the **Road Accident Fund Act 56 of 1996** framework. The tool models the three pillars of a RAF claim — general damages, loss of earnings, and medical costs — using real mathematical and actuarial methods applied in South African courts.

Built as a portfolio project for a BSc Computer Science with Mathematics graduate applying to the Road Accident Fund internship programme.

---

## Live demo

Open `index.html` in any modern browser. No build step, no dependencies, no server required.

To host it publicly (free):
1. Push this repo to GitHub
2. Go to **Settings → Pages → Source → main branch / root**
3. Your calculator will be live at `https://Masego999.github.io/raf-compensation-calculator`

---

## What it calculates

The RAF compensates victims of road accidents across three heads of damages:

### 1. General damages
Non-economic compensation for pain, suffering, loss of amenity, and loss of enjoyment of life. Calculated using the **HPCSA whole-person impairment (WPI) scale**, which rates permanent bodily function loss as a percentage of total body function.

Under the 2008 amendment to the RAF Act, general damages are only claimable if WPI exceeds **30%**, or if the injury meets one of the narrative test categories (serious long-term impairment of a body function, permanent serious disfigurement, severe long-term mental or behavioural disturbance, or loss of a foetus).

The calculator applies a severity-banded base value (derived from the *Quantum of Damages* reference used by SA courts), scaled by WPI percentage and an age factor — since younger claimants suffer over a longer period.

```
GD = base(severity) × f(WPI) × f(age)
```

### 2. Loss of earnings — actuarial present value

The core actuarial calculation: what lump sum today equals all future income the claimant will never earn?

```
PV_earnings = Σ [E₀ × (1 + g)^t × ec_loss / (1 + r)^t]   for t = 1 to retirement year
```

| Variable | Description |
|---|---|
| `E₀` | Current annual gross income |
| `g` | Annual income growth rate (typically CPI + productivity ≈ 5–6%) |
| `ec_loss` | Fraction of earning capacity permanently lost |
| `r` | Real discount rate (courts typically use 2.5%) |
| `t` | Year index from now to retirement |

Both parties to a claim appoint actuaries. The discount rate and income growth assumptions are the principal points of expert dispute, since small differences compound significantly over a 20–35 year working horizon.

### 3. Future medical and care costs

Estimated by an occupational therapist or industrial psychologist, then discounted to present value over the claimant's remaining life expectancy (based on gender-adjusted SA life tables, with optional offset):

```
PV_futmed = Σ [annual_care_cost / (1 + r)^t]   for t = 1 to life expectancy
```

### 4. Contingency deduction

After summing all three heads, a contingency deduction (typically 10–25%) is applied to account for inherent uncertainty: the claimant might have been retrenched independently, mortality may differ from expectation, income projections carry uncertainty. Plaintiffs argue for lower deductions; RAF argues for higher ones.

```
Net award = (GD + PV_earnings + past_medical + PV_futmed) × (1 − contingency)
```

---

## Technical implementation

| Concern | Approach |
|---|---|
| **Language** | Vanilla JavaScript — no frameworks, no build tools |
| **Actuarial PV** | Year-by-year discounted cash flow loop (exact, not annuity approximation) |
| **Life tables** | Gender-adjusted SA life expectancy (male 67, female 73) with user offset |
| **HPCSA impairment** | Severity-banded base values scaled by WPI% and age factor |
| **Typography** | DM Serif Display + Outfit + DM Mono (Google Fonts) |
| **Styling** | Pure CSS — CSS variables, grid, no external libraries |
| **Hosting** | Single HTML file — deployable on GitHub Pages, Netlify, or any static host |

---

## Project structure

```
raf-compensation-calculator/
├── index.html      # Complete application (HTML + CSS + JS, single file)
└── README.md       # This file
```

---

## Formula reference

| Formula | Expression |
|---|---|
| Income at year t | `E(t) = E₀ × (1 + g)^t × ec_fraction` |
| PV of earnings loss | `PV = Σ E(t) / (1 + r)^t` |
| PV of future medical | `PV = Σ annual_cost / (1 + r)^t` |
| General damages | `GD = base(severity) × (WPI/100) × age_factor` |
| Net award | `Award = (GD + PV_E + past_med + PV_futmed) × (1 − c)` |

---

## Legal framework

- **Road Accident Fund Act 56 of 1996** — governs RAF compensation in South Africa
- **RAF Amendment Act 19 of 2005** — introduced the serious injury threshold (30% WPI) for general damages
- **HPCSA Serious Injury Assessment Guidelines** — basis for whole-person impairment rating
- **Quantum of Damages** (J.P. van der Walt et al.) — South African courts' reference for general damages benchmarking

---

## Limitations & disclaimers

This tool produces indicative estimates for educational purposes only. It does not constitute legal or financial advice. Actual RAF claims are assessed by:

- **Actuaries** — for loss of earnings and future cost calculations
- **Medical practitioners** — for HPCSA WPI rating and serious injury assessment
- **Occupational therapists** — for future care cost estimates
- **RAF attorneys** — for legal strategy and claim submission

Results will differ from actual RAF assessments based on individual medical evidence, expert reports, negotiation, and judicial discretion.

---

## Author

Developed by Masego Kotlhai 
[segokotlhai21@gmail.com] · [github.com/Masego999]
