# No Good Racing

Bike racing, without the bullsh\*t. Jekyll site hosted on GitHub Pages at https://nogoodracing.cc.

## Local development

Requires Ruby 3.4.7 (see `.ruby-version`; e.g. via rbenv), ImageMagick for rider photos (`brew install imagemagick`), and a UTF-8 locale (the Sass compiler fails on an ASCII-only shell — `export LANG=en_GB.UTF-8`).

```sh
bundle config set --local path vendor/bundle
bundle install
bundle exec jekyll serve --livereload
```

Then open http://127.0.0.1:4000. Restart the server after editing `_config.yml`.

The `github-pages` gem pins Jekyll and plugins to the versions GitHub Pages uses, so local builds match production.

## Adding a rider to the Team page

The Team page is built from `_data/riders.yml` — one entry per rider, no HTML to touch. Cards show a black & white photo that fades to colour on hover, or the rider's initials if there's no photo.

**First, get their OK.** Only add riders who've agreed to be on the site. Use their name as they want it shown, and a town, city or region for where they race — never an address.

### Quick way: `scripts/add-rider`

Needs ImageMagick once: `brew install imagemagick`.

```sh
scripts/add-rider
```

Drag the photo into the terminal when asked (any JPG, PNG or iPhone HEIC — it doesn't need cropping), then answer the questions. The script:

- crops it to a centred square, 800px, under 200KB
- saves `assets/images/riders/first-last.jpg` (colour) and `first-last-bw.jpg` (black & white)
- strips all metadata — phone photos carry GPS location, and it never reaches the site
- adds the rider to the end of `_data/riders.yml`, and offers to remove the "Rider Name" placeholders

Not happy with the crop? Crop the photo square yourself first and run it again — it'll offer to overwrite. Then preview and publish (below).

### Manual way

#### 1. Photos (optional)

1. Crop the photo to a **square** with the face roughly centred.
2. Make the colour and black & white versions, with metadata stripped:

   ```sh
   magick photo.jpg -auto-orient -strip -resize 800x800^ -gravity center -extent 800x800 -quality 72 first-last.jpg
   magick first-last.jpg -colorspace Gray -quality 72 first-last-bw.jpg
   ```

3. Save both in `assets/images/riders/` with **lowercase, hyphenated** names. GitHub Pages is case-sensitive and macOS isn't — a mismatch like `First-Last.JPG` works locally but breaks once live.

No photo yet? Leave `photo:` and `photo_bw:` out and the card shows the rider's initials. With only `photo:`, the card shows colour all the time.

#### 2. Add the entry to `_data/riders.yml`

```yaml
- name: "First Last"
  photo: /assets/images/riders/first-last.jpg
  photo_bw: /assets/images/riders/first-last-bw.jpg
  racing_in: "Shrewsbury"
```

- Indent with **two spaces**, no tabs — copy an existing entry's layout.
- Order in the file = order on the page.
- Delete the placeholder "Rider Name" entries once real riders are in.
- Keep names and places in double quotes so colons and apostrophes are safe.

### Preview, then publish

```sh
export LANG=en_GB.UTF-8
bundle exec jekyll serve --livereload
```

Check http://127.0.0.1:4000/team/ — changes to `riders.yml` reload without restarting. If the build fails, the terminal shows a YAML error with a line number (usually indentation).

When it looks right:

```sh
git add _data/riders.yml assets/images/riders/
git commit -m "Add rider: First Last"
git push
```

### Removing a rider

Delete their entry and both photo files, then commit and push. The photo still exists in git history; if someone needs it gone completely, that needs a history rewrite — ask before doing it.

## Structure

- `_config.yml` — site settings, header/footer navigation (`nav`), logo slot
- `_layouts/` — `default` (shell), `home` (hero + cards), `page` (title band + content)
- `_includes/` — site header and footer
- `_sass/` + `assets/css/main.scss` — the theme (tokens, base, layout, components)
- `about.md`, `rides.md`, `team.md`, `join.md`, `sponsors.md` — section pages
- `_data/riders.yml` + `assets/images/riders/` — Team page riders (see "Adding a rider" above)
- `scripts/add-rider` — adds a rider: photos + `riders.yml` entry
- `CLAUDE.md` — voice, visual and accessibility guidelines for the site
