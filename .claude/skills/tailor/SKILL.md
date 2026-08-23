---
name: tailor
description: Tailors the resume in src/html/fullstack.html to a job posting, drawing all content from ALL.md. Uses skill tags, context blocks, and priority markers in ALL.md to select the best bullet points, updates technical skills to match, and verifies the resume fits on one page without overflow.
---

Tailor the resume in `src/html/fullstack.html` to a job posting by following these steps carefully.

## Step 1 - Get the job posting

If the user provided the job posting as args or pasted it in the conversation, use that. If not, ask them to paste the full job posting text (or a URL you can fetch).

If a URL was provided, fetch it and extract the relevant job description text.

## Step 2 - Read source files

Read both of these files in full before proceeding:
- `ALL.md` — the complete pool of resume content, with skill tags, context blocks, and priority markers
- `src/html/fullstack.html` — the resume to rewrite

## Step 3 - Analyze the job posting

Extract:
1. **Required/preferred skills** — every language, framework, tool, or concept explicitly mentioned
2. **Role priorities** — what kind of work matters most (e.g. frontend-heavy, backend systems, infra, data pipelines, etc.)

Normalize skill names for matching — e.g. "React.js" matches `[react]`, "Postgres" matches `[postgres]`.

## Step 4 - Assess entry relevance using context blocks

Each entry in ALL.md has a `> **Context:**` block. Read the "Good for:" line of each entry and compare it to the role priorities you extracted. Use this to decide:
- Which experience/project entries to **include** at all
- How many bullets each entry deserves (more relevant = more bullets)
- Whether to omit HPE entirely (its context block flags it as low-priority for most roles)

## Step 5 - Select bullet points using tags and priority markers

For each included entry, select bullets using this priority order:

1. **⭐ bullets whose tags overlap with required job skills** — always include these first
2. **⭐ bullets whose tags overlap with preferred/adjacent skills** — include if space allows
3. **Unstarred bullets whose tags overlap with required skills** — include next
4. **Unstarred bullets whose tags overlap with preferred skills** — include only if space allows
5. **Bullets with no tag overlap** — exclude unless the entry would otherwise have zero bullets

Tag matching: a bullet's tags are the comma-separated list in backticks at the end of the line. Strip these tags — do not write them into the HTML.

Hard limits to stay on one page:
- LBNL: 4-5 bullets max
- ZotBins / UCI Research: 2-3 bullets max
- HPE / personal projects: 1-2 bullets max, or omit the entry entirely
- Never include HPE's "Learned about Git..." bullet

**Redundancy applies to the resume, not to ALL.md.** ALL.md is a complete log of everything done — it's expected and fine for bullets across different entries to overlap in theme (e.g. two different jobs both having a "built a React component library in Storybook" bullet). Do not "clean up" or de-duplicate ALL.md itself. But when two tag-matched bullets *you're both selecting into the same resume* would read as the same accomplishment told twice (same theme, same tech, back-to-back), that's a resume-quality problem: prefer swapping one out for a different bullet from that entry so the resume shows breadth instead of repetition.

## Step 6 - Update technical skills using tags

Build the skills section from ALL.md's `## Skills` pool. Include a skill if:
- It appears in a tag on any bullet you selected, **or**
- It is explicitly required by the job posting and present in the skills pool

Formatting and structure rules:
- **Programming Languages always stays first**, regardless of role, and is never removed even if it ends up with very few items.
- When both TypeScript and JavaScript would appear in Programming Languages, always merge them into a single `JavaScript/TypeScript` entry — even if only one of the two is individually tag-matched.
- **Order the remaining categories (Frontend, Backend, Infrastructure / DevOps) by relevance to the role**, most relevant directly under Programming Languages, least relevant at the bottom. E.g. for a backend-leaning posting: Programming Languages, Backend, Infrastructure / DevOps, Frontend. For a frontend-leaning posting: Programming Languages, Frontend, Backend, Infrastructure / DevOps. For a balanced/fullstack posting, the original left-to-right order (Frontend, Backend, Infrastructure / DevOps) is a reasonable default.
- **Drop any non-language category that ends up with 2 or fewer items.** Don't pad it to survive — just omit the row.
- **Never list "CI/CD" itself as a technical skill.** It's a practice/methodology, not a specific technology — list the concrete tools instead (GitHub Actions, Jenkins, etc.). This applies everywhere, not just when trimming for space.

Then check: are there skills **explicitly required by the job posting** that are **not in ALL.md's skills pool at all**? Report these to the user as:

> **Missing skills not in ALL.md:** Kubernetes, GraphQL

Do not fabricate or add skills that aren't in ALL.md.

## Step 7 - Write the updated resume

