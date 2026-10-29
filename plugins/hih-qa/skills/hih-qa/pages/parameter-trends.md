# Parameter trends

Reached from Profile → **More to explore** → **Parameter trends**. Route: `/health-record/<patientId>/parameter-view`. Header: back arrow, upload and share icons, title **Parameter data for <name>** (truncates with …).

Top → bottom:
1. **Search your tested parameters** input.
2. Chip row that scrolls sideways:
   - **Filter icon:** opens the **All filters** sheet. **Body system** tab (one option per system the patient has); **Status** tab (Within range / Out of range); **Clear all** / **Apply**.
   - **Sort by:** opens a sheet with Alphabetically - Body systems / Alphabetically - Parameter name / Recently tested.
   - **Bookmarked**, **Key parameters**, any custom lists the user made (by name), **Create custom list**.
   - The active chip is dark-filled and names the section below.
3. Section title (e.g. **Key parameters**) with a **Table | Graph** toggle. Switch both ways; the list must re-render without breaking.
4. One card per parameter. Card header: name with › (→ `/parameter-details?body_system=…&parameter_id=…`), bookmark icon, body-system chip, and "Range: …" on the right.
   - **Graph:** y-axis with colour-banded range bands (green in range, amber/red outside), one point per test date on the x-axis, latest value in a black pill tagged **LATEST**.
   - **Table:** one row per test, with value and date, newest first.
   - **Differing ranges:** if the range changed between reports, the header range is dropped. The card then shows the range next to each value instead, even in Graph view. That's expected, not a bug.
5. Floating **Ask me about …** pill at the bottom, with rotating topics.

Check that graph labels (axis values, dates) don't overlap or clip at narrow widths. Check that units render properly (flag `Âµ` mojibake).
