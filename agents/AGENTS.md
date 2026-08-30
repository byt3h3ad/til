# TIL Contribution Workflow

## Add a TIL

1. Put one Markdown file in the existing topic directory that best matches it. Create a topic directory only when none fits.
2. Use a concise `kebab-case.md` filename and a single `# ` title. The README builder uses that first H1 as the index label.
3. Write a short, practical note: explain the problem or useful behavior, show the smallest correct command or code example, and cite the original source when applicable.
4. Keep a TIL self-contained. Do not add application code, package configuration, or tests for a documentation-only note.
5. Do not edit the generated part of `README.md`. The `Build README` GitHub Action rebuilds its timestamp, count, categories, and links after pushes to `main`.

## Verify and publish

1. Run `git pull` before making any repository changes. The README workflow commits the refreshed index after each push to `main`; pulling first prevents a stale README from conflicting with the next content commit.
2. Check that the example is valid for its documented tool or runtime when practical.
3. Review recent commits before choosing a message. Content additions use `feat: <topic>/<file>.md`.
4. Stage only the intended files, commit, and push to `main`.

## Repository automation

`.github/workflows/build.yml` indexes TIL Markdown files and excludes `README.md` plus this guidance file. Keep agent guidance in `agents/AGENTS.md`; it is repository workflow documentation, not a TIL.
