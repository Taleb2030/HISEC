# Handoff Spec: HISEC website (light theme)

## Overview
Static, multi-page marketing site for HISEC, a distributor and integrator of electronic security systems in Nouakchott, Mauritania. Visitors are facility managers, companies, public bodies and homeowners. **The site's one job:** show the product range and get visitors to call (+222 45 25 56 90) or visit the showroom.

- Stack: Vite 8 multi-page app, vanilla JS (ES modules), plain CSS. `npm run dev` for development, `npm run build` to produce `dist/`. See `README.md`.
- Libraries (npm): `gsap` ^3.15 (+ ScrollTrigger), `lenis` ^1.3.27 (+ `lenis/dist/lenis.css`, imported in `src/main.js`).
- Fonts (Google Fonts): Archivo (variable width/weight), IBM Plex Sans, IBM Plex Mono, plus IBM Plex Sans Arabic on `/ar/` pages.
- Languages: French `/` (default, `x-default`), English `/en/`, Arabic `/ar/` (`dir="rtl"`). Every page links its translations with `<link rel="alternate" hreflang>`, which carry `vite-ignore` so Vite doesn't try to bundle them.

### Internationalisation
- All text lives in `content/{fr,en,ar}.mjs`; shared data (phone, solution order, images, camera numbers) lives in `content/shared.mjs`.
- `scripts/generate.mjs` writes 12 pages × 3 languages. A Vite plugin (`vite.config.js`) runs it on start and on build, and re-runs it and reloads the browser when `content/` or `scripts/` changes.
- Slugs are identical across languages (`/en/intrusion.html`, `/ar/intrusion.html`), so the language switcher keeps the visitor on the same page.
- Language button (`.langsel`): one control in the header at every screen size (globe + current code + chevron). It is a disclosure: a `button[aria-expanded]` controls a list of 3 links (`hreflang`, `lang`, current one `aria-current="true"` with a check mark). Opens on click, Enter/Space or ArrowDown (focus moves to the current language); ArrowUp/Down/Home/End move through the list; Esc closes and returns focus to the button; it closes when clicking outside or when focus leaves. GSAP: menu `autoAlpha 0→1, y −8→0, scale .96→1` (0.24s power3.out) with items staggered 0.04s; close 0.15s power1.in. Choosing another language fades `main` and the footer out (0.3s power2.in) while the header stays put, with Lenis stopped, then navigates. With reduced motion everything is instant. On back/forward-cache restore the page is reset.
- RTL: layout uses logical properties (`margin-inline-start`, `padding-inline`, `inset-inline-start`). Directional arrows carry `.ico--dir` and are mirrored with `scale: -1 1`; hover nudges use `--nudge` (1 / −1). Phone numbers are wrapped in `<span dir="ltr">`. The logo and the camera HUD stay LTR on purpose (brand mark, camera vernacular).
- Arabic typography: IBM Plex Sans Arabic for every role, letter-spacing forced to 0 (tracking breaks Arabic joins), headings at weight 700 with line-height 1.35, body text at 18px / 1.8, and no uppercase transforms.

### Pages
| File | Purpose |
|---|---|
| `index.html` | Home: hero + camera wall, key facts strip, about, solutions list, showroom teaser, contact band |
| `solutions-surete.html` | Overview of all 7 solution lines (tiles) |
| `securite-incendie.html`, `interphonie.html`, `videosurveillance.html`, `controle-acces.html`, `cables-accessoires.html`, `intrusion.html`, `haute-securite.html` | Solution detail template |
| `showroom.html` | Address, photo, map embed |
| `contact.html` | Phone, address, quote checklist, map embed |
| `404.html` | Not-found page (configure your host to serve it) |

Header, footer and the icon sprite come from templates in `scripts/generate.mjs`. Change them there, never in the generated HTML.

---

## Layout
- Container: `--container` 1240px max, side gutter `--gutter` 16px (<768) / 32px (≥768).
- Header height `--header-h`: 72px (<768) / 80px (≥768). The header is sticky and blurs its backdrop.
- Vertical rhythm: sections use `--space-9` (96px) top/bottom; 64px below 768px.
- Grids collapse to one column below the breakpoints listed under Responsive Behaviour.

## Design Tokens Used
Defined in `css/styles.css` `:root`. Every text pair passes WCAG AA (ratios checked).

