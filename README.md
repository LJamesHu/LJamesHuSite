# LJamesHuSite: Portfolio / Blog site for James Hu

Built off the great work from Mediumish with some significant features and editing to fit my preferences.

[Original Live Demo](https://wowthemesnet.github.io/mediumish-theme-jekyll/) &nbsp; | &nbsp; [Original Download](https://github.com/wowthemesnet/mediumish-theme-jekyll/archive/master.zip)

**Mediumish for Jekyll** is designed and developed by [Sal](https://www.wowthemes.net) and it is *free* under MIT license. 

This site is built by **GitHub Pages' native Jekyll processing** — just push the
source and GitHub builds and publishes it. No need to build locally or commit a
`docs/` folder anymore.

> One-time setup in the repo's **Settings → Pages**: set the source to
> **Deploy from a branch → `master` → `/ (root)`**.

Quick refresher for myself whenever I need to update this thing:

1. cd into folder, `git pull`
2. Perform necessary file updates
3. (Optional) preview locally: `bundle install` then `bundle exec jekyll serve`.
   The `github-pages` gem matches what GitHub builds; `_site/` is gitignored.
4. Push to GitHub: `git add .`, `git commit -m "Update x"`, `git push origin master`.
   GitHub Pages rebuilds the site automatically within a minute or so.

### GitHub Pages compatibility notes

GitHub's native build only allows a fixed set of plugins, so this site uses:

- `jekyll-paginate` (v1) for the `/blog/` pagination — **not** `jekyll-paginate-v2`.
- Manual category pages under `categories/` (`/technical/`, `/python/`,
  `/business/`) instead of `jekyll-archives`. **Add a new file there if you ever
  introduce a new post category.**
