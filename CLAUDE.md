# Resume repo — context for future agents

This repo produces and maintains Ethan Chennault's resumes as standalone, printable HTML files. It is not a generic "one resume" repo — it maintains **three role-specific resumes** (fullstack, backend, frontend) plus a **master content database** they're all drawn from, and two custom skills for generating/auditing them.

## Directory structure

```
ALL.md              — master pool of every resume-worthy fact (see below)
src/
  style.css         — shared stylesheet for every resume, plus @font-face declarations
  fonts/            — League Spartan + Noto Serif variable fonts used by style.css
  html/
    fullstack.html  — general fullstack-track resume
    backend.html    — general backend-track resume
    frontend.html   — general frontend-track resume
resumes/            — NOT source. This holds exported PDFs the user drops in by hand
                       (e.g. after printing one of the src/html/*.html files to PDF).
                       Never generate or edit HTML here — it's an output folder, not
                       an input one.
.claude/skills/
  tailor/           — tailors src/html/fullstack.html to a specific job posting
  review-resume/    — spawns adversarial hiring-manager-persona agents to critique resumes
```

Each `src/html/*.html` links `../style.css` — that relative path is correct as long as `html/` stays a sibling of `style.css` under `src/`. Don't move `html/` without updating that link, and don't confuse the source folder (`src/html/`) with the output folder (`resumes/`) — they are two different things that happen to sound similar.

## `ALL.md` — the source of truth

Everything a resume could ever say lives here first. It has four sections:

1. **Meta** (`## Meta`) — never gets copied into any resume. Target roles, strongest selling points, bullets to deprioritize, freeform notes (e.g. "HPE is old, only include if space allows"). Read this before selecting content for any resume.
2. **Skills pool** (`## Skills`) — four categories: Programming Languages, Frontend, Backend, Infrastructure / DevOps. This is the *complete* set of skills that may ever appear on a resume; a resume's skills section is always a filtered subset of this, never a superset.
3. **Experience** (`## Experience`) — one `###` entry per employer, each with a `**dates | location**` line and a `> **Context:**` blockquote. The context block is background for whoever's writing the resume (project names, team size, stack, and a `Good for: X, Y, Z` role-fit line) — it never appears in a resume itself. Bullets follow, each optionally starting with `⭐` (priority marker — include these first) and ending with a `` `[tag, tag, ...]` `` suffix used for skill-matching. Strip the tag suffix before it ever reaches an HTML file.
4. **Projects** (`## Projects`) — same format as Experience, for personal/team projects.

**Redundancy is fine in `ALL.md` and expected.** It's a log of everything done, not a curated document — two different jobs can each have their own "built a component library" bullet. Never "clean up" `ALL.md` by deduplicating it. Redundancy only becomes a problem at the resume level (see below).

## Building or editing a resume: hard rules

These were all learned the hard way (either from repeated iteration or from repeated hits during adversarial review). Follow them by default; don't relitigate them without a reason.

**Never fabricate.** Every fact, metric, and technology on a resume must trace back to something in `ALL.md`. Trimming/rewording for space or narrative flow is fine (see "build a story" below); inventing a new claim is not.

**Every listed skill must be backed by a bullet on that specific resume.** This is the single most common finding across every round of adversarial review — a skill sitting in the Technical Skills section with zero corresponding bullet on that page is an instant credibility hit in a phone screen. When you cut a bullet, check whether any skill it was the sole source for needs to be cut too.

**Technical Skills formatting rules:**
- Programming Languages always comes first and is never dropped, even if thin.
- Merge TypeScript + JavaScript into one `JavaScript/TypeScript` entry whenever both would appear.
- Order the remaining categories (Frontend / Backend / Infrastructure-DevOps) by relevance to that resume's role — most relevant right under Programming Languages. Fullstack resumes can keep the neutral left-to-right default.
- Drop any non-language category that ends up with ≤2 items rather than padding it to survive.
- Never list "CI/CD" itself as a skill — it's a practice, not a technology. List the actual tools (GitHub Actions, Jenkins, etc.).

**Resume-level redundancy is a real problem, even though `ALL.md`-level redundancy isn't.** If two bullets you've selected for the same resume would read as the same accomplishment told twice (same theme, same tech, back-to-back), swap one out or rewrite it to differentiate. Reviewers have also flagged the subtler version of this: multiple experience entries all independently shaped like "built a FastAPI backend + React/TypeScript frontend for X" — even with different literal facts, repeating the exact same sentence template reads as one story copy-pasted three times. The fix is to vary sentence structure/what's foregrounded per bullet, not to quietly drop a word (dropping a word that's still true elsewhere just creates a new inconsistency — this happened once and got caught immediately).

**Build a story, don't transcribe verbatim.** You don't owe `ALL.md` a 1:1 bullet mapping. It's fine to merge, reorder, or re-split bullets so a resume reads as a coherent narrative instead of disconnected facts — e.g. if you're including bullets that go deep on sub-components of a project (schema design, then the API) but nothing ever introduces what the project *is*, fold a short intro clause into the first bullet so the later one can build on it without re-explaining. Still never invent facts — only rearrange ones already in `ALL.md`.

