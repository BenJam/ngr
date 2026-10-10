# No Good Racing

Bike racing, without the BS. Jekyll site hosted on GitHub Pages at https://nogoodracing.cc.

## Local development

Requires Ruby 3.4.7 (see `.ruby-version`; e.g. via rbenv) and a UTF-8 locale (the Sass compiler fails on an ASCII-only shell — `export LANG=en_GB.UTF-8`).

```sh
bundle config set --local path vendor/bundle
bundle install
bundle exec jekyll serve --livereload
```

Then open http://127.0.0.1:4000. Restart the server after editing `_config.yml`.

The `github-pages` gem pins Jekyll and plugins to the versions GitHub Pages uses, so local builds match production.

## Structure

- `_config.yml` — site settings, header/footer navigation (`nav`), logo slot
- `_layouts/` — `default` (shell), `home` (hero + cards), `page` (title band + content)
- `_includes/` — site header and footer
- `_sass/` + `assets/css/main.scss` — the theme (tokens, base, layout, components)
- `about.md`, `rides.md`, `team.md`, `sponsors.md` — section pages
- `CLAUDE.md` — voice, visual and accessibility guidelines for the site
