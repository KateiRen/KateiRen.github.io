# How to maintain this blog

## Rendering technology

This checkout uses **Jekyll** with the **Mediumish theme**:

- **Markdown** is converted to HTML by **Kramdown**.
- **Liquid templates** provide layouts and reusable components.
- **Bootstrap and jQuery** provide styling and browser interactions.
- **GitHub Pages** hosts the generated static site. There is no database or CMS/admin interface.

See [README.md](README.md) and [_config.yml](_config.yml).

## Create a post

Create a Markdown file in [_posts](_posts), named `YYYY-MM-DD-my-new-post.md`. Use lowercase, hyphen-separated filenames without spaces for new posts.

```markdown
---
layout: post
title: "My new project"
author: karsten
lang: de
translation_key: my-new-project
permalink: /mein-neues-projekt/
categories: [3D-Printing, motorbike]
tags: [ESP32, USB-C]
image: assets/images/2026-11-26/cover.jpg
featured: false
toc: true
---

## Introduction

Write your post here using Markdown.
```

- `image` supplies the cover image. Omit it if unnecessary.
- `featured: true` puts the post in the featured section.
- `toc: true` generates a table of contents from headings.
- Categories and tags automatically group posts; no separate topic registration is needed.
- Set `lang: de` for German or `lang: en` for English. Use a root-level German permalink and an `/en/` permalink for its English equivalent.
- The filename supplies the publication date. Future-dated posts are normally omitted until that date.

The [existing V3 post](_posts/2024-03-23-tomtom-rider-adapter%20V3.md) is a useful template.

For unpublished drafts, Jekyll's standard directory is `_drafts`, with filenames such as `my-new-post.md`. Preview them with `bundle exec jekyll serve --drafts`. The existing `_drafts_` directory is not Jekyll's standard drafts directory.

## Upload media

Copy images into [assets/images](assets/images), preferably in a dated subfolder. Commit them alongside the post.

Embed an image in the post body:

```markdown
![Description of the photo](/assets/images/2026-11-26/photo.jpg)
```

An optional width is supported and already used by existing posts:

```markdown
![Description](/assets/images/2026-11-26/photo.jpg){: width="500" }
```

For the cover image in front matter, use the path without a leading slash:

```yaml
image: assets/images/2026-11-26/cover.jpg
```

Other files work as ordinary download links. For example, create `assets/downloads` and place the model there:

```markdown
[Download the model](/assets/downloads/my-model.stl)
```

On GitHub itself, navigate to the destination folder and use **Add file > Upload files** instead of copying files locally.

## Cross-link posts and topics

### Another post

Prefer Jekyll's `post_url` tag. Use the post's filename **without `.md`**, including its date:

```markdown
[Original adapter]({% post_url 2023-03-03-tomtom-rider-adapter %})
```

If an existing filename contains spaces, quote it:

```markdown
[Adapter V3]({% post_url "2024-03-23-tomtom-rider-adapter V3" %})
```

The fallback permalink configuration is `/:title/`, but bilingual posts use explicit permalinks: German at the root and English under `/en/`. Using `post_url` avoids having to guess the generated URL and follows a post when its permalink changes.

### Categories and tags

```markdown
[More 3D-printing posts](/categories#3D-Printing)
[More ESP32 posts](/tags#ESP32)
```

Match capitalization exactly. Spaces in category or tag names become hyphens in these anchors. Assign the same category or tag to related posts to group them together.

### A heading in the same post

```markdown
[Jump to assembly](#putting-it-together)
```

This targets a heading such as `## Putting it together`. Duplicate headings may receive numbered anchors; check the generated page if unsure.

## German posts with English equivalents

This blog uses **two Markdown posts per article**, connected through shared metadata, with German URLs at the root and English equivalents under `/en/`.

**Implemented:** the seven personal articles have German and English versions, article language switches, language-aware HTML, and reciprocal SEO alternate links. The 14 theme/demo posts have been removed. The homepage, topics, search, feeds, and previous/next navigation remain mixed-language; the site interface has not been fully localized.

### Post files and URLs

Keep both versions in [_posts](_posts), using distinct filenames:

- `2026-11-26-mein-projekt.md`, published at `/mein-projekt/`.
- `2026-11-26-my-project.md`, published at `/en/my-project/`.

German version:

```yaml
---
layout: post
title: "Mein Projekt"
author: karsten
lang: de
translation_key: my-project
permalink: /mein-projekt/
categories: [3D-Printing]
image: assets/images/2026-11-26/cover.jpg
---
```

English version:

```yaml
---
layout: post
title: "My project"
author: karsten
lang: en
translation_key: my-project
permalink: /en/my-project/
categories: [3D-Printing]
image: assets/images/2026-11-26/cover.jpg
---
```

