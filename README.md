# Conventional-Commit-Messages

This guide outlines the Conventional Commits specification, a lightweight convention for Git commit messages. It helps create an explicit commit history, making it easier to write automated tools (e.g., for generating changelogs or determining semantic version bumps). The format is inspired by the Angular commit guidelines and aligns with Semantic Versioning (SemVer).

## Commit Message Format

A conventional commit message follows this structure:

```
<type>[optional scope][optional !]: <description>

[optional body]

[optional footer(s)]
```

- **Subject Line (Header)**: `<type>[optional scope][optional !]: <description>`
- The entire subject line should ideally be 72 characters or less.
- Use imperative, present tense in the description (e.g., "add" not "added").
- No capitalization of the first letter in the description.
- No period (.) at the end of the description.

## Types

The type must be one of the following:

| Type      | Description |
|-----------|-------------|
| feat     | Commits that add or remove a new feature. Correlates with MINOR in SemVer. |
| fix      | Commits that fix a bug. Correlates with PATCH in SemVer. |
| refactor | Commits that rewrite/restructure code without changing API behavior. |
| perf     | Special refactor commits that improve performance. |
| style    | Commits that do not affect meaning (e.g., white-space, formatting, missing semi-colons). |
| test     | Commits that add missing tests or correct existing tests. |
| docs     | Commits that affect documentation only. |
| build    | Commits that affect build components (e.g., build tool, CI pipeline, dependencies, project version). |
| ops      | Commits that affect operational components (e.g., infrastructure, deployment, backup, recovery). |
| chore    | Miscellaneous commits (e.g., modifying .gitignore). |

Other types like `ci` are allowed but not used here. Types are extensible per project.<grok:render card_id="c63e0c" card_type="citation_card" type="render_inline_citation">
<argument name="citation_id">0</argument>
</grok:render>

## Scopes

- Optional contextual information after the type, in parentheses (e.g., `feat(parser)`).
- Project-specific; no standard list.
- Do not use issue identifiers as scopes.
- Can include spaces if needed (e.g., `feat(shopping cart)`), though kebab-case is common.

## Breaking Changes Indicator

- Optional: Use `!` before the `:` to indicate breaking changes (e.g., `feat(api)!: remove status endpoint`).
- Correlates with MAJOR in SemVer, regardless of type.
- If used, a `BREAKING CHANGE:` footer is optional but recommended for details.

## Description

- Mandatory concise summary of the change.
- Imperative, present tense (e.g., "add email notifications").
- Prefix with "This commit will..." in mind.

## Body

- Optional detailed explanation.
- Starts after a blank line from the description.
- Imperative, present tense.
- Include motivation, contrasts with previous behavior, and issue references here.

## Footer

- Optional metadata, after a blank line from the body.
- Format: Token followed by `:<space>` or `<space>#` and value (e.g., `Refs: #123`).
- For breaking changes: Start with `BREAKING CHANGES:` (note the plural and uppercase), followed by a description.
- Reference issues (e.g., `Closes: #1337`).

## Examples

- `feat: add email notifications on new direct messages`
- `feat(shopping cart): add the amazing button`
- `feat!: remove ticket list endpoint

refers to JIRA-1337

BREAKING CHANGES: ticket endpoints no longer support list all entities.`
- `fix(api): handle empty message in request body`
- `fix(api): fix wrong calculation of request body checksum`
- `fix: add missing parameter to service call

The error occurred because of <reasons>.`
- `perf: decrease memory footprint for determine unique visitors by using HyperLogLog`
- `build: update dependencies`
- `build(release): bump version to 1.0.0`
- `refactor: implement fibonacci number calculation as recursion`
- `style: remove empty line`

## Git Hook Scripts to Ensure Commit Message Header Format

### commit-msg Hook (Local)

Ensure Node.js and `npx` are installed. This hook uses the `git-conventional-commits` tool, which supports the types listed above, including custom configurations via `git-conventional-commits.yaml`.<grok:render card_id="93328f" card_type="citation_card" type="render_inline_citation">
<argument name="citation_id">1</argument>
</grok:render>

Create `.git-hooks/commit-msg`:

```bash
#!/usr/bin/env sh

commit_message="$1"
# Exit with a non-zero code for invalid messages

# Use git-conventional-commits: https://github.com/qoomon/git-conventional-commits
npx git-conventional-commits commit-msg-hook "$commit_message"

# Or verify $commit_message with your own tooling
# ...
```

- Make executable: `chmod +x .git-hooks/commit-msg`
- Set Git hooks path: `git config core.hooksPath .git-hooks`
- Commit `.git-hooks/` to share with your team.

### pre-receive Hook (Server Side)

This Bash script blocks pushes with non-conforming commit messages. Corrected to include all types, support `!` for breaking changes, and handle merge commits.

Create `.git/hooks/pre-receive`:

```bash
#!/usr/bin/env bash

# Pre-receive hook that blocks commits with messages not following the regex rule

commit_msg_type_regex='feat|fix|refactor|perf|style|test|docs|build|ops|chore'
commit_msg_scope_regex='.{1,20}'
commit_msg_description_regex='.{1,100}'
commit_msg_regex="^(${commit_msg_type_regex})(\(${commit_msg_scope_regex}\))?(!)?: (${commit_msg_description_regex})\$"
merge_msg_regex="^Merge branch '.+'\$"

zero_commit="0000000000000000000000000000000000000000"

# Do not traverse over commits already in the repository
excludeExisting="--not --all"

error=""
while read oldrev newrev refname; do
  # Branch or tag deleted
  if [ "$newrev" = "$zero_commit" ]; then
    continue
  fi

  # Check for new branch or tag
  if [ "$oldrev" = "$zero_commit" ]; then
    rev_span=$(git rev-list $newrev $excludeExisting)
  else
    rev_span=$(git rev-list $oldrev..$newrev $excludeExisting)
  fi

  for commit in $rev_span; do
    commit_msg_header=$(git show -s --format=%s $commit)
    if ! [[ "$commit_msg_header" =~ (${commit_msg_regex})|(${merge_msg_regex}) ]]; then
      echo "$commit" >&2
      echo "ERROR: Invalid commit message format" >&2
      echo "$commit_msg_header" >&2
      error="true"
    fi
  done
done

if [ -n "$error" ]; then
  exit 1
fi
```

- Make executable: `chmod +x .git/hooks/pre-receive`

## References

- https://www.conventionalcommits.org/<grok:render card_id="5a19d7" card_type="citation_card" type="render_inline_citation">
<argument name="citation_id">0</argument>
</grok:render>
- https://github.com/angular/angular/blob/master/CONTRIBUTING.md
- http://karma-runner.github.io/1.0/dev/git-commit-msg.html
- https://github.com/github/platform-samples/tree/master/pre-receive-hooks
- https://github.community/t5/GitHub-Enterprise-Best-Practices/Using-pre-receive-hooks-in-GitHub-Enterprise/ba-p/13863
