# Deployment Guide

## GitHub Pages Setup

1. **Push your code to GitHub**:
   ```bash
   git add .
   git commit -m "Initial blog setup"
   git push origin master
   ```

2. **Enable GitHub Pages**:
   - Go to your repository settings on GitHub
   - Navigate to "Pages" section
   - Under "Build and deployment", select "GitHub Actions" as the source

## Custom Domain Setup with Cloudflare

1. **In Cloudflare**:
   - Add a CNAME record: `@` pointing to `janschill.github.io`
   - Add a CNAME record: `www` pointing to `janschill.github.io` 
   - Set Proxy status to "Proxied" (orange cloud)
   - SSL/TLS settings: Set to "Full"

2. **In GitHub**:
   - The CNAME file is already configured with `janschill.de`
   - GitHub will automatically provision an SSL certificate

## Plausible Analytics

- Analytics tracking is already configured in `_includes/head.html`
- Script will only load in production environment
- Visit [plausible.io](https://plausible.io) to:
  - Create your account
  - Add `janschill.de` as a website
  - View your analytics dashboard

## Writing New Posts

Create new posts in `_posts/` with the format: `YYYY-MM-DD-post-title.md`

Example:
```markdown
---
layout: post
title: "Your Post Title"
description: "A brief description"
date: 2025-06-28
tags: [tag1, tag2]
---

Your content here...
```

## Local Development

```bash
bundle exec jekyll serve
```

Visit http://localhost:4000 to preview changes.