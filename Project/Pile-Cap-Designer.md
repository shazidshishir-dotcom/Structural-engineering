# Pile Cap Designer (BNBC-2020)

An interactive, browser-based **pile-cap design tool** to **BNBC-2020 (Part 6)**
and **ACI 318-14**. Enter the **pile count** and the **column load** — the cap
**plan size, thickness and reinforcement** come out automatically, including a
full strut-and-tie check per **BNBC Appendix I**.

---

## What it does

You provide two things — **number of piles** and **load** — plus the usual
pile/column/material parameters (sensible BNBC defaults are pre-filled). The tool
sizes and checks the cap and reports:

- **Cap plan size** `B × L` (metres and ft-in), on a 3-inch grid.
- **Thickness** `h` and effective depth `d`, grown until every check passes.
- **Bottom reinforcement** both ways (`n–Ø @ spacing`, mm and inches).
- **Per-pile loads** (service) vs allowable compression `Qa` / uplift `Qt`.
- **Governing design ratios** for every limit state, each flagged pass/fail.
- A plan and a section sketch of the cap, piles and column.

### Load cases
- **Axial only** — vertical service load `P`; moments assumed carried by tie/grade beams.
- **Axial + moment** — `P`, `Mx`, `My`; distributed to the piles by the rigid-cap
  relation `P/n ± M·x/Σx²`, so the corner piles and the required steel follow the moment.

### Pile groups
Standard symmetric layouts for **1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 16** piles
(triangle for 3, quincunx for 5, hexagon-plus-centre for 7, rectangular grids
otherwise), at a user spacing (default 3Ø).

---

## Design basis

| Item | Basis |
|---|---|
| Pile count & cap plan | Unfactored **service** load + cap/backfill weight, each pile `≤ Qa`, `≥ −Qt` |
| Load for cap design | Factored `Pu = LF·P` (default LF 1.5) |
| Two-way (punching) shear | ACI 22.6 with `γv·Mu` moment transfer (BNBC Part 6 Ch. 6) |
| One-way shear | At `d` from the column faces, `φ·0.17·√f′c·b·d` |
| Pile punching | Interior / edge / corner critical perimeter around each pile |
| Flexure | At the column faces, `φ = 0.90`, tension-controlled |
| Strut-and-tie | **BNBC Appendix I** — 3-D model, `θ ≥ 25°`, bottle strut `βs 0.60`, pile-node `βn` (CCT/CTT), column node bearing |
| Reinforcement | `max(flexure As, minimum steel, STM tie force / φfy)` |
| Minimum steel | `0.0018·420/fy` (≥ 0.0014), BNBC §8.1 / ACI 24.4.3.2 |
| Cover | 150 mm effective in cap (75 mm pile embedment + 75 mm cover, cast against earth, §20.6.1.3.1) |
| φ factors | 0.75 shear / strut-and-tie, 0.90 flexure |

Pile loads use the **rigid-cap** assumption. Thickness is iterated in 1-inch
steps from the minimum until punching, one-way shear, pile punching, flexure
**and** the strut-and-tie model are all satisfied.

---

## Engine provenance & validation

The browser engine is a **faithful JavaScript port** of the author's validated
Python engine `etabs_dashboard/foundation.py` (`pile_cap`, `stm_pile_cap`,
`pile_layout`, punching / flexure / bar-detailing helpers). It was checked to
reproduce the Python engine **exactly** — identical cap `B/L`, thickness `h`,
flexural moments `Mu`, bar count / diameter / spacing, and strut-and-tie `θmin`
and strut/pile-node ratios — across pile groups from 1 to 16, with and without
moments. The only intentional difference: when the piles are overloaded, the web
tool still sizes the cap and flags the overload (suggesting the minimum group
that works) instead of returning nothing.

---

## Technical notes

- **Single self-contained file.** All logic, styling and SVG sketches are inside
  one `index.html` (~42 KB). No build step, no external data, works offline.
- **Responsive**, light/dark theme, tabular-numeric result tables.
- **Stack:** vanilla JS, inline SVG, CSS custom properties.

---

## Limitations & engineering notes

- **Preliminary** design: rigid-cap, **single load case** — not a
  multi-combination envelope. Run the governing combinations and confirm with a
  detailed model (e.g. SAFE) before drawings.
- Not included: pile **structural** capacity, group efficiency beyond a user
  factor, negative skin friction, settlement, liquefaction, and grade-beam design
  for base moments.
- Corner / edge column caps and re-entrant (L / C / core) layouts need a separate
  deep-beam / strut-and-tie detailing check by hand.

---

## Standards & credits

- Design to **BNBC-2020 (Part 6)** and **ACI 318-14**, strut-and-tie per **BNBC
  Appendix I**.
- Engine ported from the author's `etabs_dashboard` foundation module.
