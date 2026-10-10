# No Good Racing

Bike racing, without the bullsh\*t. Jekyll site hosted on GitHub Pages at https://nogoodracing.cc.

## Local development

Requires Ruby 3.4.7 (see `.ruby-version`; e.g. via rbenv) and a UTF-8 locale (the Sass compiler fails on an ASCII-only shell — `export LANG=en_GB.UTF-8`).

```sh
bundle config set --local path vendor/bundle
bundle install
bundle exec jekyll serve --livereload
```

Then open http://127.0.0.1:4000. Restart the server after editing `_config.yml`.

The `github-pages` gem pins Jekyll and plugins to the versions GitHub Pages uses, so local builds match production.

## Adding a rider to the Team page

The Team page is built from `_data/riders.yml` — one entry per rider, no HTML to touch.

### 1. Get their OK

Only add riders who've agreed to be on the site. Use their name as they want it shown, and a town, city or region for where they race — never an address.

### 2. Add a photo (optional)

1. Crop it to a **square** (Preview or Photos is fine).
2. Resize to ~800px and compress (aim for under 200KB):

   ```sh
   sips -Z 800 -s formatOptions 70 first-last.jpg
   ```

3. Save it in `assets/images/riders/` with a **lowercase, hyphenated** name, e.g. `first-last.jpg`. GitHub Pages is case-sensitive and macOS isn't — a mismatch like `First-Last.JPG` works locally but breaks once live.

No photo yet? Leave `photo:` blank and the card shows the rider's initials.

### 3. Add the entry to `_data/riders.yml`

```yaml
- name: First Last
  photo: /assets/images/riders/first-last.jpg
  racing_in: Shrewsbury
```

- Indent with **two spaces**, no tabs — copy an existing entry's layout.
- Order in the file = order on the page.
- Delete the placeholder "Rider Name" entries once real riders are in.
- If a name contains a colon or starts with a symbol, wrap it in quotes: `name: "First Last"`.

### 4. Preview, then publish

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

Delete their entry and their photo file, then commit and push. The photo still exists in git history; if someone needs it gone completely, that needs a history rewrite — ask before doing it.

## Structure

- `_config.yml` — site settings, header/footer navigation (`nav`), logo slot
- `_layouts/` — `default` (shell), `home` (hero + cards), `page` (title band + content)
- `_includes/` — site header and footer
- `_sass/` + `assets/css/main.scss` — the theme (tokens, base, layout, components)
- `about.md`, `rides.md`, `team.md`, `join.md`, `sponsors.md` — section pages
- `_data/riders.yml` + `assets/images/riders/` — Team page riders (see "Adding a rider" above)
- `CLAUDE.md` — voice, visual and accessibility guidelines for the site