**Sanity-check claims about what an employer actually does.** E.g. The Aerospace Corporation is an FFRDC providing engineering oversight/advisory work — it doesn't itself operate rocket launches, so "...the company oversees" is accurate where "...the company manages/launches" would overclaim.

**Be wary of absolute/round-number claims** ("100% uptime," "every launch," "100% test coverage"). Even when literally true, these disproportionately draw skepticism from adversarial reviewers — they read as inflated regardless of accuracy. Prefer giving the mechanism/context over a bare absolute, or accept the claim only after conscious tradeoff (this has come up repeatedly and is sometimes kept anyway by deliberate choice — that's a legitimate call, just make it consciously).

**No padding when balancing line lengths (see next section).** When a bullet needs to get longer to match its siblings, add a genuine additional fact or consequence — never empty adjectives or hedge words. Real examples that got caught by reviewers and had to be ripped out: "freely" and "own personal" (in "...freely create... to fit their own personal workflow"), "...adopted across 4 projects division-wide **so far**", "a fully scheduled, self-healing GitHub Actions workflow pipeline running around the clock" (it was just an hourly cron job restated three ways). If you can't find genuine substance to add, trim the *other* bullets down to match instead of padding.

## Page-fit and line-balance verification (mandatory after every edit)

Every resume must render as exactly one US Letter page (8.5in × 11in, i.e. an 816×1056px viewport at 96dpi) with **zero overflow**, and — within any single experience/project entry — **every bullet must render the same number of lines**. Neither of these is optional, and neither can be eyeballed from the HTML source; both must be verified by actually rendering the page, every time you change bullet text.

This project has no local Playwright install (see `find / -name playwright` or check a sibling project's `node_modules` if a prior session found a working one — do not assume npm/playwright are globally available). Install it locally if none exists (`npm i -D playwright` + `npx playwright install chromium`), or locate an existing install path before scripting against it.

Key gotchas learned the hard way:
- **Custom fonts load asynchronously.** Always `await page.evaluate(() => document.fonts.ready)` (plus a short timeout) before measuring anything, or line-wrap measurements will be silently wrong and inconsistent between runs.
- **`#resume` has a fixed height and `overflow: hidden`**, so `scrollHeight` alone won't reveal true overflow. Temporarily set `el.style.height = 'auto'; el.style.overflow = 'visible'` via `page.evaluate`, *then* read `scrollHeight` and compare to 1056px.
- **Don't hardcode the per-line pixel height.** `style.css`'s `line-height` and `h1` font-size have been tuned many times during design iteration — read the live computed value (`getComputedStyle(li).lineHeight`) instead of a remembered constant.
- **Line-wrap depends on font-size and container width, not line-height.** Changing `line-height` globally (as has happened several times for visual spacing) does not change where any bullet wraps — only how tall each line renders. So re-balancing bullet line-counts survives a line-height change; you only need to re-check total page height, not re-verify which bullets are 2 vs. 3 lines.
- **Word/character count is only a rough guide for hitting a target line-count** — a bullet can sit right at ~200-210 characters (at this container width) and still not wrap the way you'd predict, because wrapping depends on where individual words break, not total length. Always re-measure after every text edit instead of trusting an estimate.
- To fill genuinely leftover page space (more than ~one line's worth of slack), prefer adding back a real `Projects` bullet over inflating existing bullets — but never let filling space cause overflow. A slightly under-full page beats an overflowing one every time.

## The two skills

**`tailor`** (`.claude/skills/tailor/`) — takes a job posting and rewrites `src/html/fullstack.html` (only ever that file) to match it: extracts required/preferred skills from the posting, scores `ALL.md` entries for relevance via their context blocks, selects bullets by a `⭐`+tag-match priority order, rebuilds the skills section, writes the HTML, verifies one-page fit and line-balance, and — as its last step — invokes `review-resume` against the file it just wrote to get an independent critique before calling the job done.

**`review-resume`** (`.claude/skills/review-resume/`) — the adversarial QA step. Spawns one **fresh, non-fork** general-purpose agent per resume (a fork would inherit the bias of whatever session wrote the resume; a fresh agent judges it cold, the way a real recruiter would), each playing a skeptical hiring-manager persona for that resume's role. Can run standalone against any/all resumes in `src/html/`, or be invoked by `tailor`. Reports cross-cutting findings (things multiple reviewers independently caught) before per-resume specifics, and always asks before applying fixes rather than auto-rewriting off of reviewer opinion.

If you're extending this repo's automation further, prefer adding a new skill that composes with these two over baking more logic directly into `tailor` — that's the established pattern (`review-resume` was split out from `tailor` for exactly this reason).

## About the candidate (for context, not to be edited casually)

Ethan Chennault — current UCI undergrad (B.S. Computer Science + B.S. Business Information Management, expected June 2027), targeting fullstack/backend/frontend SWE internships. Current/past experience: The Aerospace Corporation, Lawrence Berkeley National Labs, UCI (research assistant), plus a personal project (UCI ZotBins) and older entries (HPE, Sense, Facets, Unimarket) that live in `ALL.md` but mostly don't make it onto current resumes due to space/relevance. Don't add new employers, dates, or projects without being told — this file describes the *system*, `ALL.md` is the *data*.
