# Agent notes for yubin_website

This is Yubin Kim's personal site, built from the al-folio v1.x starter. It is not the upstream al-folio repo, so the upstream contributor rules don't apply here.

- Content lives in `_pages/`, `_bibliography/papers.bib`, `_news/`, `_data/`, and `_config.yml`. See `README.md` for the full map.
- Layouts, includes and styles come from the `al_folio_core` gem and related `al_*` gems. Prefer config and content changes over local overrides.
- Local overrides of gem files: `_includes/metadata.liquid` (home `<title>` from `site.home_title`), `_layouts/about.liquid` (profile column with social icons), `assets/css/main.scss` (adds `@use "custom"`). Site CSS goes in `_sass/_custom.scss`.
- `baseurl` is empty and `url` is https://yubinkim04.github.io. Build with `bundle exec jekyll build` using Homebrew `ruby@3.3` (see README).
- Keep the search-disambiguation signals ("MIT Operations Research Center", "Giannis Daras") in the title, description and JSON-LD.
- Upstream docs for features and plugins are in `docs/`.
