# yubin_website

Personal academic website for Yubin Kim (MIT Operations Research Center), built on [al-folio](https://github.com/alshedivat/al-folio) v1.x. The upstream al-folio docs are kept in `docs/`.

## Where things live

| What | File |
|---|---|
| Bio, photo, sidebar text | `_pages/about.md` |
| Publications (drives the home page and `/publications/`) | `_bibliography/papers.bib`. Add `selected = {true}` to feature a paper on the home page. |
| News items | `_news/*.md` (one file per item) |
| Social icons | `_data/socials.yml` |
| Venue badge colors | `_data/venues.yml` |
| CV page (hidden until filled in) | `_data/cv.yml`, `_pages/cv.md` |
| Blog (hidden until first post) | `_posts/YYYY-MM-DD-title.md`, `_pages/blog.md` |
| Site name, domain, search metadata | `_config.yml` |

**Local overrides** of `al_folio_core` files. After upgrading the gem, compare each with `bundle exec al-folio upgrade overrides diff <path>`:

- `_includes/metadata.liquid`: the home page `<title>` uses `home_title` from `_config.yml`.
- `_layouts/about.liquid`: the profile column (photo, then social icons, then affiliation) sits beside the name, and the bottom social block is removed.
- `assets/css/main.scss`: identical to the gem's, plus `@use "custom";` at the end, which loads `_sass/_custom.scss`. That file hides the "ctrl k / ⌘ k" search hint, styles the icon row under the photo, and fixes the order on phones.

## Preview locally

This needs Homebrew `ruby@3.3`, the same version the deploy workflow uses. Ruby 4.x can't build the `eventmachine` dependency.

```sh
export PATH=/opt/homebrew/opt/ruby@3.3/bin:$PATH
# First time only. The CPLUS_INCLUDE_PATH works around missing C++ headers in macOS Command Line Tools.
SDK=$(xcrun --show-sdk-path) SDKROOT=$SDK CPLUS_INCLUDE_PATH=$SDK/usr/include/c++/v1 bundle install
bundle exec jekyll serve   # http://localhost:4000
```

Warnings like "convert: command not found" come from ImageMagick, which only resizes images and is optional locally (`brew install imagemagick`). The deploy workflow installs it.

## Before publishing

1. **Photo:** `assets/img/pfp.jpg`. If you replace it under a different name, update `image:` in `_pages/about.md`, the `image` URL in the JSON-LD block at the bottom of that file, and `og_image` in `_config.yml`.
2. **Bio:** fill in the TODO in `_pages/about.md` (undergrad / previous institution).
3. **Socials:** uncomment and fill in email, GitHub, ORCID, OpenReview, LinkedIn and X in `_data/socials.yml`. Add the same URLs to `"sameAs"` in the JSON-LD block in `_pages/about.md`.
4. **News dates:** the ICML acceptance and PhD start dates are approximate (May 1 and Sep 1). Correct them in the `_news/` file names and front matter.
5. **CV (optional):** finish `_data/cv.yml`, then set `nav: true` and delete `sitemap: false` in `_pages/cv.md`.

## Deploy (GitHub Pages)

1. Create an empty public GitHub repo named `<username>.github.io` and push this folder to its `main` branch.
2. Go to Settings → Actions → General → Workflow permissions and select **Read and write permissions**.
3. Pushing to `main` runs `.github/workflows/deploy.yml`, which builds the site into the `gh-pages` branch.
4. Go to Settings → Pages, choose **Deploy from a branch**, and select `gh-pages` / `(root)`.

## Custom domain

The site is live at `https://yubinkim04.github.io` (`url` in `_config.yml`). If you buy a domain later (`yubinkim.com` is taken; `yubinkim.ai`, `yubin-kim.com` and `yubinkim.net` were free on 2026-09-30):

- Buy the domain, then add a `CNAME` file at the repo root containing just the domain name.
- Point DNS at GitHub Pages ([guide](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)) and enable **Enforce HTTPS**.
- Once it resolves, update `url`, `og_image`, and the URLs in the JSON-LD block in `_pages/about.md` to the new domain.

## Search ranking (the other Yubin Kim at MIT)

The other Yubin Kim is a Media Lab PhD candidate who also works on LLM agents. This site sets you apart through **"Operations Research Center"**, **"Giannis Daras"**, and your papers. These appear in the home page title, the meta description, the keywords, and the structured data (`ProfilePage` / `Person` with `affiliation`).

- Ask Prof. Daras to link your name in the lab section of giannisdaras.com to this site. That link matters most.
- Ask the ORC to list you, with a link, in its student directory.
- Add the site URL to Google Scholar (profile → Homepage), GitHub, OpenReview, X and LinkedIn.
- In [Google Search Console](https://search.google.com/search-console), verify the site. Put the verification ID in `google_site_verification` and set `enable_google_verification: true` in `_config.yml`. Then submit `sitemap.xml` and request indexing for the home page.
- Keep `_news/` current. Search engines favor pages that are updated regularly.
