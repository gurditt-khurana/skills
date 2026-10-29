# UX audit: animations, transitions, interaction feel

Run this on every flow with motion: bottom sheets opening and closing, steps inside a sheet (e.g. Switch Profile → Merge / View Archived / back), tab switches, toggles, skeleton → content, page-to-page transitions.

## Measure

A single screenshot can't show a broken transition. Measure every frame instead.

- **Frame sampling:** right after the tap, run a `requestAnimationFrame` loop for about 1.5s at real speed. On each frame, log the x, y, opacity and height of each moving element and of its container. Read the log for:
  - content "animating" outside the visible area (e.g. fading in below the sheet's bottom edge)
  - blank gaps: nothing visible with opacity > 0 inside the viewport
  - single-frame flashes: opacity jumping back up for one frame
  - hard cuts: an element appearing at full opacity with no transition
  - diagonal motion: a child moving on an axis its parent isn't (e.g. content sliding sideways while the sheet slides up)
  - animations that never finish
- **CSS:** listen for `transitionstart` / `transitionend` / `transitioncancel` and `animationend` events, and check `document.getAnimations()` for anything still running after the transition should have ended (e.g. a loader running under loaded content).
- **View Transitions:** wrap `document.startViewTransition` and record whether `ready` or `finished` rejects. Flag duplicate `view-transition-name` values: they make the browser skip the transition silently.
- **Don't slow animations with Chrome DevTools** (`Animation.setPlaybackRate` / `seekAnimations`) on HIH pages. framer-motion runs opacity and transform through the browser's animation API, and slowing it leaves screens stuck at opacity 0. That looks like a bug but users never see it. Before reporting anything, reload and reproduce it with no instrumentation attached.
- **Known pattern:** framer-motion `AnimatePresence` with `exitBeforeEnter` and no `mode="wait"`. framer-motion removed that prop in v7 and the repo is on v12, so the old and new screens are mounted at the same time.

## Recordings

Recordings are the most important evidence for UX findings, and every UX finding needs one. Keep them short: a reviewer should see the bug within the first second, with no dead time.

Record in the tab that is already signed in. Copying cookies into a new browser context doesn't carry the 1mg login.

1. **Record one clip per interaction.** Pick whichever method fits your browser tool:
   - **CDP screencast (preferred with a CDP-attached Chrome):**
     - Get the screen to where the interaction starts, then call `Page.startScreencast` (jpeg, quality 75).
     - Wait about 0.25s, tap, and keep collecting `Page.screencastFrame` events for about 1.2s. Ack every frame with `Page.screencastFrameAck`, or the stream stops.
     - Call `Page.stopScreencast`.
     - Chrome only sends a frame when the screen changes, so there is no dead time. It works in a background tab at about 60fps.
     - Build the video: resample the frames to a constant 30fps using each frame's `metadata.timestamp`, repeating the previous frame to fill gaps, so timing stays true. Join the JPEGs into one temp `.mjpeg` file and encode it to MP4 (see the commands below).
   - **Playwright `recordVideo`:** only if you signed in inside the Playwright browser itself. Create the context with `recordVideo`, using the same viewport and user agent as the test. Note `t0` right after `newPage()`, because the video starts then. Log timestamps around each tap, close the context to save the video, then trim (step 2).
   - **Show every tap and swipe.** The video only captures the page, so draw a red circle into the page about 2 frames before each tap. For a scroll, slide the circle along with it. Inject the helper below again after every navigation.
     - The marker is a popover, so it shows above modals and dialogs in the browser's top layer; a plain `z-index` doesn't.
     - Tap only what a user could tap. The helper hit-tests a few points inside the element and clicks whatever is actually under the visible point (the real handler is often on a child, e.g. a modal's close `<img>`).
     - If the element is covered (by a modal, a sticky header, etc.), the helper returns `COVERED by …` and doesn't click. Close the covering element first, as a user would, and record that too. Never JS-click through an overlay.
     - Hidden copies of an element exist (e.g. links in the side drawer), so pick the visible one.
     - Leave `[data-qa-tap]` out of frame sampling and `getAnimations()` checks.

     ```js
     (() => {
       const mark = (x, y) => {
         const d = document.createElement('div')
         d.setAttribute('data-qa-tap', '')
         Object.assign(d.style, { position: 'fixed', inset: 'auto', margin: 0, padding: 0, left: `${x - 22}px`, top: `${y - 22}px`,
           width: '44px', height: '44px', borderRadius: '50%', boxSizing: 'border-box', pointerEvents: 'none', zIndex: 2147483647,
           background: 'rgba(255,59,48,.35)', border: '3px solid rgba(255,59,48,.95)', overflow: 'visible' })
         document.documentElement.appendChild(d)
         if (d.showPopover) { d.popover = 'manual'; d.showPopover() }
         return d
       }
       window.__tapEl = (el) => {
         el.scrollIntoView({ block: 'nearest' })
         const r = el.getBoundingClientRect()
         let first = null
         for (const fy of [0.5, 0.75, 0.9, 0.25, 0.1]) for (const fx of [0.5, 0.3, 0.7]) {
           const x = r.left + r.width * fx, y = r.top + r.height * fy, hit = document.elementFromPoint(x, y)
           first ||= hit
           if (hit && (hit === el || el.contains(hit))) {
             const d = mark(x, y)
             d.animate([{ transform: 'scale(.6)' }, { transform: 'scale(1)', offset: 0.35 }, { transform: 'scale(1.3)', opacity: 0 }],
               { duration: 700, easing: 'ease-out' }).onfinish = () => d.remove()
             requestAnimationFrame(() => requestAnimationFrame(() => hit.click()))
             return 'tapped'
           }
         }
         return `COVERED by ${first ? `${first.tagName}.${first.className}` : 'nothing'}`
       }
       window.__swipe = (x, fromY, toY, ms = 600) => {
         const d = mark(x, fromY)
         d.animate([{ transform: 'none' }, { transform: `translateY(${toY - fromY}px)`, offset: 0.85 },
           { transform: `translateY(${toY - fromY}px)`, opacity: 0 }], { duration: ms + 150, easing: 'ease-out' }).onfinish = () => d.remove()
         window.scrollBy({ top: fromY - toY, behavior: 'smooth' })
       }
     })()
     ```
2. **Trim** the clip to start − 0.3s through end + 0.7s:
   - Aim for 2–6s and never more than 10s.
   - No page loads, sign-in, waiting or idle frames.
   - If a flow has a long wait in the middle that isn't the bug, split it into separate clips rather than leaving the wait in.
3. **Slow-motion copy:** for glitches shorter than about 0.5s, also save a 4× slow copy named `<clip>-4x-slow.mp4`.
4. **Check the clip:** pull 2–3 frames from the trimmed clip as PNG and look at them, to confirm it actually shows the bug.
5. **Clean up:** delete raw videos and frames. Keep every final clip; never delete them once the run ends. Save clips in `recordings/` inside the audit folder, named after the finding (e.g. `merge-blank-sheet.mp4`).

Save every clip as **H.264 MP4**, which plays in QuickTime, Chrome, Confluence, Slack and WhatsApp. Never save WebM.

Use the standalone `ffmpeg-static` build from npm, installed in a cache folder so nothing changes system-wide. Playwright's bundled ffmpeg can only write WebM, so don't use it.

```sh
FFDIR=~/.cache/hih-qa-ffmpeg
[ -x $FFDIR/node_modules/ffmpeg-static/ffmpeg ] || (mkdir -p $FFDIR && cd $FFDIR && npm init -y >/dev/null && npm i ffmpeg-static --silent)
FF=$FFDIR/node_modules/ffmpeg-static/ffmpeg
MP4="-c:v libx264 -pix_fmt yuv420p -crf 23 -movflags +faststart"

"$FF" -y -f image2pipe -c:v mjpeg -framerate 30 -i frames.mjpeg $MP4 clip.mp4      # screencast frames -> MP4
"$FF" -y -ss <start> -to <end> -i raw.webm $MP4 clip.mp4                           # trim (e.g. a Playwright recording)
"$FF" -y -i clip.mp4 -vf "setpts=4*PTS" $MP4 clip-4x-slow.mp4                      # 4x slow motion
"$FF" -y -ss 0.5 -i clip.mp4 -frames:v 1 check.png                                 # frame to inspect
```

## `ux-audit.md`

Write UX findings in `ux-audit.md`, a separate file from `report.md`, which is for functional and layout bugs. For each finding include:

- **Title and severity:** Blocker / High / Medium / Low.
- **Where:** page → action, e.g. Homepage → Switch Profile → Merge.
- **What the user sees,** in one or two plain sentences.
- **Recording:** link to the clip, plus the slow-motion copy if there is one.
- **Frame timeline:** a short table from the sampling log, giving time and what is on screen.
- **Likely cause,** with `file:line` if you found it in the code.
- **Expected behaviour.**

End the file with a **Checked, no issue** list and any method notes.
