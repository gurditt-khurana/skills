# Health score page

Reached from Clinician insights → tap the health score (score, delta, or the percentile line). Route: `/health-record/<patientId>/health-score`. Header: back arrow, share icon.

Top → bottom:
1. **Health score** card: previous score → current score in dotted rings, with a delta badge on the current one (e.g. "+9 ↑" green). A line such as "Your score increased by 9 points", then "N key parameters impacted this change".
   - **Impact chips:** one per parameter, scrolling sideways, each with a green ↑ or red ↓ badge → `/parameter-details?…`.
   - **See what changed your health score** → chatbot (`/chat?faq=Please explain what changed my health score?`).
   - The score and delta must match the homepage health score widget.
2. **Your score is in top N% of your age group and gender ›** → bottom sheet titled "Your health score among people your age":
   - gender, age and age group
   - the score and percentile
   - a histogram of health score vs number of people, with your score marked
   - "Based on data from your age group: N people"
3. **Filter chips:** **All**, then **Needs Improvement**, **Looks Good** and **Untested Systems**, each with a count badge. Selecting one must reduce the body map labels to that group.
4. **Body map:** a human figure with body-system labels on both sides, joined to the figure by connector lines. Each label has a coloured status dot: amber for needs improvement, green for looks good, grey for untested. Tested systems → body system page (`/insights-summary?body_system=…`). Check the connector lines land on the figure and labels don't overlap or clip at narrow widths.
5. **Health recommendations** card → `/health-record/<patientId>/recommendations`.
6. **Get AI insights on your health reports** card → Upload More page.
7. **More for you**: **What is health score?** → `/health-record/<patientId>/about-health-score` (static explainer: what the score is, how it's computed, the body systems and parameters measured), and **Check your score accuracy** → bottom sheet: "Score accuracy high!" / impacting factors (x/12 body systems tested, y/12 untested with names, "Get tested to improve accuracy").
8. Footer illustration "Smart care for the ones you love".

Floating **Share progress** pill at the bottom (see **Share flow** in `SKILL.md`).

**Cross-check counts:** the counts must agree across the filter chips, the body-map labels, the score-accuracy sheet, and the homepage Body System Overview (out of range / normal / untested). Flag any mismatch.
