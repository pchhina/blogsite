# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Hugo static site generator project for a personal blog and portfolio. The site uses a custom theme with minimal external dependencies.

## Common Commands

### Development
```bash
hugo server -D        # Run development server with drafts enabled
hugo server           # Run development server (published content only)
```

### Build
```bash
hugo                  # Build the site to ./public directory
```

### Content Management
```bash
hugo new blog/post-name.md              # Create new blog post
```

## Architecture

### Content Structure
- `content/blog/` - Blog posts (100+ markdown files)
- `content/_index.md` - Homepage content

### Layout System
- `layouts/index.html` - Homepage template (renders profile image and intro text)
- `layouts/_default/list.html` - List pages (blog index)
- `layouts/_default/single.html` - Individual post/page template
- `layouts/_default/terms.html` - Tag listing pages
- `layouts/partials/header.html` - Navigation and common head elements
- `layouts/partials/footer.html` - Footer partial
- `layouts/partials/mathjax_support.html` - Math rendering support for technical posts
- `layouts/partials/tags.html` - Tag display component

### Assets
- `static/css/styles.css` - Main stylesheet
- `static/img/` - Static images
- `assets/images/` - Hugo-processed images (e.g., profile.png used in homepage)

### Configuration
- `config.toml` - Site configuration with title, theme settings, and syntax highlighting (pygments with "friendly" style)

### Navigation Structure
The site has a simple navigation:
- Home (/)
- Blog (/blog/)
- Tags (/tags/)
