# AGENTS.md

Project instructions for every AI agent working in this repository, and the single place they are
written. Copilot CLI loads this file directly; Claude Code loads it through the `@AGENTS.md` import
in `CLAUDE.md`.

## Project context

This repository holds the datasets that the OpenCRQ connectors synchronize into the platform, in
`data/`. It has no code and no build: [README.md](README.md) describes each dataset.

<!-- filigran-conventions:start -->
## Commit, PR & issue conventions

All commits, pull requests and issues in this repository follow the
[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
specification with a GitHub issue reference:

```
type(scope?)!?: description (#issue)
```

- Types: `feat`, `fix`, `chore`, `docs`, `style`, `refactor`, `perf`, `test`,
  `build`, `ci`, `revert`.
- The description starts with a lowercase letter and has no trailing period;
  preserve acronyms and proper nouns.
- The old `[backend]` / `[frontend]` bracket prefixes are discontinued — use a
  Conventional Commits scope instead.
- Pull request titles **must** end with the related issue reference, e.g.
  `(#1234)`, and every pull request must be linked to an issue.
- Sign your commits.

When generating commit messages, PR titles or issue titles, always follow this
convention. See [`.github/LABELS.md`](.github/LABELS.md) for the full title and
label taxonomy.
<!-- filigran-conventions:end -->

The `(#issue)` reference is for commits and pull request titles only: an issue title omits it, since
the issue is the reference.

### Links in public metadata

This repository is public. Issues, pull requests, comments and commit messages are read by people
who cannot open a private repository, so none of them links one (an issue, pull request, commit,
file or advisory hosted there): the link is a 404 for them and leaks the repository's name. To tell
whether a repository is private, run `gh repo view <owner/repo> --json visibility`: anything but
`PUBLIC` is, and so is a repository the command cannot resolve (`Could not resolve to a
Repository`), which is what a private repository looks like from outside. To connect the two, link
the public item from the private one.
