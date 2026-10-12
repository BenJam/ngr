# No Good Racing — site guide

Jekyll site for No Good Racing, hosted on GitHub Pages at https://nogoodracing.cc (repo `BenJam/ngr`, deployed from `main` / root; custom domain in `CNAME`). The theme lives in this repo (`_layouts/`, `_includes/`, `_sass/`) — no remote theme, no plugins beyond what GitHub Pages supports natively. Keep it that way: no custom Ruby plugins, no build step outside `jekyll build`.

## Starting a session

1. `git status` and `git log --oneline -10` — the user edits and commits copy themselves between sessions; take their changes as the current state.
2. Start the dev server (background, it stops after ~2h in Claude Code):
   ```sh
   export LANG=en_GB.UTF-8 && eval "$(rbenv init - zsh)"; bundle exec jekyll serve --livereload
   ```
   → http://127.0.0.1:4000. Livereload covers pages, Sass and `_data/`; **restart after editing `_config.yml`**.

## How we work

- Plan first for anything non-trivial; the user approves before edits.
- Commit locally with clear messages; **don't push** — this environment has no GitHub credentials and the user won't grant them. The user reviews locally and pushes from their own terminal.
- When the user has staged edits of their own, commit only your paths (`git commit -- <paths>`).
- Verify with a build (`bundle exec jekyll build`, or `-d <scratch dir>` to leave `_site` alone) and `curl` against the dev server. Headless Chrome crashes in this sandbox, so no screenshots — say so and ask the user to check visually (desktop + ~400px).
- Test risky changes in a scratch copy, not on real data (e.g. rider entries/photos).
- Homebrew belongs to another macOS user: you can't `brew install`; ask the user to run it.

## Gotchas

- `LANG` must be UTF-8 or the Sass compiler (Ruby Sass via `github-pages`) fails on any non-ASCII character. Keep `.scss` files ASCII anyway.
- `theme: null` in `_config.yml` is deliberate — without it `github-pages` silently applies the Primer theme.
- Sass is the old Ruby Sass: `@import`, no `@use`, no Sass `min()/max()`. Plain CSS `clamp()` passes through fine.
- macOS tools are BSD: `sed -i ''`, no GNU `0,/re/` ranges, `stat -f%z`. macOS `iconv` exits 1 after transliterating.
- GitHub Pages is case-sensitive; macOS isn't. Lowercase filenames.
- System Ruby is 2.6 — always go through rbenv (3.4.7, `.ruby-version`).

## Site map

| Path | Source | Notes |
|---|---|---|
| `/` | `index.md` (`layout: home`) | Hero (title, tagline, CTA from front matter) + cards from `cards:` front matter. 5 cards: one row ≥75em, 3+2 from 56em, stacked below. |
| `/about/` | `about.md` | Manifesto copy, disciplines as `.tag-list`, "What's with the name?" |
| `/rides/` | `rides.md` | Chapter cards; only Shrewsbury running, rest TBC |
| `/team/` | `team.md` + `_data/riders.yml` | Rider roster (see Riders below) |
| `/join/` | `join.md` | Invitation-only joining, club status, benefits & costs |
| `/sponsors/` | `sponsors.md` | No sponsors yet |

Nav (header + footer) comes from `nav:` in `_config.yml`. Section pages use `layout: page` with `title` + optional `intro` front matter. Tooling: `scripts/add-rider`. Team-facing docs: `README.md`.

## Voice

Straightforward, a bit punky, always welcoming. "Serious, but fun."

