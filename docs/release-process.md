# Release Process

This repository uses a split CI and release model:

- pull requests are validated automatically
- releases are created only when a maintainer pushes a semantic version tag
- the manual DingTalk workflow remains available for end-to-end notification checks

## Pull Request Validation

Every pull request to `main` runs the PR validation workflow.

The workflow checks:

- dependencies install successfully with `pnpm install --frozen-lockfile`
- TypeScript build succeeds
- the action bundles successfully with `pnpm run package`
- generated bundled files are already committed via `git diff --exit-code`

The PR workflow does not use DingTalk secrets and does not send real notifications.

## How Maintainers Publish a Release

After a pull request is merged and you decide the release version, create and push a semantic version tag.

```bash
git checkout main
git pull
git tag v1.2.3
git push origin v1.2.3
```

## What Happens After Pushing a Tag

Pushing a tag that matches `v*.*.*` triggers the release workflow.

That workflow will:

- install dependencies with a frozen lockfile
- rebuild the bundled action artifact
- verify generated files are committed
- create a GitHub Release with generated release notes
- update the floating major tag such as `v1`
