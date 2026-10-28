# Consult a doctor

Reached from Profile → **More to explore** → **Consult a doctor**. The same page opens from the **Consult a doctor** cards in Next steps (lab report detail) and the prescription insights page. Route: `/health-record/<patientId>/tele-consultation`. Header: back arrow only.

Top → bottom:
1. Hero: **Consult with our specialist doctors to improve your health**, subtitle "Get lifestyle, diet & supplement suggestions", then "Consult via chat, audio or video for up to 30 minutes" with chat/call/video icons.
2. Price strip: "Consult now at only **₹999** ~~₹2000~~" (green price, struck-through MRP), with "Introductory price | Valid for a limited time" below.
3. **Our specialist doctors** on a dark band: one card per doctor with photo, name, speciality, years of experience, and a **Consult now** CTA. Consult now leaves HIH for the online-consultation slot booking (`/online-doctor-consultation/slots?…&hih_page_source=hih_online_consult`). Only check that it lands there; don't QA that flow.
4. **How it works:** 4 numbered steps (choose a doctor → slot, method, payment → consult, with two sub-points → detailed prescription with recommendations), joined by a dashed line.
5. **FAQ** accordion (6 questions). Tap one: it must expand to show its answer and collapse again.

Flag any large empty space after the FAQ before the page ends.
