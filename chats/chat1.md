# Hoot: University-owned teaching layer

_Started 2026-07-09 06:45 UTC_

---

## User

Build a single-page marketing website for Hoot — the same radically minimal, editorial one-pager we already have, but with a little more functionality. The restraint stays exactly as-is: this should still feel like a well-set page in a book, mostly type and whitespace, almost no chrome. We are adding a few quiet interactive elements inspired by vertus.ai (minimal sticky nav, a product switcher, an us-vs-them comparison, disclosure/FAQ sections, and a real pilot-request form with success/error states) — but every addition must obey the same rules of restraint. When an addition fights the calm, cut it.

What Hoot is (content source of truth — current build, July 2026) Hoot is the university-owned AI teaching layer for undergraduate courses — delivered institutionally, grounded only in the professor's released course materials, citation-locked, and owned by the institution. It is not a generic student chatbot, not a personal notebook, not shadow AI outside school governance. The system is two student-facing halves plus the machinery behind them:

Hoot — the answering half. A student's question becomes a cited answer built only from released course materials. End to end: authenticate and check course membership (hard gate, no auto-enrollment) → transcribe any attached image → prepend rolling chat memory → route how much retrieval is needed → search by meaning (vector) and exact words (full-text) together, merged by reciprocal-rank fusion → bias by source type (textbook above slides/notes, teacher-tunable) → pack top passages to ~85% of the token budget, each stamped with a citation marker → drop anything below the relevance floor → write a source-bounded answer (if nothing relevant survives, it says so instead of guessing) → enforce citations after the fact: invented markers deleted, uncited paragraphs given a valid one. It can't point to a page it never read. Only released materials are searchable; weekly slides/notes unlock as the course reaches that week, textbooks stay visible throughout.
Apollo — the teaching half. A teach-back partner: the student explains a concept back, and what they say is recorded as a knowledge graph of typed nodes (equations, conditions, definitions, variable mappings, steps) wired by dependency. Apollo structurally can't leak the solution — it grades what the student explains against the teacher's own worked solution; it doesn't hand it over. Grading is structural, not one LLM score: coverage, soundness, structural similarity, plus a misconception safety layer so fluent-but-wrong explanations don't pass.
Behind them: problem provisioning (teacher PDFs → validated, tiered practice bank; suspect pairs held for review), reference graphs (canonical solution graph per teach-back), and a learner model — every graded answer updates a persistent, interpretable belief vector per student × concept (confused / shaky / mastered) that aggregates into a classroom map for teachers: class-wide strengths, weak concepts, common misconceptions, students who are stuck.
Positioning / business / team (unchanged facts) Problem: AI in academia is a governance and fairness problem — usage today is private, uneven, unmonitored, outside institutional control. Differentiation: instructor upload/control authority · dedicated student + instructor interfaces · institution-level governance · protection of instructional IP · equal footing for every student. Buyer: universities/administration. Operators: instructors + TAs. Users: undergraduates. Business model: pilot 1–3 courses at low/no cost against pre-agreed success metrics (e.g. TA hours deflected), then expand to a departmental subscription once met. Roadmap 2026: Jan–Jun Purdue pilot · Jun–Aug efficacy study at TMU with control groups · Aug–Dec departmental expansion at Purdue. Founders: Ishaan Batra and Alfonso Macarrein — Purdue undergraduate aerospace students. $24,000 research grant. (Do not invent titles.) Guardrails: always call it "Hoot." Never describe it as student-owned, cloud-SaaS-first, or dependent on proprietary third-party models in its intended production form. Institution-hosted, open models, private storage is the target architecture.

Visual system (match the existing Hoot brand exactly — do not change it)

--bg:      #ece3d2   (warm ivory)
--bg-soft: #e4d9c3
--bg-panel:#fbf6ea
--text:    #1a1612   (near-black ink)
--muted:   #5a4f40
--border:  #c9bba1
--accent:  #000000   (ink)
--serif:   Georgia, "Times New Roman", serif   /* body + display */
--mono:    "JetBrains Mono", ui-monospace, Menlo, monospace  /* labels/eyebrows only */
Georgia for headings and body — large and confident for display, calm for reading. JetBrains Mono only for small uppercase eyebrow labels (letter-spacing ~.14em), nav/tab labels, and the occasional metric. Flat and square: border-radius: 0 (2px max). No drop shadows (at most one barely-there hairline). 1px --border hairlines to divide, not boxes to contain. No gradients, no texture, no diagonal stripes, no emoji, no illustrations, no SVG diagrams, no device mockups, no decorated cards. Black accent used sparingly: the one solid CTA, eyebrow labels, at most one thin left-rule. One column, centered, generous margins, reading measure ~60–70ch, big vertical gaps between sections.

