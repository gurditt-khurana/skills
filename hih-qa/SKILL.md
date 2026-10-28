---
name: hih-qa
description: Use when asked to test, QA, or verify HIH (Health Records) flows on mweb in a browser. Covers opening the health-record page on a given environment, signing in with the test account, checking the homepage tabs, health score page, body system page, profile page, and health-document listings and detail pages (lab reports, prescriptions, radiology, vaccination), consult a doctor, parameter trends, and family view.
---

# HIH QA

Drive the browser with whatever browser-use or computer-use tool you have (e.g. the Playwright MCP in Claude Code). URL: prod `https://1mg.com/health-record`, else `https://<env>.1mg.com/health-record` (e.g. stag, earth).

## Viewport (mobile only)

Test HIH only in mobile view: mobile user agent, touch enabled, and mobile request headers. Never test it in desktop layout.

Repeat key pages across the combinations below and flag any UI break, e.g. overflow, horizontal scroll, clipped or overlapping text, wrapped CTAs, or a sticky element covering content.
- **Width:** only these common portrait CSS widths:
  - 320px: compact and older devices (iPhone SE 1st gen, small legacy Android).
  - 360px: most widespread budget and mid-range Android (Galaxy S24, Infinix).
  - 375px: modern iPhone baseline (iPhone SE 3rd gen, iPhone 15/16).
  - 390px, 393px: modern flagships and upper-midrange (iPhone 15/16 Pro, Pixel).
  - 412px, 414px: larger Android flagships and phablets (Pixel Pro, Galaxy Ultra, iPhone Plus/Max).
- **Height:** 640, 844px.
- **Page zoom:** 100% only.

Never test other widths or zoom levels, and never resize on purpose to make the UI break.

## Sign in (skip if already signed in)

Static sign-in page → tap bottom button → bottom sheet with number input.
- **Non-prod:** number `9140942009` (unless the user gives another) → **Proceed** → OTP `123456` → tap bottom button (Proceed/Login).
- **Prod:** test creds don't work. Ask user for number → **Send OTP** → get OTP: if Gmail is signed in on this browser, open the latest "OTP for Tata 1mg" mail in a new tab; else ask the user → enter OTP → tap bottom button.

## Pages

Each page has its own description file under `pages/`. Read only the files for the pages you are testing. A file describes the page top → bottom, its route, where each control leads, and page-specific checks.

| Page | Route | File |
| --- | --- | --- |
| Homepage: Latest report overview, Clinician insights | `/health-record` | `pages/homepage.md` |
| Body system page | `/health-record/<patientId>/insights-summary?body_system=…` | `pages/body-system.md` |
| Health score page, What is health score? | `/health-record/<patientId>/health-score` | `pages/health-score.md` |
| Profile | `/health-record?tab_value=records` | `pages/profile.md` |
| Health documents: lab reports, prescriptions, radiology, vaccination, Upload More | `/health-record/<patientId>/test-reports`, `/prescriptions`, `/radiology-reports`, `/vaccination-tracker` | `pages/health-documents.md` |
| Consult a doctor | `/health-record/<patientId>/tele-consultation` | `pages/consult-a-doctor.md` |
| Parameter trends | `/health-record/<patientId>/parameter-view` | `pages/parameter-trends.md` |
| Family view | `/health-record?header_toggle=family` | `pages/family-view.md` |

When you find a new page or a page has changed, update its file (or add a new one and a row here).

## Share flow

Test every share entry point you come across, e.g. the share icon in the Parameter trends header, the floating share button on Family view, and the header share icon and **Share progress** pill on the Health score page.

1. Tap share and follow the flow until it produces a link. Note what the UI shows on the way: options, loader, the "link copied" toast or share sheet. Flag anything broken.
2. Capture the link from the clipboard, the share sheet, or the network response. Flag it if no link is produced.
3. Open the link in a fresh, signed-out context of the same browser: an incognito window, or a new browser context with no cookies or storage (e.g. Playwright `browser.newContext()`, CDP `Target.createBrowserContext`). Use the same mobile viewport and user agent.
4. Check that the shared page loads without asking to sign in. It should show the same data as the source (patient name, parameters, values, dates) and render without errors or broken layout. Also flag a 404, an error page, an endless loader, or a redirect to sign-in.
5. Close the incognito tab or context when done.

On prod the link exposes real health data. Ask the user before generating one.

## Evidence

- Save everything from a run (screenshots, scripts, logs) in one folder on the user's desktop: `~/Desktop/hih-qa-audits/<pages-tested>_<YYYY-MM-DD>/` (create it if missing). Never save audit files inside the repo. If that folder already exists, add a suffix: `-2`, `-3`, and so on.
- Take a screenshot only when something is breaking; inspect everything else via DOM snapshots, not screenshots. Many screenshots eat disk space.
- Save screenshots as JPEG at 80% quality (e.g. `type: 'jpeg', quality: 80`), never full-quality PNG.
- Write a `report.md` in the same folder. Put each screenshot in it with a caption directly below the image saying what's breaking, e.g. `![](home-320.jpg)` followed by *"320px: Download report CTA wraps to 2 lines and overlaps Report highlights."*

## Teardown (when done)

Leave nothing that would block another agent's sanity run: close every tab and the browser session, and stop any dev server or background process you started.

## Always check (every page)

- **Missing widgets:** if any widget described in the page file is absent, report it to the user.
- **Skeleton → content CLS:** if the layout shifts noticeably when the loading skeleton is swapped for real data, flag it and suggest a better-matched skeleton.
- **Visual polish:** look for pixel-level issues and report them, e.g. a corner radius that isn't applied, a gradient that's off, or nested elements with the same border radius where the parent's padding leaves visible gaps at the corners (the inner radius should be the outer radius minus the padding).
