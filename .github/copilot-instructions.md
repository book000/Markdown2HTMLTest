# Copilot code review instructions

Markdown2HTMLTest is a test repository whose purpose is to convert Markdown to HTML
via a GitHub Actions + pandoc pipeline. It has no application code, package manager,
or test suite — most content is sample Markdown plus one workflow file.

## What to review

- `.github/workflows/convertMarkdown.yml` is the only executable logic. Check for:
  - Shell word-splitting or injection risks in the `find | xargs | pandoc` and `rm`
    pipelines. A filename with spaces already exists in this repo (`新しいテキスト
    ドキュメント.txt`); today it is not a `*.md` file so it only exercises the `rm`
    pipeline, but it would also exercise the pandoc pipeline if a future `*.md` path
    contained spaces.
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
