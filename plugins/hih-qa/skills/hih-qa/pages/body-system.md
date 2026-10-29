# Body system page

- Top: a horizontally scrollable list of body systems (heart health, kidney health, BMI, hormones, bone health, …). On scroll it must stay sticky below the header so the user can still switch systems. Flag if it unsticks, overlaps, or looks off.
- Below the tabs: system icon, name, "x/y Parameters are out of range", a deviation pill (e.g. MILD DEVIATION / Fine tune lifestyle), an AI summary with **Read more**, then **How to Improve** and **Consult a Doctor** cards.
- **Show out of range parameters only (N)** toggle, on by default: when on, only out-of-range parameters show, with a **+ N normal parameters** button. Turning it off shows all. Flag if it doesn't switch.
- Watch the skeleton → content swap. As of Oct 2026 the skeleton is a single grey block, giving CLS ≈ 0.76.
- Each parameter: name, graph, a summary of what it means for the user, and chips (e.g. "How can I improve").
- After the whole parameter list: a **Next steps** horizontal card list (recommendations, doctor consultation, …).
