# Template: classic-serif

- **Type:** CV
- **Source extension:** .tex
- **Engine/toolchain:** lualatex
- **Page limit:** 1 page
- **Fonts:** TeX default serif (Latin Modern) — distribution font, no external font files required
- **Class/packages:** `\documentclass[letterpaper,10pt]{article}` with fullpage, xcolor, titlesec, enumitem, hyperref, fancyhdr, babel, tabularx — all standard TeX Live / MiKTeX packages, no custom `.cls`/`.sty`

## Compile command

```
rm -f <file>.pdf && mkdir -p build && lualatex -interaction=nonstopmode -output-directory=build <file>.tex && mv build/<file>.pdf ./
```

Run it **twice** — `hyperref` writes outlines on the first pass and needs a
second to settle. On Windows/PowerShell drop the `rm -f`/`mkdir -p`/`mv` and
run `lualatex -interaction=nonstopmode -halt-on-error <file>.tex` twice from the
output directory.

## Style rules

- **Section order is FIXED and education-first**: Education → Work Experience →
  Projects → Achievements and Profile Links → Technical and Non-Technical
  Skills. Do not insert a summary/profile-statement section; Education leads.
- Section headings render in **small caps with a horizontal rule beneath**
  (defined once via `\titleformat{\section}`, so `\section{Education}` is all
  the drafter writes). Rules are defined in the preamble — do not add
  per-section formatting.
- **No summary section.** The summary line belongs in the cover letter.
- Every bullet carries a **number** — throughput, latency, scale, coverage, or
  time saved. A bullet with no metric is a bullet to cut.
- Macros available: `\resumeSubheading{role}{city}{org}{dates}`,
  `\resumeProjectHeading{title}{stack + URL}{dates}`, `\resumeItem{...}`,
  `\resumeSubHeadingListStart/End`, `\resumeItemListStart/End`, `\sepbar`.
- Bullets use `\textbullet`. Do not switch to `\resumeItem` from a nested list.
- Contact line is a single centred block: name, then city / phone / email /
  LinkedIn / GitHub separated by `\sepbar`.
- Body is the **default serif** — there is deliberately no font package, so the
  template compiles on any TeX install with zero font dependencies. Do not add
  `fontspec`, `times`, or `libertine`.
- Skills are grouped with `\textbf{Label:}` prefixes in a flat `itemize`. Each
  label gets its own line; do not nest.

## Known pitfalls

- **Never tighten spacing with negative `\vspace` or negative `itemsep`.** A
  wrapped bullet's second line will collide with the next bullet — glyph boxes
  intersect by ~3pt and the text becomes unreadable. **LaTeX will not warn**:
  there is no `Overfull \hbox` and the log is clean, so this failure is
  invisible unless you inspect the rendered PDF. Tightness comes from
  `\linespread{0.94}` in the preamble, which scales leading uniformly while
  keeping the baseline skip above the descender+ascender depth. Keep
  `itemsep=0pt` and keep `\resumeItem` free of trailing `\vspace`.
- **Verify the rendered PDF, not just the log.** After compiling, check that
  adjacent text lines do not overlap vertically — compare line bounding boxes,
  or open the PDF and read it. `\linespread` below ~0.90 reintroduces the
  collision; if the content needs more room, cut a bullet instead.
- **Escape every literal `%` as `\%`.** An unescaped percent starts a comment
  and silently swallows the rest of the line — including a closing brace —
  which surfaces as `! File ended while scanning use of \resumeItem.` This is
  the single most common failure when filling this template.
- **The page limit is 1 page and the geometry is tuned to hit it.** When content
  overflows, cut a bullet or shorten a line — do **not** loosen
  `\topmargin`/`\textheight`. A 2-page result fails `/apply`'s page check.
- `\resumeSubheading` and `\resumeProjectHeading` use a `p{}` parbox column
  rather than `l{}`. This is deliberate: a bare `l{}` column overflows the
  tabular by a few points when the left cell is long, producing an
  `Overfull \hbox`. If you change the column spec, keep `p{}`.
- The `\usepackage[empty]{fullpage}` + explicit `\addtolength` margin block is
  redundant-looking but load-bearing — `fullpage` sets the margins and the
  `\addtolength` calls pull them in further. Removing either changes pagination.
- Do not use `\\` inside `\resumeItem` or `\resumeSubheading` arguments; the
  macros already own line breaking.
- `[N]`, `[M]`, `[P]` inside the *example* placeholders are illustrative — they
  describe the shape of a quantified bullet, not values to copy.