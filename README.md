# ramadatta.github.io

Personal portfolio + blog built with Jekyll and hosted on GitHub Pages.

**Live site:** [ramadatta.github.io](https://ramadatta.github.io)

## Updating content

- **Homepage** (bio, research tabs, publications, presentations, etc.): `index.md`.
  Sidebar section links and profile buttons are listed at the top of the file.
- **Blog posts:** add a file to `_posts/` (start from `_posts/TEMPLATE.md`).
- **Gallery:** create a folder in `assets/img/gallery/`, named like
  `Conference, City (Mon YYYY)`, and put the photos in it. Each folder becomes
  one album. Optional album captions go in `_data/gallery.yml`.
- **Colors:** `_sass/_themes-editorial.scss`. Fonts: `_sass/_base.scss` and
  `_includes/head.html`.

## Run locally

```sh
eval "$(rbenv init - zsh)"
bundle exec jekyll serve --livereload
```

Then open http://127.0.0.1:4000/.
