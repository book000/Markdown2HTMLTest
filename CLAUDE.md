# CLAUDE.md

## Project overview

Markdown2HTMLTest is a demonstration/test repository that automatically converts
every Markdown file in the repo to HTML. It exists to exercise a GitHub Actions +
pandoc conversion pipeline, not to ship an application. The Markdown files
(`test.md`, `folder/file.md`) are sample GitHub-Flavored Markdown documents used
as conversion input.

## How the conversion works

- `.github/workflows/convertMarkdown.yml` runs on every push to the `master` branch.
- It installs `pandoc`, converts each `*.md` file to a sibling `*.html` file, deletes
  all non-HTML files, and force-pushes the result to the `release` branch.
- The `release` branch is generated output only. Never edit it by hand or open PRs
  against it — every run overwrites it with `git push -f`.

## Working on this repository

- Author content on `master`. The repo is primarily Markdown sources (plus the
  workflow), but may include a small number of non-Markdown fixture files
  (e.g. `新しいテキスト ドキュメント.txt`) for edge cases such as filenames with spaces.
- There is no local build, test, or lint tooling (no `package.json`, no test suite).
  To verify a change, push to `master` (or run the pandoc command below locally) and
  inspect the generated HTML on the `release` branch.
- Local one-off check of the conversion (mirrors the workflow):
  `find -type f -name '*.md' | sed -e 's/\.md$//' | xargs -L 1 -I {} pandoc --verbose -f markdown_github -t html -o {}.html {}.md`

## Conventions and cautions

- Keep `master` limited to source Markdown; do not commit generated `*.html` there
  (the workflow produces those on `release`).
- Do not hardcode real credentials in the workflow; the commit identity is a bot
  placeholder.