Rules of restraint (unchanged — these still govern everything) Motion is functional only: accordion expand/collapse, a sub-200ms tab swap, smooth anchor scrolling, and form state changes. No tickers, no auto-advance, no scroll-triggered reveals, no pulsing dots, no ambient motion, no interactive diagrams. Keep a prefers-reduced-motion guard that disables even these transitions. Ruthless content edit — someone reads the page top to bottom in about a minute; each section is a short heading + one or two sentences + maybe a tight list. When in doubt, remove a word, remove an element, add whitespace.

Functionality to add (inspired by vertus.ai, restrained to fit)

Minimal sticky top bar — wordmark "Hoot" left; 3–4 mono anchor links (What it is · How it works · Institutions · Pilot) center/right; one text CTA "Request a pilot" right. Hairline bottom border, ivory background, no shadow. Collapses to just wordmark + CTA on mobile. Anchor links smooth-scroll to sections.
Product switcher (Hoot ⇄ Apollo) — a small mono segmented control (two flat, square, hairline-bordered buttons; active = ink fill, ivory text) that swaps one shared content block between the answering half and the teaching half. Sub-200ms crossfade, no layout jump. This replaces the two separate stacked product sections with one switchable one.
Comparison block ("Shadow AI today / Hoot") — a two-column, hairline-divided text comparison echoing vertus's language-model-vs-cognitive-system framing: left column the ungoverned status quo (private, uneven, unmonitored, outside institutional control), right column Hoot (course-scoped, cited, membership-gated, institution-owned). Plain type, no icons, no decoration; stacks to one column on mobile.
Disclosure / FAQ — 4–6 hairline-divided questions that expand on click (a rotating "+" glyph as the only indicator, never an icon or emoji). Content: "Is student data private?", "Can it answer outside the course materials?", "What does an instructor control?", "How does a pilot start?", "Which models does it run on?". One open at a time is fine.
Pilot-request form — name, institution, work email, number of courses. Flat square inputs with hairline borders and mono labels; one solid ink submit button. On submit (no backend — simulate client-side): validate email, show an inline success state ("Thank you — our team will reach out to schedule an onboarding call.") and a distinct error state ("Something went wrong — email hello@hoot.study directly."), mirroring vertus's success/error copy. Keep it inline in the page, not a modal.
Dual CTA pattern — in the hero, "Request a pilot" (solid, scrolls to the form) sits beside a quiet "Keep reading ↓" text link, echoing vertus's "Join now or continue exploring."
"Back to top" link — a single mono text link in the footer. No floating button.
Sections, in order Sticky nav · Hero (wordmark, one-line positioning "The institutional AI teaching layer for undergraduate courses.", one support sentence, dual CTA) · What it is / isn't (short paragraph + the three "not this" lines) · Shadow AI today vs Hoot (the comparison block) · How it works (the Hoot ⇄ Apollo product switcher; the Hoot pipeline may appear as one quiet inline line, Authenticate → Retrieve → Rank → Ground → Cite, not a diagram) · For institutions (learner model → classroom map, plus pilot→measure→expand) · Roadmap (three dated lines, hairline-divided) · FAQ (the disclosures) · Request a pilot (the form) · Team (two founders, one line, the grant) · Footer / contact (one line, mailto, back-to-top).

Technical requirements One HTML file. Inline CSS in a <style> block. Vanilla JS only, inline in a <script> block — no libraries, no build step — for the nav scroll, switcher, accordion, and form. Semantic tags (<header> <nav> <main> <section> <form> <footer>), close every element, double-quote every attribute. Define a and a:hover colors explicitly from the palette. Include the prefers-reduced-motion guard. Responsive by default — the single centered column narrows on mobile; nav and comparison collapse as noted; nothing else changes.

Tone Confident, plain, institutional-but-warm. Sentences a dean would trust and a sophomore would understand. Every interactive element should feel like it was always part of a calm printed page — never like a product demo.

## Assistant

This is a fully-specified brief with the exact brand, sections, copy source, and interactions — no ambiguity to resolve. I'll build it directly.

Let me lay out the plan and build the single Design Component.

_[tool: update_todos]_

_[tool: dc_write]_

_[tool: show_html]_

I want to confirm interactions and layout before handing off. Let me capture a couple of states.

_[tool: show_html]_

Hero reads clean and editorial. Let me check the switcher and FAQ states.

_[tool: multi_screenshot]_

The captures stayed at the hero — `scrollIntoView` isn't reliable here. Let me use offset-based scrolling.

