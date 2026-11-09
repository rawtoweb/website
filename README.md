# rawtoweb website

The website of rawtoweb, the workshop offer of Global Health Engineering, ETH Zurich, in which a raw dataset becomes a published data package in front of the room, with every AI step disclosed. One site serves every workshop (cohort): the shared pages are the manual, and each cohort gets a snapshot under its own path, for example `/dcc-2027/`. Model: gitklar/website, decision 0002 in `rawtoweb-dev/admin`.

## How the site is built

- `quarto render` renders the root into `_site/`.
- `quarto render --profile cohort,<name> --no-clean` renders a cohort into `_site/<name>/`, with the values of `_quarto-<name>.yml` (`cohort.*`, read in the pages with `{{< meta cohort.key >}}`) and the blocks marked `when-profile="cohort"`.
- `.github/workflows/publish.yml` does both on every push to `main` and publishes `_site` to GitHub Pages (source: GitHub Actions). An active cohort renders from `main`; a frozen one from the tag of its name.
- Nothing rendered is committed. Shared values that never change between cohorts are in `_variables.yml`.

Quarto is not on the shell PATH of the build machine; use `/Applications/RStudio.app/Contents/Resources/app/quarto/bin/quarto`.

## Add a cohort

1. Copy `_quarto-dcc-2027.yml` to `_quarto-<name>.yml` and fill in the values. Names are the event plus the year (dcc-2027), or audience plus a two-digit counter when an audience returns.
2. Add the line `<name> active` to `cohorts`.
3. Add the cohort to the table on `index.qmd` (root block).
4. Open the pull request from `dev` into `main`. The merge publishes it at `https://rawtoweb.github.io/website/<name>/`.
5. Put that address into the cohort config of `rawtoweb-dev/admin`.

## Freeze a cohort

After the workshop: `git tag <name>` on the commit the cohort last saw, push the tag, change its line in `cohorts` to `frozen`, merge. The pages stay as they were.

## Licence

Content CC BY 4.0, code MIT (see LICENSE).
