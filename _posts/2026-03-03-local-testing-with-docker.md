---
layout: post
title: "Local Testing with Docker"
date: 2026-03-03
author:
  name: Robert Jackson
  email: rjackson@mtech.edu
---

One of the first infrastructure decisions we made for The Butte Forge website
was how to preview changes locally before pushing to GitHub Pages. We wanted
something that mirrors the production build as closely as possible, without
requiring every team member to install Ruby, Bundler, and Jekyll on their
host machine. Docker was the obvious choice.

## The Problem

GitHub Pages uses Jekyll to build the site from Markdown files. If you edit a
page and push it, you have to wait for the GitHub Actions pipeline to finish
before you can see whether the result looks right. That feedback loop is too
slow when you are iterating on layout, CSS, or content structure.

Installing Jekyll locally works, but Ruby environments are notoriously
fragile across platforms. We needed a solution that anyone on the team could
run with a single command, regardless of operating system.

## The Dockerfile

We use a multi-stage Docker build. The first stage installs Jekyll inside an
Alpine Linux container and builds the site from source, exactly the way
GitHub Pages does. The second stage copies the compiled HTML into a minimal
nginx container that serves it on port 80.

```dockerfile
FROM ruby:3.2-alpine AS builder
RUN apk add --no-cache build-base git
WORKDIR /site
COPY Gemfile ./
RUN bundle install
COPY . .
RUN bundle exec jekyll build --destination /site/_site

FROM nginx:alpine
RUN rm -rf /usr/share/nginx/html/*
COPY --from=builder /site/_site /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

The builder stage is cached aggressively. Unless the Gemfile changes, Docker
skips the `bundle install` step entirely, which means rebuilds after content
edits take about one second.

## Docker Compose

A simple `docker-compose.yml` wraps the build so the whole workflow fits in
one command:

```yaml
services:
  web:
    build: .
    ports:
      - "8080:80"
    restart: unless-stopped
```

To build and start the preview server:

```bash
docker compose up --build -d
```

Then visit `http://localhost:8080`. To stop it:

```bash
docker compose down
```

## Why This Works Well

- **Parity.** The Docker build uses the same Ruby and Jekyll versions as
  GitHub Pages, so what you see locally is what gets deployed.
- **Isolation.** No Ruby, gems, or native extensions on the host.
  Everything lives inside the container.
- **Speed.** Layer caching makes content-only rebuilds near-instant. A full
  cold build takes about 30 seconds.
- **One command.** Any team member can run `docker compose up --build -d`
  without setup beyond installing Docker itself.

This has become part of our standard workflow. Every change to a Markdown
page or config file gets a local preview before it goes to `main`.