_[tool: multi_screenshot]_

Switcher works cleanly — active ink fill, content crossfades, pipeline line shows on Hoot. Now the FAQ and form states.

_[tool: multi_screenshot]_

FAQ toggle works (+→×). Let me confirm the form success and error messages actually render, since the inputs cleared on submit.

_[tool: eval_js]_

Async re-render — my read fired before the flush. Let me re-test with a wait.

_[tool: eval_js]_

_[tool: eval_js]_

The `type="email"` input triggers native browser validation, which blocks submit before my JS runs — so the custom error state never fires. Let me check whether inputs also lose their values on re-render.

_[tool: eval_js]_

Values persist fine. The only real bug is native email validation blocking the custom error state. I'll add `novalidate` to the form and switch the email input to `type="text"` (with `inputmode="email"`) so my regex validation drives the success/error states.

_[tool: dc_html_str_replace]_

_[tool: dc_html_str_replace]_

_[tool: eval_js]_

_[tool: eval_js]_

The preview is still running the old version. Let me reload it and re-test.

_[tool: show_html]_

_[tool: eval_js]_

All form states work now: `novalidate` on, error shows on invalid email, success on valid, error clears on success. Everything's wired.

_[tool: update_todos]_

Built `Hoot.dc.html` — the radically minimal one-pager with the new functional additions, all obeying the restraint rules.

_[tool: ready_for_verification]_

What I built, all in the existing ivory/Georgia/JetBrains-Mono system with flat hairlines and no chrome:

- **Sticky nav** — wordmark + four mono anchors + a text "Request a pilot"; anchors collapse to just wordmark + CTA under 680px, smooth-scrolling to sections.
- **Hero** with the dual CTA (solid "Request a pilot" beside a quiet "Keep reading ↓").
- **Product switcher** — a mono segmented control that crossfades one shared block between Hoot (with the inline `Authenticate → Retrieve → Rank → Ground → Cite` line) and Apollo.
- **Shadow AI vs Hoot** comparison, hairline-divided, stacking on mobile.
- **FAQ** — five disclosures, rotating "+", one open at a time, grid-row height animation.
- **Pilot form** — flat inputs, client-side validation with distinct success and error states.
- Roadmap, team, and a footer with mailto + back-to-top.

Motion is functional only, with a `prefers-reduced-motion` guard. One note: the form is front-end only — submissions aren't sent anywhere, so you'll want to wire it to a real endpoint before launch.

## User

Attached is the hoot logo animation we use. Create claude like animations based off this owl that give the site a sense of continuity and creativity.

## Assistant

_[tool: image_metadata]_

Let me extract some frames from the video to see the owl animation.

_[tool: run_script]_

_[tool: view_image]_

_[tool: view_image]_

_[tool: view_image]_

The mark is a clean monoline owl, and the animation is a lovely "thinking" head-tilt that settles back upright — perfect signature motion. Let me build a small, restrained system around it: the owl greets in the hero, anchors the nav, and *thinks* when you submit the pilot form.

First, assets — a crisp ink-on-transparent owl mark, plus copying the animation.

_[tool: run_script]_

_[tool: copy_files]_

_[tool: view_image]_

Clean ink owl. Now let me verify the live video recolor (invert + multiply → ink lines on ivory) before wiring it in.

_[tool: write_file]_

_[tool: show_html]_

Video needs a moment to paint its first frame. Let me wait and recapture.

_[tool: save_screenshot]_

_[tool: save_screenshot]_

