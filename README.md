# Team website

Jekyll site for the racing team, hosted on GitHub Pages (theme: Cayman).

## Local development

Requires Ruby 3.4.7 (see `.ruby-version`; e.g. via rbenv).

```sh
bundle config set --local path vendor/bundle
bundle install
bundle exec jekyll serve --livereload
```

Then open http://127.0.0.1:4000.

The `github-pages` gem pins Jekyll and plugins to the versions GitHub Pages uses, so local builds match production.

## Structure

- `_config.yml` — site settings and header navigation (`nav`)
- `_layouts/default.html` — Cayman layout override that renders the nav buttons
- `about.md`, `rides.md`, `team.md`, `sponsors.md` — section pages
