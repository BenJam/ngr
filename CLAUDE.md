# No Good Racing — site guide

Jekyll site hosted on GitHub Pages (custom domain in `CNAME`). The theme lives in this repo (`_layouts/`, `_includes/`, `_sass/`) — no remote theme, no plugins beyond what GitHub Pages supports natively. Keep it that way: no custom Ruby plugins, no build step outside `jekyll build`.

Local dev: `bundle exec jekyll serve --livereload` → http://127.0.0.1:4000 (restart after editing `_config.yml`).

## Voice

Straightforward, a bit punky, always welcoming. "Serious, but fun."

- UK English (colour, endeavour, prioritise). Short sentences. Plain words. Talk to "you".
- Humour is self-deprecating or aimed at results-obsessed racing culture — never at people. "(bad) results board" good; mocking slow riders never.
- No gatekeeping. Never ask about FTP, training volume, kit or bike cost. Acknowledge life happens off the bike.
- Any discipline counts: TT, road, track, gravel, CX, XC, enduro, DH.
- Swearing tops out at acronyms: "BS", "AF".
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
| `join@nogoodracing.cc` | Members recommending new members | Team |
| `chapter@nogoodracing.cc` | Starting a new chapter / ride | Rides |
| `sponsors@nogoodracing.cc` | Sponsorship enquiries | Sponsors |

Anything else — personal names, personal links, other email addresses, exact meeting venues/times — stays off the site until the team explicitly approves that specific item. Use a visible `.placeholder` block in the meantime, and remove it once the real content goes in.

## Visual system

Tokens in `_sass/_tokens.scss`.

- Colours: ink `#0B0B0B`, paper `#FFFFFF`, off-white `#F4F1EA`, red `#D7000F` (text/buttons on light), bright red `#FF3B3B` (text on ink only). Red `#D7000F` on ink is 3.65:1 — large display text and decoration only.
- Type: Anton (display, caps) + Atkinson Hyperlegible (body, ≥1.125rem).
- Look: big condensed headings, thick red racing stripes, slight diagonal edges, hard offset shadows (no blur), sticker-style tags. No faux-Japanese styling; the real logo comes from the team's designer (set `logo:` in `_config.yml` when ready).
- Components: `.button`, `.card-grid` + `.card`, `.tag-list` + `.tag`, `.section--alt`, `.placeholder`.

## Accessibility (non-negotiable)

WCAG AA contrast (4.5:1 body, 3:1 large text/UI), visible `:focus-visible` on everything interactive, skip link, landmarks, `aria-current` on nav, underlined links in body text, ≥44px tap targets, respect `prefers-reduced-motion`, never rely on colour alone.
