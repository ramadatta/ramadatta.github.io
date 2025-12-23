---
layout: post
title: "How I Built This Portfolio + Blog with Jekyll"
date: 2025-12-23
description: "A step-by-step guide to creating a personal website with Jekyll and GitHub Pages, including blog functionality and comments."
tags: [jekyll, github-pages, tutorial, portfolio]
categories: [tutorials]
views: 0
giscus_comments: true
---

Want to build a portfolio + blog like this one? Here's how I did it from scratch!

---

## Prerequisites

- **Ruby** (version 3.x recommended)
- **Git** installed
- **GitHub account**

---

## Step 1: Set Up Ruby with rbenv

```bash
# Install rbenv (macOS)
brew install rbenv ruby-build

# Install Ruby 3.3
rbenv install 3.3.0
rbenv global 3.3.0

# Verify
ruby -v
```

---

## Step 2: Create Your Jekyll Site

```bash
# Create project folder
mkdir my-portfolio && cd my-portfolio

# Initialize git
git init
```

---

## Step 3: Set Up the Project Structure

Your folder structure should look like this:

```
my-portfolio/
├── _config.yml          # Site configuration
├── _includes/           # Reusable HTML components
│   ├── header.html
│   ├── footer.html
│   └── comments.html
├── _layouts/            # Page templates
│   ├── default.html
│   ├── post.html
│   └── blog.html
├── _pages/              # Additional pages
├── _posts/              # Blog posts (YYYY-MM-DD-title.md)
├── _sass/               # SCSS stylesheets
│   ├── _themes.scss
│   ├── _base.scss
│   ├── _layout.scss
│   └── _components.scss
├── assets/
│   ├── css/main.scss
│   └── img/             # Your images
├── index.md             # Home/About page
├── blog.md              # Blog listing page
└── Gemfile              # Ruby dependencies
```

**Reference:** Check my [GitHub repo](https://github.com/ramadatta/ramadatta.github.io) for the complete file structure.

---

## Step 4: Create the Gemfile

```ruby
source "https://rubygems.org"

gem "jekyll", "~> 4.4"
gem "jekyll-feed", "~> 0.17"
gem "jekyll-sitemap", "~> 1.4"
gem "jekyll-seo-tag", "~> 2.8"
gem "jekyll-sass-converter", "~> 3.1"
gem "sass-embedded", "~> 1.97"
```

Then run:

```bash
bundle install
```

---

## Step 5: Configure _config.yml

```yaml
title: Your Name
description: Your tagline here
author:
  name: Your Name
  email: your@email.com

url: "https://yourusername.github.io"
baseurl: ""

markdown: kramdown
highlighter: rouge
permalink: /blog/:year/:month/:day/:title/

plugins:
  - jekyll-feed
  - jekyll-sitemap
  - jekyll-seo-tag

sass:
  sass_dir: _sass
  style: compressed
```

---

## Step 6: Create Your First Blog Post

Create a file in `_posts/` with the format `YYYY-MM-DD-title.md`:

```markdown
---
layout: post
title: "My First Post"
date: 2025-12-23
description: "Hello world!"
tags: [hello, first-post]
categories: [general]
giscus_comments: true
---

Welcome to my blog! This is my first post.
```

---

## Step 7: Test Locally

```bash
bundle exec jekyll serve --livereload
```

Visit `http://localhost:4000` to see your site!

---

## Step 8: Deploy to GitHub Pages

1. Create a repo named `yourusername.github.io` on GitHub
2. Push your code:

```bash
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/yourusername/yourusername.github.io.git
git push -u origin main
```

3. Go to **Settings → Pages** → Select `main` branch → Save
4. Wait 1-2 minutes, then visit `https://yourusername.github.io`

---

## Step 9: Enable Comments with Giscus

1. **Enable Discussions** on your repo (Settings → Features → Discussions)
2. **Create a "Comments" category** in Discussions
3. **Install Giscus app**: [github.com/apps/giscus](https://github.com/apps/giscus)
4. **Get your config**: Visit [giscus.app](https://giscus.app), enter your repo, and copy the `data-repo-id` and `data-category-id`
5. **Create `_includes/comments.html`**:

```html
<div class="comments-section">
  <h3>Comments</h3>
  <script src="https://giscus.app/client.js"
          data-repo="yourusername/yourusername.github.io"
          data-repo-id="YOUR_REPO_ID"
          data-category="Comments"
          data-category-id="YOUR_CATEGORY_ID"
          data-mapping="pathname"
          data-reactions-enabled="1"
          data-theme="light"
          data-lang="en"
          crossorigin="anonymous"
          async>
  </script>
</div>
```

6. **Include in `_layouts/post.html`**:

```liquid
{% raw %}{% if page.giscus_comments %}
  {% include comments.html %}
{% endif %}{% endraw %}
```

---

## That's It!

You now have a fully functional portfolio + blog with:
- ✅ Beautiful custom theme
- ✅ Blog with tags and categories
- ✅ GitHub Pages hosting (free!)
- ✅ Comments via Giscus

**Full source code:** [github.com/ramadatta/ramadatta.github.io](https://github.com/ramadatta/ramadatta.github.io)

---

*Questions? Drop a comment below!*

