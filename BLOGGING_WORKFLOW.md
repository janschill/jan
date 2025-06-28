# Blogging Workflow

## Quick Start

1. **Copy the template**:
   ```bash
   cp _templates/post-template.md _posts/$(date +%Y-%m-%d)-your-post-title.md
   ```

2. **Edit the post** in your favorite editor

3. **Preview locally**:
   ```bash
   bundle exec jekyll serve
   # Visit http://localhost:4000
   ```

4. **Publish**:
   ```bash
   git add .
   git commit -m "Add new post: Your Post Title"
   git push origin master
   ```

## File Naming Convention

Posts MUST be named: `YYYY-MM-DD-post-title.md`

Examples:
- `2025-06-28-setting-up-jekyll-blog.md`
- `2025-07-01-javascript-tips-and-tricks.md`

## Front Matter (Required)

```yaml
---
layout: post
title: "Your Post Title"
description: "SEO description (appears in Google/social shares)"
date: 2025-06-28
tags: [javascript, tutorial, web-development]
---
```

## Adding Images

1. **Save images** in `/assets/images/YYYY/` (e.g., `/assets/images/2025/`)

2. **Reference in posts**:
   ```markdown
   ![Alt text](/assets/images/2025/screenshot.png)
   *Optional caption in italics*
   ```

3. **Image formats**: Use `.jpg` for photos, `.png` for screenshots, `.svg` for icons

## Code Snippets

### Inline Code
Use backticks: `const variable = "value"`

### Code Blocks
Use triple backticks with language:

\`\`\`javascript
function example() {
    return "This will be syntax highlighted";
}
\`\`\`

### Supported Languages
- `javascript`, `js`
- `python`, `py`
- `bash`, `shell`
- `html`, `css`
- `json`, `yaml`
- `sql`, `markdown`

## Writing Tips

### Headlines
- Use `##` for main sections
- Use `###` for subsections
- Keep titles descriptive and scannable

### Links
- Internal: `[About page](/about)`
- External: `[GitHub](https://github.com/janschill)`
- Always include meaningful link text

### SEO Best Practices
- Write compelling descriptions (150-160 characters)
- Use relevant tags (3-5 per post)
- Include alt text for all images
- Use descriptive filenames for images

## Drafts

Create drafts in `_drafts/` folder (no date needed):
```
_drafts/my-upcoming-post.md
```

Preview drafts locally with:
```bash
bundle exec jekyll serve --drafts
```

## Tags

Common tags to use:
- `tutorial`, `how-to`, `guide`
- `javascript`, `python`, `css`, `html`
- `productivity`, `tools`, `workflow`
- `web-development`, `programming`
- `personal`, `reflection`, `learning`

## Publishing Checklist

- [ ] Proofread for typos and grammar
- [ ] Test all links work
- [ ] Check images display correctly
- [ ] Verify code snippets are properly formatted
- [ ] Preview locally before pushing
- [ ] Write descriptive commit message