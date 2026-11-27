# t00m-s.github.io

Personal site of Tommaso Soncin, built with [Zola](https://www.getzola.org) and the
[tabi](https://github.com/welpo/tabi) theme, deployed to GitHub Pages via GitHub Actions.

## Local development

Install [Zola](https://www.getzola.org/documentation/getting-started/installation/)
(exact version pinned in `.github/workflows/zola.yaml`), then:

```sh
zola serve
```

The site is available at `http://127.0.0.1:1111`.

## Structure

```
config.toml               Site + theme configuration (most tabi settings live in [extra])
content/
  _index.md               Homepage: banner, intro, latest posts
  posts/                  Blog posts
  archive/_index.md       Archive page (lists everything in posts/)
  pages/about/            About page (/about)
  pages/cv/               CV page (/cv)
themes/tabi               The theme, as a git submodule (pinned at v5.0.0)
static/                   Static files (favicon, cv.pdf, custom icons…)
.github/workflows/zola.yaml   CI: builds with Zola, deploys to GitHub Pages
```

## Writing posts

Create a Markdown file in `content/posts/`. The existing posts use TOML front matter:

```toml
+++
title = "My post"
date = 2026-01-01
draft = false

[taxonomies]
tags = ["ctf", "writeup"]
+++
```

Notes:

- File names become URLs (`slugify.paths = "safe"` in `config.toml`): use
  underscores or dashes, avoid spaces and uppercase letters.
- Set `draft = true` to hide a post from the built site.
- Tags are supported: once a post has tags, you may want to add a
  `{ name = "tags", url = "tags", trailing_slash = true }` entry to `menu` in
  `config.toml`.
- `zola check` validates internal/external links.

## CV (PDF)

Drop the PDF into `static/` as `cv.pdf`; it will be served at
<https://t00m-s.github.io/cv.pdf>. Then add a link to it in `content/pages/cv/index.md`.

## Deployment

Pushing to `main` triggers `.github/workflows/zola.yaml`, which builds the site
with the [official Zola action](https://github.com/getzola/github-pages) and deploys
it to GitHub Pages (repository setting: source *GitHub Actions*). No `gh-pages`
branch is used.

## Maintenance

The tabi theme is pinned at tag **v5.0.0** and requires **Zola v0.23.6**. To upgrade:

```sh
cd themes/tabi && git fetch --tags && git checkout vX.Y.Z
# Update zola_version in .github/workflows/zola.yaml to match theme.toml's min_version
```

Every available tabi option is documented in `themes/tabi/theme.toml` and at
<https://welpo.github.io/tabi/blog/mastering-tabi-settings/>.