Write the corresponding language's Markdown content below each front matter block.

- `lang` identifies the content language.
- `translation_key` is a custom field, shared by translations of the same article. Use a different key for each article and at most one published post per language for each key.
- Explicit `permalink` values prevent URL collisions and allow translated slugs.
- Both versions can share images and downloads; there is no need to duplicate media.
- An article can remain German-only. Publish an English equivalent later if desired.

The article layout automatically shows the available sibling language when published posts share a `translation_key`. You do not need to add a language switch manually. For ordinary cross-links inside the article body, use `post_url` and choose the sibling in the same language:

```markdown
[Read in English]({% post_url 2026-11-26-my-project %})
```

```markdown
[Auf Deutsch lesen]({% post_url 2026-11-26-mein-projekt %})
```

### Implemented language support

- **Article language switch:** [_layouts/post.html](_layouts/post.html) finds published posts with the same `translation_key` and another `lang`. It shows "English" or "Deutsch" when a translation exists.
- **Document language:** [_layouts/default.html](_layouts/default.html) uses the page's `lang`, then an optional site `lang`, then `en`. The English fallback preserves the language of existing non-post pages and interface text. Always set `lang` explicitly on new posts.
- **SEO:** the default layout adds reciprocal `hreflang` links, including each article's own language. Each translation retains its own canonical URL. The site's `url` is set in [_config.yml](_config.yml) so production SEO links are absolute.
- **Legacy redirects:** English originals now live under `/en/`. Their `redirect_from` fields preserve the exact old URLs, including capitalization, through `jekyll-redirect-from`. On GitHub Pages these are generated HTML redirects, not HTTP 301 responses.
- **Shared media:** translations reference the same asset files.
- **Root hosting:** `baseurl` is an empty string in [_config.yml](_config.yml). Do not set it to `/`: the theme's URL concatenation would produce `//assets/...` and `//en/...`, which browsers interpret as external hosts rather than local paths.

Keep the `redirect_from` fields on migrated English posts. For any future URL move, add the previous URL to `redirect_from`, and ensure it does not collide with another post's permalink.

The [Gemfile](Gemfile) includes `jekyll-redirect-from` and the `kramdown-parser-gfm` parser required by the existing Markdown configuration. Run `bundle install` after pulling dependency changes. [HowTo.md](HowTo.md) is excluded from the generated site so its example Liquid tags are not evaluated.

### Remaining multilingual considerations

1. **Navigation and listings:** for separate browsing, add a German homepage at `/` and an English overview at `/en/`, with translated interface labels and language-filtered featured and recent posts. English article URLs do not themselves create an `/en/` overview.
2. **Categories and tags:** decide whether topic listings should filter by language or show language labels. Use consistent topic identifiers across translations; translated display labels can be handled separately.
3. **Search, feeds, and previous/next links:** decide whether these should stay mixed-language or become language-specific. Currently, they do not distinguish article languages.

The current homepage uses `jekyll-paginate`. **Do not filter translations out after pagination:** this produces short or empty pages and incorrect pagination counts. Separate language listings need filtering before pagination, or can initially be unpaginated.

### Choose the browsing experience

The current starting point is:

- One mixed-language homepage; language labels on post cards could be added later.
- Article-level translation switches.
- Correct document language and translation SEO links.
- Existing pagination unchanged.

For a fully bilingual experience, add separate language listings, localized navigation, and appropriate language handling for topics, search, feeds, and previous/next links. A multilingual plugin is not essential for two languages. If one is chosen, verify that the publishing build supports it before relying on it.

**Preserve incoming links when changing URLs.** The existing English posts were moved under `/en/` with redirects from their previous root-level URLs. New German posts use distinct root-level slugs, avoiding those reserved redirect URLs.

These considerations describe the local Mediumish checkout. Check the actual publishing branch and its theme before implementing template changes.

## Preview locally

With Ruby, Bundler, and the project's dependencies available, run from the repository root:

```powershell
bundle exec jekyll serve
```

Open **http://localhost:4000**.

If dependencies are missing, install them with `bundle install`, then retry. Stop the preview server with **Ctrl+C**.

Check both languages, language switches, images, downloads, and cross-links before publishing. For migrated articles, also open the old URL and confirm that it redirects to the English version.

## Publish

1. Check the repository's **Settings > Pages > Build and deployment** for the publishing branch and directory.
2. Commit the post and all referenced media.
3. Push the changes to the configured publishing branch.
4. Wait for GitHub Pages to rebuild, then check the live site at <https://kateiren.github.io/>.

**Configuration checked on 2026-11-26:** GitHub Pages publishes from the root of the `migrate/minimal-mistakes` branch. This local checkout contains the Mediumish theme; the publishing branch may use a different theme. Recheck the Pages settings before relying on this branch name or applying theme-specific front matter.