### Color
| Token | Value | Usage | Contrast |
|---|---|---|---|
| `--color-primary` | #E31E24 | Primary buttons, icons, focus ring, camera corners | white text 4.69:1 |
| `--color-primary-hover` | #C3161C | Primary button hover/pressed | white text 6.08:1 |
| `--color-primary-ink` | #C3161C | Red text: links, eyebrows, active nav, `em`/`mark` | 5.80:1 on bg |
| `--color-primary-tint` | #FDEDED | Contact band, icon chips, quality pills | fg 15.7:1 |
| `--color-primary-tint-2` | #FAD9DA | Borders on tint surfaces | — |
| `--color-bg` | #FBF9F9 | Page background | — |
| `--color-surface` | #FFFFFF | Cards, header CTA outline buttons, status strip | — |
| `--color-panel` | #F3EFEF | Alternate sections, top bar, footer | muted 5.65:1 |
| `--color-border` | #E7E0E0 | Hairlines, card borders | decorative |
| `--color-border-strong` | #D6CCCC | List row dividers, outline buttons | decorative |
| `--color-fg` | #1C1617 | Body and heading text | 17.0:1 on bg |
| `--color-muted` | #665C5E | Secondary text | 6.15:1 on bg |
| `--color-success` | #1E7F47 | "Live" status dot only | 4.79:1 |
| `--color-hud` | rgba(28,22,23,.66) | Camera HUD label background (white text) | ≈15:1 |
| `--color-screen` | #EBE6E6 | Empty camera screen (image missing) | — |

There is no dark mode: the client asked for a light site.

### Typography
| Token | Value | Usage |
|---|---|---|
| `--font-display` | Archivo, `font-stretch: 118%` (`--display-stretch`), 700–900 | Headings, nav, buttons, row names |
| `--font-body` | IBM Plex Sans 400/500/600 | Body copy |
| `--font-mono` | IBM Plex Mono 400/500, uppercase, +0.1em tracking | Eyebrows, HUD, labels, footer titles |
| `--fs-3xl` | clamp(38px → 68px) | Page/hero H1, weight 900, −0.035em |
| `--fs-2xl` | clamp(30px → 44px) | Section H2, weight 800 |
| `--fs-xl` | 26px | Tile names, H3 |
| `--fs-lg` | 20px | Leads, row names |
| `--fs-base` | 17px | Body, line-height 1.6 |
| `--fs-sm` | 15px | Buttons, secondary copy |
| `--fs-xs` | 13px | Mono labels, crumbs |

### Spacing, shape, depth
| Token | Value |
|---|---|
| `--space-1 … --space-10` | 4, 8, 12, 16, 24, 32, 48, 64, 96, 128px |
| `--radius-sm` / `--radius` / `--radius-lg` | 6 / 12 / 18px; buttons and pills 999px |
| `--shadow-sm` / `--shadow` / `--shadow-lg` | Resting cards / feeds / hover and dropdowns |

---

## Components
| Component | Class | Variants / props | Notes |
|---|---|---|---|
| Camera feed (signature) | `.feed[data-feed]` | `--ratio` inline var (4/3, 16/10, 21/9); `.feed--mini` (tiles: hides bottom HUD); `.feed--wide` | Contains `.feed__screen` (image + fallback icon), `.feed__corners` (red brackets), `.feed__hud` (REC, CAM n°, label, clock). The HUD is `aria-hidden`; the `img` carries the description. |
| Camera wall | `.wall[role=group]` | — | 2×2 feeds on the home hero. Bottom-left label hidden; clock shows time only. Below 520px the clock is hidden too. |
| Button | `.btn` | `--primary`, `--outline`, `--block` | Min height 48px. A trailing arrow icon nudges 3px on hover. |
| Text link | `.link` | — | Red ink, 44px min target, arrow nudges on hover |
| Solution row | `.rows > li > a.row` | — | Icon chip, name, one-line description, arrow. 1 column below 900px, 2 columns above. |
| Solution tile | `.tile` | — | Mini feed + name + description + "Découvrir" |
| Key points | `.keypoints` | — | Sticky (top = header + 24px) at ≥1024px |
| Info card | `.info` | — | Icon disc + mono title + content |
| Call card | `.callcard` | — | Full red, phone number at display size |
| Contact band | `.band` | — | On every page except Showroom, Contact and 404 |
| Mega menu | `.nav__drop` / `.mega` | — | 2-column dropdown, 8 links |
| Mobile menu | `#mobile-menu` | — | Full screen below the header; `data-lenis-prevent` |

## States and Interactions
| Element | State | Behavior |
|---|---|---|
| `.btn--primary` | Hover | Background → `--color-primary-hover`; arrow +3px |
| `.btn` | Active | `scale(.98)` |
| `.btn--outline` | Hover | Border → `--color-fg` |
| Any focusable | Focus-visible | 3px `--color-primary` outline, 3px offset |
| `.nav__link` | Current page | Red text + 2px underline bar (`aria-current="page"`; the "Solutions" trigger gets `.is-current` on solution pages) |
| Mega menu | Mouse hover | Opens on pointerenter, closes 180ms after pointerleave. A click right after a hover-open keeps it open. |
| Mega menu | Keyboard | Enter/Space toggles; Esc closes and returns focus to the trigger; closes when focus leaves |
| Mobile menu | Open | `main`, footer and top bar set `inert`; page scroll locked (Lenis stopped); focus moves to the first link; Esc or the burger closes it and returns focus to the burger; closes automatically at ≥1024px and on back/forward-cache restore |
| `.row` | Hover | White background, icon chip turns solid red, arrow +4px |
| `.tile` | Hover | Lifts 4px, `--shadow-lg` |
| Feed image | Missing / 404 | `onerror` removes the `img`; the grey scan-line screen + outline icon remains |
| Map iframe | Loading | `--color-panel` background shows until tiles load |