- UK English (colour, endeavour, prioritise). Short sentences. Plain words. Talk to "you".
- Humour is self-deprecating or aimed at results-obsessed racing culture — never at people. "(bad) results board" good; mocking slow riders never.
- No gatekeeping. Never ask about FTP, training volume, kit or bike cost. Acknowledge life happens off the bike.
- Any discipline counts: TT, road, track, gravel, CX, XC, enduro, DH.
- Swearing tops out at acronyms ("BS", "AF") or asterisked words — the tagline is "Bike racing, without the bullsh*t" (the team's call). Nothing stronger, nothing uncensored.
- Sentence case for prose; ALL CAPS only via the display font on headings (write headings in normal case in Markdown — CSS uppercases them).
- Reusable lines: "No pressure, just good times." · "Serious, but fun." · "Because life happens off the bike too."

| Do | Don't |
|---|---|
| "Race whatever you like. It's all the same to us." | "Open to riders of all abilities and disciplines." |
| "Didn't train this week? Neither did we." | "Members are expected to maintain fitness." |
| "Come for the racing, stay for the post-ride chips." | "Elite performance, community values." |

## Facts (from the team manifesto)

- Informal, welcoming cycle racing "team" for people fitting racing around other commitments.
- Joining is by recommendation from an existing member.
- Not BC-affiliated; members stay in their own clubs and can list NGR as a sponsor.
- Members get a sticker pack and the team chat; dues £5/year.
- Tuesday-night training rides run by local chapters.
- The name and look are an homage to the 1985 Osaka street-racing team No Good Racing. No association with them, and no endorsement of street racing.

## Privacy

Approved for publication — team role addresses, always as `mailto:` links (`[x@nogoodracing.cc](mailto:x@nogoodracing.cc)`):

| Address | Use | Page |
|---|---|---|
| `join@nogoodracing.cc` | Members recommending new members | Join |
| `chapter@nogoodracing.cc` | Starting a new chapter / ride | Rides |
| `sponsors@nogoodracing.cc` | Sponsorship enquiries | Sponsors |

Anything else — personal names, personal links, other email addresses, exact meeting venues/times — stays off the site until the team explicitly approves that specific item. Use a visible `.placeholder` block in the meantime, and remove it once the real content goes in.

### Riders (Team page)

Rider names and photos are personal data. Rider profiles are the exception to the rule above: the team adds them, and each one is consent-based.

- Data: `_data/riders.yml` (`name`, `photo`, `photo_bw`, `racing_in`), rendered by `_includes/rider-card.html`. Order in the file = order on the page.
- Add riders with `scripts/add-rider` (ImageMagick): crops, makes colour + B&W, strips metadata, asks for consent, appends the YAML entry. Prefer it to hand-editing.
- Only add riders who've agreed to be on the site; remove anyone who asks, both photo files included.
- `racing_in` is a town/city or region — never an address.
- Photos: `assets/images/riders/`, square JPG, 800px, under 200KB, `first-last.jpg` + `first-last-bw.jpg`. **Every photo must have its metadata stripped** (`magick … -strip`) — phone photos carry GPS location.
- Display: with `photo_bw`, the card shows B&W and fades to colour on hover (instant under reduced motion); `photo` only → colour; no photo → initials block.
- Don't invent riders or copy names from elsewhere; placeholders stay as "Rider Name".

## Visual system

Tokens in `_sass/_tokens.scss`.

- Colours: ink `#0B0B0B`, paper `#FFFFFF`, off-white `#F4F1EA`, bright red `#FF3B3B` (text on ink only), and two reds in the logo's hue:
  - `$red` `#EB3324` — the logo red. Fills, stripes, borders, decoration, and **large text only** (WCAG large = ≥24px, or ≥18.66px bold): 4.17:1 on paper, 4.72:1 on ink. Used for the hero tagline strip and the home hero button (label kept at 1.5rem so it stays "large").
  - `$red-text` `#CF2D20` — logo hue, 12% darker. **Anything with small text**: links, buttons, tags, skip link, hover states. 5.19:1 on paper, 4.6:1 on off-white.
  - Rule of thumb: if red touches normal-size text (as the text colour or behind white text), use `$red-text`.
- Type: Anton (display, caps) + Atkinson Hyperlegible (body, ≥1.125rem).
- Look: big condensed headings, thick red racing stripes, slight diagonal edges, hard offset shadows (no blur), sticker-style tags. No faux-Japanese styling; the logo is from the team's designer (`assets/images/logo-simple.svg`, set via `logo:` in `_config.yml`; header height `2.75rem`/44px).
- Components: `.button`, `.card-grid` + `.card`, `.tag-list` + `.tag`, `.section--alt`, `.placeholder`, `.rider-grid` + `.rider` + `.rider__media` (via `_includes/rider-card.html`).

## Accessibility (non-negotiable)

WCAG AA contrast (4.5:1 body, 3:1 large text/UI), visible `:focus-visible` on everything interactive, skip link, landmarks, `aria-current` on nav, underlined links in body text, ≥44px tap targets, respect `prefers-reduced-motion`, never rely on colour alone.

## Status and open items (as of 2026-10-12)

Live and pushed. Logo, favicon and web app icons are in (from the team's designer, 2026-10-12). Waiting on the team for:

- **Riders** — `_data/riders.yml` still has three "Rider Name" placeholders; `scripts/add-rider` offers to remove them on first real add. B&W→colour hover not yet eyeballed with a real photo.
- **Rides** — chapters other than Shrewsbury are TBC; meeting venues/times not published (privacy rule above).
- **Sponsors** — none yet.
- **French version** — discussed and parked; English only for now. If revived: per-language pages under `/fr/`, UI strings + nav in `_data/i18n.yml`, `ref:` front matter to pair translations, header language switcher, `hreflang` alternates (no i18n plugins — GitHub Pages doesn't allow them). French copy needs a native-speaker review.
