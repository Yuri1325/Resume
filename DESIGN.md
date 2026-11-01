# DESIGN.md: uri-richardson.com

Personal resume + portfolio site for Uri Richardson. Static HTML on GitHub Pages (`CNAME` → www.uri-richardson.com).
Pages: `index.html` (resume), `portfolio.html` (cinematic project reel), `resume_pdf.html` (PDF source).

## Design read
Developer portfolio for recruiters, admissions readers and internship leads. Monochrome, film-inspired
("cinematic") language carried over from the resume page. Dials: VARIANCE 6, MOTION 7, DENSITY 3.

## Color tokens (CSS variables, `html.dark` swaps them)
| Token | Light | Dark | Use |
|---|---|---|---|
| `--bg` | `#f6f5f2` | `#080808` | page background |
| `--surface` | `#ffffff` | `#101010` | raised panels |
| `--border` | `#e3e1dc` | `#202020` | hairlines |
| `--accent` | `#2f2f2f` | `#e0e0e0` | links, strong text |
| `--text` | `#4c4c4c` | `#a8a8a8` | body |
| `--muted` | `#76736c` | `#7a7a7a` | secondary text (AA on bg) |
| `--white` | `#131313` | `#f5f5f5` | headlines (inverted naming kept from v1) |
| `--ai` | `#2648c9` | `#8ea6ff` | **only** marks AI-assisted work |

Rule: `--ai` is information, not decoration. It appears on the AI-assisted chip, the AI reel title card and
the reel indicator. Hand-built work uses neutral outline chips. Never use `--ai` anywhere else.

## Type
- Display: **Cinzel** 400/700/900 (names, project titles, reel title cards). Wide tracking, no italics.
- Body/UI: **Raleway** 300-600. Body 1rem/1.75, max 65ch.
- Scale (portfolio): hero name clamp(2.6rem, 8vw, 6.5rem); reel title clamp(2.4rem, 7vw, 5.5rem);
  project title clamp(1.6rem, 3.6vw, 2.6rem); body 1rem; meta 0.8rem.

## Shape + surfaces
- Radius: 6px on media frames and panels, full pill on chips and buttons. No other radii.
- Film grain overlay fixed at 3% opacity (pointer-events none). No drop shadows except media frames
  (`0 30px 80px -40px rgba(0,0,0,.45)`).

## Motion (GSAP 3.12.5 + ScrollTrigger from cdnjs)
- Hero: letterbox bars retract with scroll (scrub). One orchestrated load fade.
- Reel title cards: pinned for one viewport on >=768px, title tracking tightens on scrub.
- Project media: "aperture" reveal, clip-path inset opens as the frame enters (scrub), media scales 1.12 → 1.
- Videos autoplay muted only while on screen (IntersectionObserver), paused otherwise.
- `prefers-reduced-motion: reduce` → no GSAP, everything static, videos do not autoplay.
- Never `window.addEventListener('scroll')`.

## Components
- `.chip--ai` / `.chip--hand`: provenance label on every project. Required.
- `.scene`: one project = aperture media + details grid + optional gallery strip.
- Lightbox: native `<dialog>` for full-size screenshots.

## Content rules
- No em-dashes in visible copy. Factual descriptions only (from source code / live app).
- Media lives in `portfolio/` (webp stills <= 1600px, H.264 mp4 clips, no audio).
