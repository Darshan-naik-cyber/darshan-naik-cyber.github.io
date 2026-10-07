# Darshan Naik: personal site

Minimal Jekyll site with a blog, ready for GitHub Pages.

## Deploy
1. Create a repo named `<your-username>.github.io` and push these files to `main`.
2. In **Settings > Pages**, set source to **Deploy from a branch**, branch `main`, folder `/ (root)`.
3. Edit `_config.yml`: set `url`, `email` and your `social` handles.
   (For a project repo instead, also set `baseurl: "/repo-name"`.)

## Write a post
Add `_posts/YYYY-MM-DD-title.md` with front matter `title:` and optional `tags:`. Push and it publishes.

## Run locally
```
bundle install
bundle exec jekyll serve
```