## Responsive Behavior
| Breakpoint | Changes |
|---|---|
| ≥1024px | Desktop nav + "Appeler" CTA visible; burger hidden; hero, page head, split, detail and band switch to 2 columns; tiles 3 columns; key points sticky |
| 900–1023px | Burger menu; solution rows 2 columns; status strip in one row |
| 768–899px | Gutter 32px; about columns 2-up; info grid 3-up; footer 2 columns |
| 640–767px | Tiles 2 columns; maps 4:3; wide feed 4:3 |
| <640px | Single column everywhere; top-bar address hidden (phone stays) |
| <520px | Smaller HUD text and corners; camera wall hides clocks |

## Edge Cases
- **No photos supplied:** every feed shows the fallback screen. The layout never changes height because feeds use `aspect-ratio`.
- **Long labels:** HUD pills are capped at 60% of the feed width with an ellipsis. Solution names wrap; rows grow in height.
- **No JavaScript:** all content is visible. Menus fall back as follows: the desktop mega menu cannot open, so link to `solutions-surete.html`; the mobile burger does nothing (known limitation, acceptable for a brochure site).
- **Script failure:** GSAP and Lenis are bundled with `main.js`. If the bundle fails to load or run, an inline head script removes `.js` after 3s and everything shows statically.
- **Slow connection:** animated elements are hidden by CSS before first paint (only when motion is allowed), so there is no flash of visible content followed by hiding.
- **Time zone:** clocks show Africa/Nouakchott time (UTC+0) and fall back to UTC.

## Animation / Motion
All motion runs inside `gsap.matchMedia("(prefers-reduced-motion: no-preference)")`. With reduced motion, nothing animates, Lenis is not created, and the REC blink is disabled.

| Element | Trigger | Animation | Duration | Easing |
|---|---|---|---|---|
| H1 lines (`.line > span`) | Page load | `yPercent` 110 → 0, 0.09s stagger | 1s | power4.out |
| `[data-intro]` | Page load (+0.25s) | Fade in + `y` 18 → 0, 0.08s stagger | 0.7s | power3.out |
| Feeds above the fold | Page load (+0.3s, +0.14s each) | "Screen on": `clip-path` inset 50% → 0, then HUD + corners fade in | 0.75s + 0.3s | power3.inOut / none |
| Feeds below the fold | Top hits 85% of viewport, once | Same "screen on" | same | same |
| Feed image | Scroll (scrub) | Parallax `yPercent` −4 → 4 inside the screen | scroll-linked | none |
| `[data-reveal]` | Top hits 96%, batched, once | Fade in + `y` 26 → 0, 0.07s stagger | 0.8s | power3.out |
| `[data-row]` | Top hits 96%, batched, once | Fade in + `y` 30 → 0, 0.06s stagger | 0.8s | power3.out |
| `[data-count]` | Top hits 95%, once | Count 0 → value | 1.4s | power2.out |
| REC dot | Always | Blink, `steps(1)` | 1.6s loop | — |
| Page scroll | Wheel/trackpad | Lenis smoothing, `lerp: 0.1`, driven by `gsap.ticker` | — | — |
| Mobile menu items | Menu opens | Fade in + `y` 14 → 0, 0.025s stagger | 0.35s | power2.out |

## Accessibility Notes
- **Focus order:** skip link → top-bar phone → logo → nav (Accueil, Solutions trigger → 8 mega links, ShowRoom, Contact) → "Appeler" → main → footer.
- **Landmarks:** `header`, `nav[aria-label]` (main, mobile, breadcrumb), `main#contenu`, `footer`. The skip link targets `#contenu`.
- **ARIA:** the mega trigger is a `button` with `aria-expanded` and `aria-controls`; the burger has `aria-expanded` and a dynamic label ("Ouvrir/Fermer le menu"); breadcrumbs mark the current page with `aria-current="page"`; the camera wall has `role="group"` + `aria-label`; all decorative SVGs are `aria-hidden`.
- **Targets:** buttons ≥48px tall; nav, links, crumbs and footer links ≥32–44px.
- **External links** (Google Maps) open in a new tab and include a visually hidden "(nouvel onglet)" notice.
- **Color is never the only signal:** the current page uses an underline as well as red; status uses text as well as the green dot.

## Content to confirm with the client
- "13 ans" and "Leader" come from the client's existing copy. Update the year count each year, or rephrase as "depuis 20XX" once the founding year is known.
- An email address or a form service is needed before adding a contact form. There is none on purpose, so the site doesn't collect messages nobody receives.
- Real photos go in `public/assets/img/` (names listed in `public/assets/img/LISEZ-MOI.txt`). The real logo can replace the CSS logo (`.logo__box`).
