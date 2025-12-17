# Memazing Website

Corporate website for Memazing, Inc. Hosted on GitHub Pages at [memazing.com](https://memazing.com).

## Contents

- Mission statement
- Staff and advisors (with links to Nobel Prize, Turing Award, etc.)
- Media section with YouTube videos and press coverage (NYT, Guardian, Economist, Nature)
- Contact link

## Logo

The logo (`images/MemazingLogoSubtitleOutlineFlyHumanCaps.svg`) uses:
- SVG format with text converted to outlines for consistent rendering across browsers (fixes letter spacing issues in Samsung mobile browser)
- 3 decimal places precision for optimal file size
- CSS `filter: invert(1)` to display white-on-black in the header
- `transform: scale()` to appear larger than its layout box

PNG versions also exist for fallback purposes.

## Responsive Design

The site handles multiple device configurations:

| Condition | Logo Scale | Notes |
|-----------|------------|-------|
| Desktop (>1100px) | 1.4 | Full-size logo |
| Tablet/narrow desktop (<=1100px) | 1.1 | Prevents overflow at intermediate widths |
| Mobile landscape (height <=500px) | 1.1 | Regular phones rotated |
| Foldables landscape (height <=1000px, touch) | 1.1 | Galaxy Fold and similar devices |

The header uses `overflow: hidden` to clip the scaled logo and prevent horizontal scrollbar issues.

## Technologies

- HTML5
- CSS3 with responsive media queries
- SVG graphics
- GoatCounter analytics
