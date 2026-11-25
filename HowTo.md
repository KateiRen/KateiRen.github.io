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

The current permalink configuration is `/:title/`, so post URLs do not include the date. Using `post_url` avoids having to guess the generated URL.

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

## Preview locally

With Ruby, Bundler, and the project's dependencies available, run from the repository root:

```powershell
bundle exec jekyll serve
```

Open **http://localhost:4000**.

If dependencies are missing, install them with `bundle install`, then retry. Stop the preview server with **Ctrl+C**.

Check the post, images, downloads, and cross-links before publishing.

## Publish

1. Check the repository's **Settings > Pages > Build and deployment** for the publishing branch and directory.
2. Commit the post and all referenced media.
3. Push the changes to the configured publishing branch.
4. Wait for GitHub Pages to rebuild, then check the live site at <https://kateiren.github.io/>.

**Configuration checked on 2026-11-26:** GitHub Pages publishes from the root of the `migrate/minimal-mistakes` branch. This local checkout contains the Mediumish theme; the publishing branch may use a different theme. Recheck the Pages settings before relying on this branch name or applying theme-specific front matter.
