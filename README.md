# itdeepdive.com

Source of [itdeepdive.com](https://itdeepdive.com/): a [Hugo](https://gohugo.io/) site using the
[hugo-clarity](https://github.com/chipzoller/hugo-clarity) theme (git submodule in `themes/`).
Every push to `master` is built and deployed to GitHub Pages by `.github/workflows/hugo.yml`.

## Local preview

```bash
git clone --recurse-submodules <repo-url>
hugo server -D        # http://localhost:1313, -D also shows drafts
```

Use the same Hugo **extended** version as CI (`HUGO_VERSION` in the workflow), e.g. into `~/.local/bin`:

```bash
V=0.168.0
curl -sSL "https://github.com/gohugoio/hugo/releases/download/v$V/hugo_extended_${V}_linux-amd64.tar.gz" | tar xz -C ~/.local/bin hugo
```

## Upgrading

- **Hugo:** bump `HUGO_VERSION` in the workflow and your local binary together, then `hugo` and fix any warnings.
- **Theme:** `git -C themes/hugo-clarity pull origin master`, build, commit the submodule pointer.
  Local overrides to re-check against the theme after an upgrade:
  - `layouts/partials/comments.html` – theme copy with the "See Also" list (`see-also.html`) in front.
  - `assets/sass/_override.sass` – colour/variable overrides.
