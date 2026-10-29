# Homepage

Header: back (or home) button, default patient's name with **Switch Profile** ▾, upload icon, **You | Family** toggle. Then tab selector: **Latest Report Overview** | **Clinician Insights (Powered by AI)**. Floating: bottom nav (**Insights | Improve | Ask me | Trends | Profile**) and a **1voice by 1mg** pill.

Check that the patient name stays on one line with ellipsis and doesn't spill out of the header at 320–390px.

## Latest report overview (top → bottom)
1. Name of the most recent report, "N parameters tested • Booked on <date>".
2. **Download report** CTA, with **Report highlights** (→ `/report-highlights`) beside it if eligible.
3. **All Parameters** label with a **Key parameters only** toggle. Toggle it both ways; it must switch (e.g. 5/100 ↔ 2/28) and nothing should break.
4. **OUT OF RANGE** / **WITHIN RANGE** count cards.
5. Out-of-range parameter list ("Summarise my out of range parameters" → chatbot). Each row: name, range, body system, previous → latest value, › to `/parameter-details`. **Show N more parameters**, then "Improve your out of range parameters with improvement plan" → `/recommendations`.
6. Within-range parameter list, built the same way.
7. **Merge duplicate profiles** nudge (dismissible; shown only if eligible).
8. **Did a family member book a lab test for you?** card → `/records-import/request`.
9. **Improvement Plan** card: key parameters that need improvement, then Consultation / Diet / Supplements / Lifestyle, then **View my improvement plan**.
10. **Talk to your latest report:** chatbot question rows plus **Ask a different question** → chatbot page.

## Clinician insights (top → bottom)
1. "Your activity data is now linked" strip with **View** (only if wearable data is linked).
2. **HEALTH OVERVIEW:** "Your overall health" with a rotating status, then **Your health score**. Only two variants:
   - Lock icon, no number (no valid score).
   - Number + trend: stable, or ↑ green + delta (increased vs last report), or ↓ red + delta (decreased). Flag any other combo.
   Then the "Your score is in top N% …" pill. All three → `/health-score`.
3. **AI REPORT SUMMARY:** "<name>'s personalised report" with **Read More** → bottom sheet (PERSONALISED REPORT text) with **Consult a doctor** and **Listen** CTAs.
4. **Body System Overview:** groups for **OUT OF RANGE**, **NORMAL** and **UNTESTED**, each with a count and **Show More**. Untested tiles → `/labs/search/all?q=…`.
   - Tile → bottom sheet: system name, "x/y Parameters are out of range", and → (→ `/insights-summary?body_system=…`). Then the **AI Clinician Summary** (Read more), out-of-range parameter cards (ideal range, value, date, range bar, **What does it mean?** / **How to improve?**, › → parameter page), and an **Ask your AI Clinician** prompt.
   - Counts must match the Health score page chips and the score-accuracy sheet.
5. **Improvement Plan** widget (systems that need improvement) → recommendations page.
6. **Next steps:** Consult a doctor, Check health recommendations.
7. "Your activity data for Today" (Steps / Calories / HRV / Resting HR), only if linked.
8. **Get AI insights on your health reports** → Upload More page.
9. Footer illustration "Smart care for the ones you love".
