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

Load case toggle: **axial only**, or **axial + moment** (Mx, My distributed to
the piles as `P/n ± M·x/Σx²`, rigid-cap). Standard pile groups: 1, 2, 3, 4, 5,
6, 7, 8, 9, 12, 16.

Self-contained single `index.html` — no build step, works offline.

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