The recolor works and the video even blinks — great continuity. (Faint box tint; I'll add `contrast()` to clean it.) Now wiring the owl into the nav, hero, and form.

_[tool: dc_html_str_replace]_

_[tool: dc_html_str_replace]_

_[tool: dc_html_str_replace]_

Now the logic — refs, thinking state machine, and hero autoplay-once.

_[tool: dc_js_str_replace]_

_[tool: dc_js_str_replace]_

_[tool: show_html]_

Hero owl plays the curious tilt on load (caught mid-animation here) and the nav mark reads cleanly. Let me verify it settles upright and test the form "thinking" flow.

_[tool: multi_screenshot]_

Empty inputs in the capture are an html-to-image artifact (it doesn't clone live values). The hero owl settles upright cleanly and the box tint is gone. Let me confirm the thinking→success timing with a direct probe.

_[tool: eval_js]_

Everything works: owl thinks on submit, success appears after the beat, error path too. Let me clean up temp files and hand off.

_[tool: delete_file]_

I added a small, restrained owl system drawn straight from your logo animation, so the mark carries through the page without breaking the calm.

_[tool: ready_for_verification]_

Three quiet owl moments, all pulled from your actual animation and recolored live to ink-on-ivory so they match the brand exactly:

- **Nav** — a small static owl mark beside the wordmark, anchoring identity.
- **Hero** — the owl plays its signature curious head-tilt once on load, then settles upright and still. Not a loop, not ambient.
- **Pilot form** — the owl "thinks" (the same tilt) for a beat when you submit, *then* the success or error message appears — so the moment the site is working, the owl is visibly thinking about your request.

All three respect `prefers-reduced-motion` (static upright owl, instant form result, no playback). The recolor is done with a CSS filter over your `owl-think.mp4`, so there's a single source of truth for the animation and no re-encoding. Everything else on the page is untouched.

One note: the owl videos load the full `mp4` (~1080p); if page weight matters for launch, I can downscale the asset — just say the word.

## User

Nicely done, but I was thinking way more. I was thinking a real claude like large animation that weaves throughout the entire website, forms in that same anthropic stop motion style almost. Right now, the animation just sits awkwardly on top of everything. There is no sense of continuity or creativity here.

## Assistant

You're right — it sits on top instead of running *through* the page. Let me rebuild this as one continuous hand-drawn line in the Anthropic stop-motion style: it forms the owl at the top, then threads down the entire page as a living, gently-boiling ink line that draws itself as you scroll.

First I need the owl as a real vector line (traced from your logo) so it can draw itself and connect to the thread.

_[tool: run_script]_

Tracing fragmented. Let me diagnose by drawing the raw marching-squares segments directly.

_[tool: run_script]_

_[tool: view_image]_

Geometry is perfect — my simplify step was the bug. Let me re-chain and decimate cleanly.

_[tool: run_script]_

_[tool: view_image]_

That's a clean, drawable owl. Let me export it as point arrays (so I can generate the stop-motion "boil" variants at runtime).

_[tool: run_script]_

The traced owl is clean and drawable. Now the core: a standalone controller that draws the owl at the top and threads a living, boiling ink line down the whole page. Let me write it.

_[tool: write_file]_

Now wire it into the DC. Let me read the current file to get exact strings.

_[tool: read_file]_

I see the headline is now normal-weight (a direct edit) — I'll preserve that. Now let me wire everything: load the scripts, remove the two "on top" videos, and connect the controller.

_[tool: str_replace_edit]_

_[tool: dc_html_str_replace]_

_[tool: dc_html_str_replace]_

_[tool: dc_html_str_replace]_

Now the logic — instantiate the thread controller, build the mini owl, update lifecycle.

_[tool: dc_js_str_replace]_

_[tool: dc_js_str_replace]_

_[tool: dc_js_str_replace]_

_[tool: show_html]_

The owl draws over ~1.7s and the thread draws on scroll, so at t=0 it's blank. Let me probe the layer and capture after it draws.

_[tool: eval_js]_

Controller is live (33 paths, full-height SVG behind content). Let me let the owl draw and capture top + a scrolled state.

_[tool: save_screenshot]_

Let me diagnose why it's not painting — check draw progress and stacking.

_[tool: eval_js]_

The owl draws correctly but sits behind the content wrapper's background. Let me fix the stacking: insert the layer inside the content wrapper and lift the text above it.

_[tool: str_replace_edit]_

_[tool: show_html]_

_[tool: save_screenshot]_

That's the concept — the owl draws itself in the gutter and the line threads down the page. Let me verify the thread through the full scroll and the form owl.

_[tool: multi_screenshot]_

The thread runs the full page and draws with scroll. At this preview width the gutter is tight so it reads a bit straight — let me give it more organic meander and a stronger boil, then check the form.

_[tool: str_replace_edit]_

_[tool: show_html]_

_[tool: save_screenshot]_

Capture stayed at top (save_screenshot renders from document top). Let me use multi_screenshot and confirm the mini owl.

_[tool: multi_screenshot]_

Scroll captures are being reset by the harness (the tall SVG layer changes how it renders). Let me verify the form owl and flow functionally instead.

_[tool: eval_js]_

Form owl renders and re-draws on submit, then success lands. Everything works. Let me check console and clean up unused assets.

_[tool: get_webview_logs]_

_[tool: grep]_

No console errors and the old video/poster assets are unused. Cleaning up.

_[tool: delete_file]_

I rebuilt the owl as one continuous drawn line that runs through the whole page instead of sitting on top of it.

_[tool: ready_for_verification]_

Reworked into a single hand-drawn line, Anthropic stop-motion style:

- **It forms the owl at the top** — I traced your actual logo to a real vector, and the owl *draws itself* in the left margin on load, stroke by stroke.
- **Then it threads down the entire page** — the same line flows out of the owl as a living ink thread through the left gutter, drawing itself further as you scroll, all the way to the footer.
- **It boils** — the whole line gently jitters frame-to-frame (three subtly-different variants cycling ~8fps), so it reads as hand-inked stop-motion rather than a static rule.
- **The form owl is now the same drawn owl** — a small vector version that re-draws itself while it "thinks" on submit, then success/error lands.

It lives entirely in the margins and behind the type, so it weaves through the page without touching the reading column or breaking the calm. Fully `prefers-reduced-motion` guarded (owl and thread render fully-drawn and static, no boil, no scroll-draw), and it hides below 820px so mobile stays clean. The old overlay videos are gone.

One thing worth a look: on a narrow desktop window the gutter is tight so the thread hugs the left edge; on wider screens it has room to meander and the owl grows and centers. If you'd like it bolder — thicker line, more wander, or a moment where it drifts toward the text at a section break — I can push it further.

## User

Good start, but needs more improvement. Right now, what's drawn is the outline of the lines themselves that should be drawn, making the drawn lines skinny and ugly. Second, owls can fly. The logo should fly across the page in a playful manner, flapping its wings. The eyes should not be circles, but rather playful blinks (in the shape of a left parantheses symbol turned 90 degrees clockwise). The owl's has to be this cute playful figure that flies throughout the page and offers a real sense of continuity and creativity. Right now, it just sits awkwardly as a thin scribble on the left side of the page.

## Assistant

Got it — three real changes: the owl needs proper solid strokes (not hairline outlines-of-outlines), it should *fly* with flapping wings, and the eyes should be playful blink-arcs. Let me rebuild it as a rigged, cute, flying owl.

_[tool: snip]_

_[tool: snip]_

_[tool: write_file]_

Now remove the unused owl-paths.js include and delete it.

_[tool: dc_html_str_replace]_

_[tool: delete_file]_

_[tool: show_html]_

The flying owl is gated to ≥940px and this preview is 924, so it's hidden. Let me lower the threshold, size it to fit the gutter, and clamp it on-screen.

_[tool: str_replace_edit]_

_[tool: grep]_

_[tool: show_html]_

Nav owl was removed by your direct edit — I'll leave it as you set it. The flying owl loads with a fly-in, so it's mid-entrance at capture. Let me probe it.

_[tool: eval_js]_

It's there (12 paths, visible at top-left). Let me capture it now that the fly-in finished.

_[tool: save_screenshot]_

Solid strokes now, arc eyes, ear tufts, wings — much better. Let me zoom in to judge the character and confirm it animates.

_[tool: eval_js]_

_[tool: multi_screenshot]_

The character reads well — cute owl, flapping wings, arc blink-eyes, ear tufts, the Hoot facial-disc heart. Let me reload clean and confirm the form owl + motion.

_[tool: show_html]_

_[tool: eval_js]_

Flying owl flaps and the form submits, but the form mini-owl didn't render. Let me inspect.

_[tool: eval_js]_

_[tool: grep]_

The user removed the nav owl and the form owl span — they want the *flying* owl to be the whole story. I'll align the logic: drop the now-dead mini-owl/thinking code so the form responds crisply. Let me read the current logic.

_[tool: read_file]_

_[tool: read_file]_

_[tool: dc_js_str_replace]_

_[tool: dc_js_str_replace]_

The gutter is tight at this width — I'll only show the flying owl when there's comfortable margin room and clamp it clear of the text column.

_[tool: str_replace_edit]_

_[tool: show_html]_

_[tool: eval_js]_

Let me lower the show threshold slightly so it also appears at ~924 (the clamp keeps it clear of text), then capture it in place.

_[tool: str_replace_edit]_

## User

go back. this was ass. fuck no

## Assistant



## User

go back. this was ass. fuck no
'

## Assistant

Reverting the flying-owl direction now — restoring the previous version where the owl draws itself and threads down the page.

First, regenerating the owl path data:

_[tool: run_script]_

Now restoring the drawn-thread controller:

_[tool: write_file]_

Re-adding the owl-paths include before thread.js:

_[tool: dc_html_str_replace]_

_[tool: show_html]_

_[tool: save_screenshot]_

Reverted. The owl-that-draws-itself and the thread down the page are back, exactly as before the flying-owl attempt. Your manual edits (headline weight, removed nav/form owls) are preserved.

_[tool: delete_file]_

Back to the previous version. Let me know where you'd like to take it from here.

