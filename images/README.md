# Logo assets — notes (2026-09-01)

## Files

| File | Role | Status |
|---|---|---|
| `memazing_logo_9.pdf` | **Master.** Plain PDF saved from Illustrator with "Preserve Illustrator Editing Capabilities" **unchecked**. Single representation: what renders is what Illustrator opens. Editable both by hand (Illustrator) and programmatically. | Current, has all fixes |
| `memazing_logo_9.svg` | **Deployed web asset.** Referenced by every page header. | Current, has all fixes |
| `memazing_logo_9.ai` | Original Illustrator export. | **Stale** — still contains the misaligned crossbar (see below). Do not treat as source. |
| `MemazingLogoNoSubtitle.svg` + other files | Previous logo generation, no longer referenced by any page. | Kept for history |

## How the site uses the logo

All six pages (`index.html`, `legal/eula`, `legal/privacy`, `apps/claude-mcp/{,connect,disconnect}`) reference
`images/memazing_logo_9.svg`. The SVG stores the letters in their original **dark** color
(`#231f20`, from CMYK `0 0 0 1`); the CSS applies `filter: invert(1)` to the `<img>`, which is what
makes the logo appear light on the black site background. Keep new exports dark for this reason.

## Fix 1 — crossbar alignment

The wordmark's first "e" has a horizontal cut ("crossbar"). In the original AI export this was a
**white rectangle painted on top** of the letter at `x 97.025→125.674, 3.248 pt tall` — slightly
thinner than, and vertically offset from, the slit built into the glyph itself
(top `y = 476.768`, bottom `y = 480.411`, measured from the top of the A4 artboard). Where the
rectangle met the native slit there was a visible step on both edges, and the rectangle overshot
the letter's left edge.

Fixed by aligning the bar exactly to the native slit: `x 99.8→128`, `y 476.768→480.411` (3.643 pt).

## Fix 2 — true knockout (transparent cut)

The white-rectangle approach only looks right on white or (via the invert filter) black
backgrounds; on a photo or colored background the bar would render as a white stripe. Both the PDF
and the SVG were therefore rebuilt so the cut is a **real hole**:

- The seven letters "m" + "mazing" remain **live text** (font: MuseoModerno-Black, subset-embedded
  in the PDF; `<use>` glyph references in the SVG).
- The "e" is a **compound vector path** in the same fill, constructed by outlining the glyph and
  deleting the horizontal band `y 476.768→480.411`. The band's intersections with the bowl's
  left-wall Bézier were computed exactly and the curve split there (de Casteljau,
  t ≈ 0.259 and 0.623), so the cut edges are exact. The result is three subpaths: upper bowl,
  its counter (the hump), and the lower curve + foot. There is no white overlay anywhere.

Consequence for editing: the "e" is artwork, not type. Retyping the word in Illustrator will not
recreate the cut; edit the compound path instead. The PDF's text object is literally `(m)` +
`(mazing)` positioned at x = 29.8633 and 140.68718 pt, baseline y = 350.6221 pt (bottom-up PDF
coordinates), size 72.8625 pt — the "e" path sits between them.

## Regenerating the web SVG from the PDF

The SVG is a crop of the same artwork: `viewBox="31 432.67 372.67 75.33"` (the wordmark's ink
bounding box plus ~2 pt padding on the A4 page). If the PDF master changes, re-derive the SVG with
PyMuPDF (`page.get_svg_image()`), reapply the viewBox crop, and keep the fills dark.

## Illustrator save settings

When re-saving the PDF master from Illustrator, keep **"Preserve Illustrator Editing
Capabilities" unchecked**. Leaving it on embeds a private Illustrator payload that Illustrator
reads *instead of* the visible PDF content — programmatic edits would then be invisible in
Illustrator, and the file balloons from ~54 KB to ~370 KB.
