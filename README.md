# AIoT Notes

A personal blog about artificial intelligence and the Internet of Things, built with Hugo and PaperMod for GitHub Pages.

## Preview locally

Install Hugo Extended and Go. The [Pages workflow](.github/workflows/pages.yml) lists the versions used by CI. Hugo downloads the pinned PaperMod revision on the first build using `go.mod` and `go.sum`. [Install Hugo](https://gohugo.io/installation/), [Install Go](https://go.dev/doc/install)

```sh
hugo server --buildDrafts
```

Open <http://localhost:1313/>. Run `hugo build --gc --minify` to reproduce the production build in `public/`. Generated output is ignored by Git.

## Write a post

1. Create a branch such as `post/sensor-notes`.
2. Run `hugo new content posts/sensor-notes/index.md`. The post archetype supplies a title, date, description, tags, and `draft: true`.
3. Put post-specific images beside `index.md` and reference them with relative paths, such as `![Sensor enclosure](enclosure.jpg)`. Preview with drafts enabled.
4. When the post is ready, set `draft: false` and check that its date is current or past. Open a PR into `main` and merge after the **Build Hugo site** check passes. The merge publishes the site.

`content/posts/hello-aiot/index.md` is a starter draft. Hugo omits drafts and future-dated posts from the live site. Files on branches and PRs in a public repository are still visible, so keep private drafts outside the repo.

## Repository layout

| Path | Purpose |
| --- | --- |
| `hugo.toml` | Site title, URL, navigation, and theme settings |
| `content/about.md` | About page |
| `content/posts/` | Blog posts and images bundled with each post |
| `archetypes/posts.md` | Template for new posts |
| `go.mod`, `go.sum` | PaperMod version and checksums |
| `.github/workflows/pages.yml` | PR builds and GitHub Pages deployment |

## GitHub settings

1. Open **Settings → Pages** and set **Build and deployment → Source** to **GitHub Actions**. Keep the default `dzon2000.github.io` domain and enable **Enforce HTTPS** if it is selectable. [Pages settings](https://github.com/dzon2000/dzon2000.github.io/settings/pages), [GitHub Pages setup](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
2. Push or merge the setup to `main`. In **Actions → Hugo site**, check that the build and deployment pass. If Pages was enabled after the push, run **Hugo site** manually on `main`. The deployment URL should be <https://dzon2000.github.io/>.
3. In **Settings → Environments → github-pages**, restrict deployment branches to `main`. Keep the default workflow token read-only in **Settings → Actions → General**. The deploy job requests the Pages and OIDC permissions it needs. [Deployment environment settings](https://docs.github.com/en/actions/reference/workflows-and-actions/deployments-and-environments)
4. After the first PR build, consider a branch ruleset requiring PRs and the **Build Hugo site** status check on `main`. For a solo blog, leave required approvals at zero. [GitHub rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets)

## Maintain the site

Update PaperMod in a dedicated PR with `hugo mod get github.com/adityatelange/hugo-PaperMod@<commit-sha>` followed by `hugo mod tidy`. Commit both module files and run the production build. When upgrading Hugo, update the version and the matching Linux amd64 archive checksum in the workflow. To roll back a published change, revert its commit on `main` and let the workflow redeploy. [PaperMod releases](https://github.com/adityatelange/hugo-PaperMod/releases), [Hugo releases](https://github.com/gohugoio/hugo/releases)
