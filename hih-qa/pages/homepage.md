# Homepage

Top: default patient's name, then tab selector: **Latest report overview** | **Clinician insights**.

## Latest report overview (top → bottom)
1. Name of the most recent report.
2. **Download report** CTA, with **Report highlights** button before it if eligible.
3. Changed parameters. Toggle **Key parameters** ↔ **All parameters** both ways; must switch, nothing breaks. Grouped by category: worsened on top, became-normal at bottom.
4. AI chatbot Q&A tiles (questions) → tap → chatbot page.

## Clinician insights (top → bottom)
1. **Health score**, only two variants:
   - Lock icon, no number (no valid score).
   - Number + trend: stable, or ↑ green + delta (increased vs last report), or ↓ red + delta (decreased). Flag any other combo.
2. **Report explanation:** AI summary of the latest report and user metadata → tap → bottom sheet with **Listen** and **Consult a doctor** CTAs.
3. **Changed body systems** tiles → tap → bottom sheet:
   - Top: how many of the system's parameters are in range → tap → body system page `/health-record/<patientId>/insights-summary?body_system=<name>`.
   - Below: the system's parameters that changed in the latest report → tap one → parameter page.
4. **Improvement plan** widget → tap → recommendations page.