Rewrite `src/html/fullstack.html` with the selected content. Rules:
- Preserve all HTML structure, class names, and formatting conventions exactly
- Use `<p>` tags inside `#skills-list`, not `<div>`
- Strip all tags (backtick blocks), context blocks, ⭐ markers, and Meta content — none of these appear in the HTML
- Do not change the header, education section, or page metadata
- Start from the bullet text in ALL.md verbatim (minus the tag suffix) as your factual base. You may lightly trim or extend a bullet's wording for line-fit purposes (Step 8) — never fabricate metrics, technologies, or claims not already in ALL.md
- **You don't have to copy each ALL.md bullet 1:1 into its own resume bullet — build a story, not a transcription.** When several ALL.md bullets describe pieces of the same project, it's fine to merge, reorder, or re-split them so the resume reads as a coherent narrative rather than disconnected facts. The clearest example: if you select bullets that go deep on sub-components of a project (e.g. "designed the database schema" and "built the API") but no bullet ever establishes what the project *is*, a reader hits unexplained context (a product name, an unintroduced acronym) with no anchor. Fold a short intro clause from the project's context block or its higher-level ALL.md bullet into the first of those bullets (e.g. "Modernized a day-of-launch monitoring application (Winds) by designing its MongoDB schema...") so the later bullet can build on it (e.g. "Built the Winds REST API...") without re-explaining. Still never invent facts — only rearrange and connect ones already in ALL.md.
- Keep bullets at resume altitude: don't spell out exhaustive low-level implementation steps (specific internal libraries, queueing mechanics, etc.) unless that exact technology is explicitly required by the job posting. Favor the outcome and the named technologies that matter for matching over a blow-by-blow of how it works internally
- Watch for claims that overstate an employer's actual role. E.g. The Aerospace Corporation is an FFRDC that provides engineering oversight/advisory work — it doesn't itself operate rocket launches, so "used on every rocket launch the company oversees" is accurate where "...the company manages" or "...launches" would overclaim. When a bullet describes what a company does as a whole (not just the candidate's specific work), sanity-check the verb against what that company actually is.

## Step 8 - Verify one-page fit, balance bullet lengths, and fill the page

Custom fonts in this project load asynchronously — always wait for `document.fonts.ready` before measuring anything, or line-wrap measurements will be wrong.

Use Playwright to open the file in a browser at a 816×1056px viewport (standard US letter at 96dpi):

1. **Measure line counts.** For every `<li>` inside `#experience`, compute rendered lines as `Math.round(li.getBoundingClientRect().height / 19.872)` (19.872px = the 0.9rem/1.38-line-height text in this stylesheet, at the default 16px root).
2. **Balance each experience-item.** Within a single `.experience-item`, every bullet must render the same number of lines. Where they don't match, edit bullet text (trim the longer ones, or add reasonable clarifying detail to the shorter ones — never invent new claims) and re-measure until they match. Do this per section independently; different experience-items can have different line counts from each other. Word/character count is only a rough guide here — where a line breaks depends on word lengths, so a bullet can sit right at a threshold (e.g. ~200-210 characters at this container width) and still not wrap the way you'd expect. Always re-measure after each edit rather than trusting a character-count estimate.
   - **When lengthening a short bullet, add a genuine additional fact — never adjective-stacking or hedge words.** A real reviewer will catch this: "a fully scheduled, self-healing GitHub Actions workflow pipeline running around the clock" was called out as padding (it's just "an hourly cron job" restated three ways), as was "letting analysts freely create, resize, and reorganize chart cards to fit their own personal workflow" ("freely" and "own personal" add nothing) and hedges like "adopted across 4 projects division-wide **so far**." Instead, add a real consequence or detail the fact implies but doesn't state — e.g. "removing a manual refresh step engineers previously had to run by hand," or "letting each analyst rearrange chart cards to match how that specific launch's data needed to be reviewed." If you can't find genuine additional substance, it's better to trim the *other* bullets down to match the shorter one than to pad with empty adjectives.
3. **Check total page fit.** With `#resume`'s `height` set to `auto` and `overflow` set to `visible` (its normal fixed height + `overflow: hidden` will hide true overflow from `scrollHeight`), read `scrollHeight` and compare to 1056px.
4. **If it overflows:** cut the lowest-priority bullet (per the Step 5 priority order, from the entry with the most bullets), re-balance that section's remaining bullets per step 2, and re-check. Repeat until it fits.
5. **If there's meaningful slack left** (more than roughly one bullet-line's worth, ~20px): fill it rather than leaving the page visibly empty at the bottom, in this order:
   - First, see if evening out an experience-item's bullets to a taller line count (Step 2, choosing the larger count when balancing) is enough.
   - If slack remains, add back a `Projects` section entry — the most relevant one per Step 4's context-block matching — starting with a single bullet. Test-render after adding; if it overflows even at one bullet, that project doesn't fit — try a shorter bullet or drop the project entirely rather than overflow.
   - Never let filling the page cause overflow. A slightly under-full page is always preferable to overflow.
6. Once it fits and is balanced, take a final screenshot and show it to the user.

## Step 9 - Report back

Tell the user:
- A brief summary of which entries were included and how many bullets each got
- Which skills were included vs. trimmed from the skills section
- Any skills the job requires that are missing from ALL.md
- Which bullets were cut for fit (if any) and why
- Whether a project was added back to fill remaining page space, or if the page was left slightly under-full to avoid overflow

## Step 10 - Independent critical review

Invoke the `review-resume` skill against `src/html/fullstack.html` (the file this skill just wrote) to get an adversarial, independent critique before considering the tailoring done — that skill spawns a fresh hiring-manager-persona agent with no memory of how the resume was built, which is exactly what's needed to catch padding, unbacked skills, or overclaiming that look fine from inside the editing session.

Present its findings to the user and ask whether to apply the suggested fixes — per `review-resume`'s own Step 4, don't silently rewrite based on reviewer opinion alone.
