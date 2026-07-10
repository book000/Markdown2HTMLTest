# Copilot code review instructions

Markdown2HTMLTest is a test repository whose purpose is to convert Markdown to HTML
via a GitHub Actions + pandoc pipeline. It has no application code, package manager,
or test suite — most content is sample Markdown plus one workflow file.

## What to review

- `.github/workflows/convertMarkdown.yml` is the only executable logic. Check for:
  - Shell word-splitting or injection risks in the `find | xargs | pandoc` and `rm`
    pipelines (filenames with spaces already exist in this repo).
  - Destructive steps: the workflow deletes all non-HTML files and force-pushes the
    `release` branch. Flag changes that could delete or overwrite unintended files or
    branches.
  - Action versions that are outdated or unpinned (e.g. `actions/checkout`).

## What NOT to flag

- Missing tests, linters, or `package.json`: this repository intentionally has none.
- The Markdown files (`test.md`, `folder/file.md`) are deliberate GFM feature samples.
  Do not report their unusual constructs (placeholder `⋅` dots, intentionally
  malformed lists, raw HTML blocks such as the `<dl>` definition list, raw
  `<b>`/`<em>` tags) as bugs — they are test fixtures.
- Generated `*.html` files and the `release` branch: these are build output, not source.
- Non-ASCII filenames or Japanese text: expected in this repository.
