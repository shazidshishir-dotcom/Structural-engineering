# Bangladesh Wind & EQ Map (BNBC-2020)

BNBC-2020 site-load lookup with a Wind / Seismic toggle:
- WIND: basic wind speed V (km/h and m/s), 542 thana values.
- SEISMIC: seismic zone (1-4) and zone coefficient Z.

Click/tap a point, enter lat/long, or search a thana/district (dropdown
sits under the box). All 64 districts covered. Boundaries OCHA/BBS ADM2;
Noakhali & Pabna split to upazila so Hatiya (260) / Ishwardi (225) read
exactly on Wind. Responsive (laptop + mobile, safe-area aware). Self-
contained single file; works online (basemap) and offline.

## Deploy to Vercel (keep the file named index.html)
- Drop: vercel.com/new -> drag this folder. Use the SAME project name to update the same URL.
- CLI:  `vercel --prod` in this folder (keeps the same URL).
- Git:  push -> import (Output Directory ".").

## Note on mobile testing
In-app browsers (Messenger/Facebook) cache aggressively and can show a
stale version. After deploying, open in Safari/Chrome directly, or hard-
refresh, to see the latest.
