# THE BUTTE FORGE: EMBEDDED SYSTEMS & AI

Official website for THE BUTTE FORGE: EMBEDDED SYSTEMS & AI, a student club
at Montana Tech.

Live site: [https://butteforge.github.io](https://butteforge.github.io)

## About

This repository hosts the source for our club website, built with
[Jekyll](https://jekyllrb.com/) and the
[minima](https://github.com/jekyll/minima) theme. GitHub Pages automatically
builds and deploys the site on every push to the main branch.

## Local Development

You need [Docker](https://www.docker.com/) installed.

```bash
# Build and start the site locally
docker compose up --build -d

# View the site
open http://localhost:8080

# Stop the local server
docker compose down
```

The Docker setup uses a multi-stage build: Jekyll renders the Markdown into
HTML (mirroring GitHub Pages), then nginx serves the static files.

## Project Structure

| File / Directory        | Purpose                                          |
|-------------------------|--------------------------------------------------|
| `pages/`                | Site pages (Markdown content)                    |
| `pages/index.md`        | Main landing page                                |
| `pages/about/`          | About section (purpose, team, Montana Tech)      |
| `pages/departments/`    | Department pages (software, hardware, etc.)      |
| `pages/activity/`       | Activity section (meetings, status)              |
| `pages/blog.md`         | Blog listing page                                |
| `_posts/`               | Blog posts (`YYYY-MM-DD-title.md`)               |
| `_config.yml`           | Jekyll configuration (theme, navigation, etc.)   |
| `_layouts/`             | HTML layouts (`default.html`, `post.html`)       |
| `_includes/`            | Shared partials (header, footer, icons)          |
| `assets/css/style.scss` | Custom styles                                    |
| `assets/images/`        | Images (logo, etc.)                              |
| `assets/icons/`         | SVG icon source files                            |
| `favicon.ico`           | Site favicon                                     |
| `Dockerfile`            | Multi-stage build for local preview              |
| `docker-compose.yml`    | Docker Compose config (serves on port 8080)      |
| `Gemfile`               | Ruby dependencies for Jekyll / GitHub Pages      |
| `AGENTS.md`             | Instructions for AI coding agents                |
| `README.md`             | This file                                        |

## Adding a New Page

1. Create a new `.md` file in the appropriate `pages/` subdirectory
   (e.g., `pages/about/`, `pages/departments/`, `pages/activity/`).
2. Add front matter with `layout: default`, `title`, and a `permalink`.
3. Add the page path to the appropriate group under the `nav` list in
   `_config.yml` so it appears in the site navigation.

## Writing Guidelines

- Write content in Markdown (kramdown flavor).
- Do not use emoji or decorative Unicode characters.
- Do not use em-dashes or en-dashes.
- Do not use double hyphens (`--`) as separators; use a single hyphen.
- Keep lines under 120 characters where practical.
- Do not add JavaScript, external CSS, or tracking scripts without approval.

## Writing Blog Posts

Blog posts live in the `_posts/` directory. This is a
[special Jekyll directory](https://jekyllrb.com/docs/posts/) that is
processed automatically; do not rename or move it.

Each post filename must follow the format `YYYY-MM-DD-title.md` (for
example, `2026-03-03-local-testing-with-docker.md`). Jekyll uses the date
and title from the filename to generate the URL.

Add the following front matter at the top of every post:

```yaml
---
layout: post
title: "Your Post Title"
date: 2026-03-03
author:
  name: Your Name
  email: you@mtech.edu
---
```

The `permalink` setting in `_config.yml` maps posts to `/blog/:slug.html`,
so visitors see URLs like `/blog/local-testing-with-docker.html` even though
the source files are in `_posts/`.

For more details, see the
[Jekyll Posts documentation](https://jekyllrb.com/docs/posts/).

## Contributing

1. Fork or clone the repository.
2. Create a feature branch.
3. Make your changes and test locally with Docker.
4. Open a pull request with a clear description of what changed and why.

## License

(c) 2026 THE BUTTE FORGE: EMBEDDED SYSTEMS & AI - All rights reserved.
