# Pile Cap Designer (BNBC-2020)

Preliminary pile-cap design to **BNBC-2020 (Part 6)** / **ACI 318-14**.
Give the **number of piles** and the **column load** — the tool returns the
**cap plan size, thickness and reinforcement** automatically.

- **Sizing** (pile count, cap plan) uses the **unfactored service load**.
- **Thickness & steel** use the **factored load** `Pu = LF·P`.
- Checks: column two-way (punching) shear with the `γv·Mu` moment-transfer
  stress, one-way shear at `d` from the column faces, corner/edge-pile punching,
  flexure at the column faces, and a **3-D strut-and-tie model (BNBC Appendix I:
  θ ≥ 25°, bottle strut βs 0.60, pile-node βn)**.
- Bottom reinforcement = `max(flexure, minimum steel, strut-and-tie ties)`.

### Features
- **Load combinations** — enter Dead / Live / Wind / Seismic cases (P, Mx, My);
  the tool auto-generates and envelopes the BNBC/ACI service (D, D+L, D±0.6W,
  D±0.7E, 0.6D±…) and strength (1.4D, 1.2D+1.6L, 1.2D+1.0L±1.0W/E, 0.9D±1.0W/E)
  combinations, W/E taken ±. Optional **+33% allowable (Qa/Qt) for W/E** combos.
- **Qa / Qt from an SPT soil report** (BNBC §3.10): editable borehole,
  bored/driven, layer-wise skin friction + end bearing, FS 3.5 and the 1.5/3.0
  partial check, uplift (0.7Qs+W)/FS — feeds the design automatically.
- **Top + bottom reinforcement**; bar Ø is user-selected (optional auto-upsize).
- **Triangular 3-pile cap** option (saves concrete).
- **Quantities & bar bending schedule**, steel/concrete volumes, optional cost.
- **Exports:** print/PDF calculation sheet, cap drawing (DXF), bar schedule (CSV).

Standard pile groups: 1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 16. Rigid-cap pile loads
`P/n ± M·x/Σx²`. Self-contained single `index.html` — no build step, works offline.

## Engine provenance
The browser engine is a faithful JavaScript port of the validated
`etabs_dashboard/foundation.py` `pile_cap` routine. It was verified to
reproduce that Python engine **exactly** (cap B/L, thickness, flexural moments,
bar count/diameter/spacing, strut-and-tie θ and strut/node ratios) across pile
groups from 1 to 16, with and without moments.

## Deploy to Vercel (keep the file named index.html)
- Drop: vercel.com/new -> drag this folder. Use the SAME project name to update the same URL.
- CLI:  `vercel --prod` in this folder (keeps the same URL).
- Git:  push -> import (Output Directory ".").

## Limitations
Preliminary tool, rigid-cap assumption, single load case (not a multi-combination
envelope). Verify pile structural capacity, group action, negative skin friction,
settlement, edge/corner punching detailing, and grade beams for base moments with
a detailed model before issuing drawings.
