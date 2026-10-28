# Family view

Reached from Profile → **More to explore** → **Your family view**, or the **You | Family** toggle in the header. Route: `/health-record?header_toggle=family`. Header: back arrow, upload icon, **You | Family** toggle with Family active.

Top → bottom:
1. **Family hub** hero, "Track family's health data in one view". A podium of the top 3 members by health score: avatar and name with ›, highest in the middle with a star badge. Name → that member's homepage (`/health-record/<patientId>?header_toggle=you`).
2. **Health overview:** horizontal cards, one per member. Each has an avatar with a score badge, name with ›, and percentile ("Top 1% / in your age group", "Above 20%…", or "Percentile is not available"). **How to improve it?** links to `/health-record/<patientId>/recommendations`. Ends with an **Add member** card ("Create a new member in health insights"). A floating share button sits over this row.
3. **Body system:** one block per member: avatar, name with ›, "x/y body systems are out of range", then horizontal body-system tiles, each with a name, ›, and **Attention Level** bars.
   - Tile → bottom sheet: the system name, "x/y Parameters are out of range", then out-of-range parameters (ideal range, value, date, **Out of Range**, range bar, **What does it mean?** / **How to improve?**). After those: "You're doing good with N parameters", **See all parameters**, and an **Ask your AI Clinician** prompt.
   - Ends with an **N more / Body systems** tile → that member's homepage.
   - A member without reports shows "Upload a report" and placeholder tiles (Heart health, Blood health, Vitamins).
4. **Turn your lab report into AI insights** card with **Upload Report** → `/health-record/upload-external-report/…`.
5. Footer text: "Family: Always Together, Forever".

Avatars show as empty circles while their images are still loading; that's expected. Wait for images to finish loading before judging, and flag an avatar only if it stays empty after load (e.g. the image failed). Check that the floating share button doesn't cover card content.
