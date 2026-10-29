# Health documents (from Profile)

Tap each **Your Health Documents** tile and walk down to the detail pages. If the signed-in patient has no data for a type, switch to a family member who does (patient dropdown on Profile, or **Your family view** → `/health-record?header_toggle=family`). Every listing below ends with "Showing 1 - N of N", **That's all folks!**, **Back to top**, and a sticky upload CTA at the bottom. An empty listing shows a "No … available" message and the upload CTA only.

## Lab reports

- **Listing** `/health-record/<patientId>/test-reports`: header **Test Reports**, a search box for parameter values (e.g. B12, D), then one card per report: name, date, and a status row. The status is either "All parameters are normal" in green, or "<parameter> +N outside normal" in amber with a chevron that expands the out-of-range parameters (value and range). Card → report detail.
- **Detail** `/health-record/<patientId>/test-reports/<reportId>`. Header: **Test Report** and the patient name.
  1. A horizontal card row: **View overview page for all insights**, then the report name with its booked date.
  2. **Report overview:** report score, then **Within range** and **Out of range** parameter counts.
  3. **Parameter overview:** an out-of-range group and a within-range group. Each row shows the name, body system, value and range, and taps through to `/health-record/<patientId>/parameter-details?body_system=…&parameter_id=…`. Each group expands with **Show N more parameters**.
  4. **Did you find report overview helpful?** feedback.
  5. **Next steps** horizontal cards (Consult a doctor, Recommendations, partner programs like Weightwise) with **See All** → `?tab_value=improve`.
- Check units render properly. Flag mojibake such as `Âµg/dL` where `µg/dL` is expected.

## Prescriptions

- **Listing** `/health-record/<patientId>/prescriptions`: header **Prescriptions**, one card per prescription:
  - **Digitised:** doctor name, date, optional degree chip (e.g. "Mbbs"), and an amber first-medicine row with a chevron that expands every medicine with its frequency (sos, one/two/three times a day, once a week/month).
  - **Not digitised:** the title is the status instead of a doctor: **Sent For Digitization** ("Please check in 48 hours…"), **Digitisation Failed**, or **Prescription Invalid**.
- **Detail (prescription insights)** `/health-record/<patientId>/prescription/<rxId>`:
  - **Digitised:** cart icon in the header, **Insights by medically trained AI** pill, **Prescribed by <doctor>** card with a download icon, then a sticky tab row that jumps to each widget present: **Medications**, **Diagnosis**, **Complaints**, **Medical History**, **Precautions**, **Lab Tests**. Medicines matched to a 1mg SKU show the full SKU name and manufacturer, plus **Dosage | About medicine | Impact** and a chevron that expands chatbot FAQ links. Unmatched ones show the plain name. A **Disclaimer** with **Read more** ends the page, and a floating **Ask me about …** pill (rotating topics) sits over it.
  - **Who created it:** only **Medications** + **Disclaimer** = created by us. Any other widget too (Diagnosis, Complaints, Medical History, Precautions, Lab Tests) = the user uploaded the prescription and we digitised it. Say which kind each one you test is.
  - **Pending:** "Sent For Digitisation… Come back in a few days" with **View prescription**.
  - **Failed or invalid:** "Prescription digitisation failed… Please try again or upload a different file" with **View prescription**.
- Check that the medicines in the listing card match the Medications widget on the detail page. Flag any widget that renders a heading with no items (e.g. an empty **Lab tests suggested**).

## Radiology scans

- **Listing** `/health-record/<patientId>/radiology-reports`: header **Radiology reports**, one card per scan: name (e.g. Knee X-Ray), date, "See detailed report for results". Card → detail.
- **Detail** `/health-record/<patientId>/radiology-report/<reportId>`: header **Radiology Scan** and the patient name. Then the scan name, "Done on <date>", a **Download report** pill, and **Inference from the scan**: a report summary (scan type and findings, changes, clinical significance, recommendations, next steps) and a **Key findings** bullet list. Footer illustration at the end.

## Vaccination

- **Tracker** `/health-record/<patientId>/vaccination-tracker`. Header: patient name, gender | age, and an **+ Add Vaccine** button. A summary card counts **Done | Partial | Overdue | No Record**. Below it, vaccine tiles are grouped as **Annual** (renews every year), **Lifelong** (one-time protection), **Age-specific**, **Periodical** (recurring/booster doses), and **Incidental** (need based). Each tile: icon, status chip (Done / Partial / Overdue / No Record), name, a segmented dose bar, "x/y doses". Done tiles also show the source (e.g. "Self-rep.") and a ✓ date. Check that the summary counts match the tiles.
- **Vaccine page** (tile →) `/health-record/<patientId>/book-vaccine/<VACCINE>`. Header **Vaccine Booking**. The vaccine name and about text, **Recommended dose**, and "Consult a doctor now." come first. Then:
  - **With doses:** a **Dose history** list. Each entry has its source (e.g. Self-Reported), **Delete**, the date and the product.
  - **Products:** product cards (name, brand, price, MRP, % off, a **With Administration | Medicine Only** toggle, **Add to Cart**).
  - **Bottom bar:** **Add External Dose** and **Go to cart**.

## Upload More

`/health-record/choose-report-prescription/<patientId>`. **What do you want to upload?** offers **Upload lab report**, **Upload prescription** and **Upload radiology scan**, each with a subtitle. A **Here's how it helps you** section follows: Unified Health Tracking, AI Powered Insights, Actionable next steps.
