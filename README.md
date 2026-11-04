# capture-fixture

**Maple Grove School Division (test fixture)** is a small, synthetic static website. Yoodel Capture, our website archiving and migration tool, crawls it to prove specific behaviours. Every school, person, address, phone number and event on the site is invented. It is not a real school.

Covered Yoodel Capture tickets: #212, #217, #222, #223, #224, #230, #231.

- Live site: https://yoodel26.github.io/capture-fixture/
- Served by GitHub Pages from the root of `main`. The site is plain HTML/CSS with no build step and no JavaScript. `.nojekyll` makes Pages serve the files exactly as committed.
- Every page has `<meta name="robots" content="noindex">` and a visible "Test fixture for Yoodel Capture" banner.
- The site is a GitHub Pages project site served under `/capture-fixture/`, so every link and asset path is relative (none start with `/`).

## Pages and their purpose

| Path | Purpose |
| --- | --- |
| `index.html` | Home page. Links to every page. Has one working image (`images/logo.svg`). |
| `about.html` | Ordinary content page with one working image (`images/about-campus.png`). |
| `programs.html` | Ordinary content page with one working image (`images/programs-classroom.svg`). |
| `contact.html` | Ordinary content page with one working image (`images/contact-map.png`). |
| `news.html` | News listing that links to the two articles below. |
| `news/welcome-back.html` | News article in a subfolder, with a date line and an image (`../images/news-welcome-back.png`). |
| `news/science-fair.html` | News article in a subfolder, with a date line and an image (`../images/news-science-fair.svg`). |
| `broken.html` | Has real defects to diagnose: a missing `<img src="images/missing-photo.jpg">` (404) and an inline `background-image:url(https://www&example.com/images/zippy.gif)` whose host is malformed on purpose. |
| `shared-problem-a.html`, `shared-problem-b.html`, `shared-problem-c.html` | Three ordinary pages that each include the same missing `<img src="images/shared-banner.png">` (404), so one shared defect affects several pages. |
| `video.html` | Embeds a YouTube (`youtube-nocookie.com`) iframe and a Vimeo iframe. |
| `styles/site.css` | Stylesheet shared by every page. Sets the background to `url(../images/paper.png)`. References a missing decorative image `url(fancybox/fancybox_loading.gif)`, which resolves to `styles/fancybox/fancybox_loading.gif` and returns 404. Declares an `@font-face` with `icons.eot?v1`, `icons.eot?v1#iefix` and `icons.woff?v1` sources. |
| `fonts/icons.eot`, `fonts/icons.woff` | A real, tiny generated icon font ("FixtureIcons", CC0) in EOT and WOFF formats. |
| `images/` | Small generated PNG and SVG images. |
| `robots.txt` | Allows everything and points to the sitemap. |
| `sitemap.xml` | Lists every HTML page by absolute URL. |

### Expected HTTP results (v1)

- **200:** every HTML page above, `styles/site.css`, every file in `images/` and `fonts/`, `robots.txt`, and `sitemap.xml`.
- **404:** `images/missing-photo.jpg`, `images/shared-banner.png`, and `styles/fancybox/fancybox_loading.gif`.
- **No request possible:** `https://www&example.com/images/zippy.gif`, because its host name is malformed.

Note: `robots.txt` is served at `/capture-fixture/robots.txt`. The host root (`https://yoodel-inc.github.io/robots.txt`) is outside this repository, so crawlers that only check the host root will not find this file.

## Planned mutations (v2)

**Version 2 applied 4 Nov 2026:** added news/new-library.html (linked from news.html), edited about.html, deleted contact.html, repaired images/missing-photo.jpg and images/shared-banner.png. **Version 3 applied 4 Nov 2026:** styles/site.css now uses images/paper-v2.png and no longer references the missing FancyBox image (a shared stylesheet change for every page).

**Version 4 applied 4 Nov 2026:** only images/shared-banner.png changed (used by shared-problem-a/b/c.html).

These changes are documented here but **not applied yet**. A later commit will make them, so that Capture can be tested on detecting changes between crawls:

1. **Add** `news/new-library.html`, a new news article.
2. **Edit** the text of `about.html`.
3. **Delete** `contact.html`.
4. **Change** the background in `styles/site.css` from `../images/paper.png` to `../images/paper-v2.png`. The commit must also add the new `images/paper-v2.png`.
5. **Repair** `images/missing-photo.jpg` and `images/shared-banner.png` by adding the missing files, so that those 404s become 200s.

When v2 is applied, also update `sitemap.xml`, the navigation on every page, and the home page list so that they match the new set of pages.
