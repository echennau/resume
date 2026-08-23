---
name: review-resume
description: Critically reviews one or more resumes in src/html/ by spawning independent, skeptical hiring-manager agents (one per resume) that read the rendered page and HTML source and report credibility issues, padding, unbacked skills, and redundancy. Can be run standalone for an audit of all resumes, or invoked by the tailor skill to critique a resume it just produced.
---

Critically review resume(s) in `src/html/` by having independent agents play a skeptical hiring manager for the matching role. This is an adversarial check — its whole point is to catch things the person who wrote the resume won't notice, so the reviewing agents must not share context with whatever session produced the resume.

## Step 1 - Determine scope

- If invoked with specific file(s) named (e.g. by the tailor skill, which only ever edits `src/html/fullstack.html`), review just those.
- If invoked standalone with no target specified, review every `src/html/*.html` file that currently exists.

Infer the target role for each file from its name (`frontend.html` → frontend engineering internship, `backend.html` → backend, `fullstack.html` → fullstack). If a resume was produced by tailoring against a specific job posting, use that posting's role framing instead.

## Step 2 - Render and sanity-check each resume first

Before sending anything to a reviewer, confirm the resume itself is valid — reviewing a broken/overflowing render wastes the review. For each resume:

1. Wait for `document.fonts.ready` (custom fonts load async in this project).
2. Screenshot it full-page at a 816×1056px viewport to a temp path (e.g. `/tmp/review_<name>.png`).
3. Check for overflow: set `#resume`'s `height` to `auto` and `overflow` to `visible`, then compare `scrollHeight` to 1056px.

If any resume overflows or fails to render, fix that first (or tell the user) rather than sending a broken artifact for review.

## Step 3 - Launch one independent reviewer agent per resume, in parallel

Use fresh (non-fork) general-purpose agents — never forks. A fork inherits the conversation that wrote the resume and will be biased toward defending its own choices; a fresh agent has no memory of how or why any bullet was written and will judge it cold, the way a real recruiter would.

For each resume, launch an agent with this framing (adjust the role and file paths per resume):

> You are a skeptical, experienced hiring manager/technical interviewer reviewing a resume for a **[role] software engineering internship** application. You have seen hundreds of intern resumes and are not easily impressed. Be honest and critical — do not be sycophantic. The candidate is a current undergrad, so calibrate expectations for an intern, not a senior engineer, but still hold the bar for clarity, credibility, and technical substance.
>
> 1. Read the rendered resume image at `[screenshot path]`.
> 2. Read the underlying HTML source at `[resume path]` for exact wording.
> 3. Optionally cross-reference `ALL.md` (the candidate's full master list of experience/projects/skills with extra context not shown on the resume) — only to sanity-check claims, not to critique reasonable omissions.
>
> Critique specifically as a [role] application. Consider:
> - Does it show real [role] depth, or lean on buzzwords/technology name-dropping without substance?
> - Are claims specific and credible, or vague/inflated? Look for padded phrasing (empty adjectives, hedge words, adjective-stacking) versus genuine substance.
> - Any bullets that feel like filler, repetition, or a stretch to fit space?
> - Is the technical skills section coherent, and is every listed skill actually backed by at least one bullet on this specific resume?
> - Formatting/readability: information density, one-page usage, awkward wrapping, redundant info between bullets and headers, whether dates/roles read as credible (e.g. tenure vs. claimed scope of impact).
> - What questions/doubts would this raise going into a phone screen?
> - Anything that reads as AI-generated filler/fluff versus authentic engineering description.
>
> Report back: overall impression (1-2 sentences), concrete strengths (bulleted, specific), concrete weaknesses/red flags (bulleted, quote exact text where possible), and a prioritized top 3-5 fixes. Under ~500 words.

Launch all reviewer agents in a single message (parallel tool calls) so they run concurrently.

## Step 4 - Consolidate and report

Once all reviews return (they arrive as background task notifications — don't block or poll, just wait for them), consolidate into one report:

1. **Cross-cutting issues first** — anything flagged independently by more than one reviewer (e.g. an unbacked skill that appears on multiple resumes, a tenure/credibility mismatch, a padded phrase reused across files) is the highest-signal finding and should lead the report.
2. **Per-resume specifics** after that, grouped by file.
3. Ask the user whether to apply the fixes — do not silently rewrite the resumes based on reviewer opinions alone, since some critiques are matters of taste the user may disagree with.
