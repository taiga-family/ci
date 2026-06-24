# Actions

Ready-made actions for continuous integration

---

## Auto approve by single user

Auto-approves a pull request using a single token.

| Input   | Description  | Default    |
| ------- | ------------ | ---------- |
| `token` | GitHub token | (required) |

```yml
- uses: taiga-family/actions/auto-approve-single@main
  with:
    token: ${{ secrets.APPROVE_BOT_TOKEN }}
```

## Auto approve by two users

Auto-approves a pull request using two different tokens (for branch protection requiring 2 approvals).

| Input    | Description  | Default    |
| -------- | ------------ | ---------- |
| `token1` | First token  | (required) |
| `token2` | Second token | (required) |

```yml
- uses: taiga-family/actions/auto-approve-double@main
  with:
    token1: ${{ secrets.APPROVER1_TOKEN }}
    token2: ${{ secrets.APPROVER2_TOKEN }}
```

## Auto cleanup cache

Deletes all GitHub Actions caches for a specific branch.

| Input         | Description                | Default    |
| ------------- | -------------------------- | ---------- |
| `branch-name` | Branch to clean caches for | (required) |

```yml
- uses: taiga-family/actions/auto-cleanup-cache@main
  with:
    branch-name: ${{ github.head_ref }}
```

## Auto label community

Labels merged fork PRs and their linked issues with `community contribution`. Removes `contributions welcome` label.

| Input               | Description         | Default                  |
| ------------------- | ------------------- | ------------------------ |
| `github-token`      | GitHub token        | (required)               |
| `pr-number`         | Pull request number | (required)               |
| `label-name`        | Label to apply      | `community contribution` |
| `remove-label-name` | Label to remove     | `contributions welcome`  |

```yml
- uses: taiga-family/actions/auto-label-community@main
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    pr-number: ${{ github.event.pull_request.number }}
```

## Auto label when approved

Adds a label when a PR reaches the required number of approvals.

| Input       | Description              | Default          |
| ----------- | ------------------------ | ---------------- |
| `token`     | GitHub token             | (required)       |
| `approvals` | Required approvals count | `2`              |
| `label`     | Label to add             | `ready to merge` |

```yml
- uses: taiga-family/actions/auto-label-when-approved@main
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
```

## Auto merge

Marks a pull request for auto-merge when all checks pass.

| Input          | Description                                  | Default            |
| -------------- | -------------------------------------------- | ------------------ |
| `token`        | GitHub token                                 | (required)         |
| `login`        | Bot name                                     | `taiga-family-bot` |
| `merge-method` | Merge method: `MERGE`, `SQUASH`, or `REBASE` | `SQUASH`           |

```yml
- uses: taiga-family/actions/auto-merge@main
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
```

## Auto push

Commits and pushes changes to the PR branch. Skips forks and the main branch.

| Input     | Description     | Default                                                 |
| --------- | --------------- | ------------------------------------------------------- |
| `token`   | GitHub token    | (required)                                              |
| `name`    | Commit username | `taiga-family-bot`                                      |
| `email`   | Commit email    | `41898282+github-actions[bot]@users.noreply.github.com` |
| `message` | Commit message  | `apply changes after linting`                           |

```yml
- uses: taiga-family/actions/auto-push@main
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
```

## Auto remove label

Removes a label from the current issue/PR.

| Input   | Description     | Default              |
| ------- | --------------- | -------------------- |
| `label` | Label to remove | `state: need triage` |

```yml
- uses: taiga-family/actions/auto-remove-label@main
```

## Cypress screenshot diff

Combines Cypress failed screenshot pairs (actual + expected) into a single diff image for easier review.

```yml
- uses: taiga-family/actions/cypress-screenshot-diff@main
```

## Deploy GitHub Pages

Deploys a folder to GitHub Pages.

| Input    | Description                       | Default     |
| -------- | --------------------------------- | ----------- |
| `token`  | GitHub token                      | (required)  |
| `folder` | Folder to deploy                  | `dist/demo` |
| `clean`  | Clean target branch before deploy | `true`      |
| `branch` | Target branch                     | `gh-pages`  |

```yml
- uses: taiga-family/actions/deploy-github-pages@main
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
    folder: dist/demo/browser
```

## Get node info

Reads Node.js workspace info (version, release candidate status) without full setup. Internally calls `setup-variables`
and `setup-node`.

| Output                       | Description                               |
| ---------------------------- | ----------------------------------------- |
| `root-package-version`       | Full version of root package.json (x.y.z) |
| `root-package-major-version` | Only major version (x)                    |
| `is-release-candidate`       | Whether version includes -rc              |

```yml
- uses: taiga-family/actions/setup-node@main
- uses: taiga-family/actions/get-node-info@main
  id: node-info

- run: echo "Version ${{ steps.node-info.outputs.root-package-version }}"
```

## Git clone

Clones a repository with configurable depth and branch.

| Input         | Description             | Default      |
| ------------- | ----------------------- | ------------ |
| `repo`        | Repository (owner/repo) | current repo |
| `depth`       | Clone depth             | `1`          |
| `branch`      | Branch to clone         | (required)   |
| `destination` | Target directory        | (required)   |

```yml
- uses: taiga-family/actions/git-clone@main
  with:
    branch: main
    destination: ./repo
```

## Git rebase

Rebases the current branch onto the base branch. Aborts on merge conflicts.

```yml
- uses: taiga-family/actions/git-rebase@main
```

## Git status

Checks if the working tree has modifications.

| Output     | Description                                    |
| ---------- | ---------------------------------------------- |
| `modified` | `true` if there are changes, `false` otherwise |

```yml
- uses: taiga-family/actions/git-status@main
  id: git-status

- run: echo "Modified: ${{ steps.git-status.outputs.modified }}"
```

## Git sync submodule

Synchronizes all git submodules by reinitializing and updating to latest remote commits.

```yml
- uses: taiga-family/actions/git-sync-submodule@main
```

## HTTP serve

Starts a local web server for a given directory. Optionally replaces `<base href>` in `index.html`.

| Input                | Description                 | Default    |
| -------------------- | --------------------------- | ---------- |
| `protocol`           | Protocol                    | `http`     |
| `port`               | Port                        | (required) |
| `directory`          | Directory to serve          | (required) |
| `replaceBaseUrl`     | Enable base URL replacement | `true`     |
| `replaceBaseUrlFrom` | Original base URL           |            |
| `replaceBaseUrlTo`   | Replacement base URL        |            |

```yml
- uses: taiga-family/actions/http-serve@main
  with:
    port: 3333
    directory: dist/demo/browser
```

## Messenger telegram announce

Sends a release announcement to a Telegram chat.

| Input       | Description                       | Default    |
| ----------- | --------------------------------- | ---------- |
| `chatId`    | Telegram chat ID                  | (required) |
| `token`     | Telegram bot token                | (required) |
| `textLink`  | Text to display                   | (required) |
| `version`   | Version to announce               | (required) |
| `topicId`   | Telegram topic ID                 |            |
| `parseMode` | Parse mode (`html` or `markdown`) | `markdown` |

```yml
- uses: taiga-family/actions/messenger-telegram-announce@main
  with:
    chatId: ${{ secrets.TELEGRAM_CHAT_ID }}
    token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
    textLink: '@taiga-ui/core'
    version: ${{ needs.release.outputs.version }}
```

## Messenger mattermost announce

Sends a release announcement to a Mattermost/webhook channel.

| Input      | Description     | Default         |
| ---------- | --------------- | --------------- |
| `url`      | Webhook URL     | (required)      |
| `version`  | Release version | (required)      |
| `username` | Display name    | `Taiga release` |
| `channel`  | Channel         | (required)      |

```yml
- uses: taiga-family/actions/messenger-mattermost-announce@main
  with:
    url: ${{ secrets.MATTERMOST_WEBHOOK }}
    version: ${{ needs.release.outputs.version }}
    channel: releases
```

## NPM install

Performs a clean install with `package-lock.json` regeneration and workspaces support.

```yml
- uses: taiga-family/actions/npm-install@main
```

## Playwright screenshot diff

Combines Playwright failed screenshot pairs (actual + expected) into a single diff image for easier review.

```yml
- uses: taiga-family/actions/playwright-screenshot-diff@main
```

## Playwright diff check

Checks for Playwright visual regression diff images and fails the workflow if any are found.

| Input       | Description                       | Default    |
| ----------- | --------------------------------- | ---------- |
| `pw-result` | Path to Playwright results folder | (required) |

```yml
- uses: taiga-family/actions/playwright-diff-check@main
  with:
    pw-result: test-results
```

## Read package.json

Reads `name` and `version` from a `package.json` file.

| Input  | Description          | Default          |
| ------ | -------------------- | ---------------- |
| `path` | Path to package.json | `./package.json` |

| Output    | Description     |
| --------- | --------------- |
| `name`    | Package name    |
| `version` | Package version |

```yml
- uses: taiga-family/actions/read-package-json@main
  id: pkg

- run: echo "Version ${{ steps.pkg.outputs.version }}"
```

## Release it

Runs `release-it` to create a new version. Detects conventional commits and bumps version accordingly.

| Input         | Description                                                       | Default     |
| ------------- | ----------------------------------------------------------------- | ----------- |
| `ref`         | GitHub ref                                                        | current ref |
| `githubToken` | GitHub token                                                      | (required)  |
| `npmToken`    | NPM token                                                         |             |
| `mode`        | Increment mode (`patch`, `minor`, `major`, `alpha`, `prerelease`) |             |
| `flags`       | Custom release-it flags                                           | `''`        |

| Output     | Description                     |
| ---------- | ------------------------------- |
| `released` | Whether the version was updated |

```yml
- uses: taiga-family/actions/release-it@main
  with:
    githubToken: ${{ secrets.GITHUB_TOKEN }}
    mode: minor
```

## Security codeql

Runs GitHub CodeQL static analysis for JavaScript.

```yml
- uses: taiga-family/actions/security-codeql@main
```

## Security dependency review

Reviews dependency changes in a pull request for known vulnerabilities.

| Input      | Description                 | Default    |
| ---------- | --------------------------- | ---------- |
| `severity` | Minimum severity to fail on | `critical` |

```yml
- uses: taiga-family/actions/security-dependency-review@main
  with:
    severity: high
```

## Setup git

Configures git user name, email, and remote URL with token-based auth.

| Input   | Description    | Default                                                 |
| ------- | -------------- | ------------------------------------------------------- |
| `name`  | Git user name  | `taiga-family-bot`                                      |
| `email` | Git user email | `41898282+github-actions[bot]@users.noreply.github.com` |
| `token` | GitHub token   | (required)                                              |

```yml
- uses: taiga-family/actions/setup-git@main
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
```

## Setup cypress

Installs Cypress with browser caching.

| Input     | Description    | Default    |
| --------- | -------------- | ---------- |
| `version` | Custom version | (required) |

```yml
- uses: taiga-family/actions/setup-cypress@main
  with:
    version: 13.0.0
```

## Setup node

Sets up Node.js with cached `node_modules` and Nx cache. Optionally runs `npm audit` on pull requests.

| Input                | Description                  | Default                             |
| -------------------- | ---------------------------- | ----------------------------------- |
| `node-version`       | Node.js version              | `24.x`                              |
| `node-registry`      | Registry URL                 | `https://registry.npmjs.org`        |
| `npm-ci-flags`       | Additional npm ci flags      |                                     |
| `node-path`          | Paths for node_modules cache | `node_modules/`, `**/node_modules/` |
| `nx-path`            | Paths for Nx cache           | `.angular/`, `.nx/`                 |
| `validate-peer-deps` | Validate peer dependencies   | `true`                              |
| `audit`              | Run npm audit on PR          | `false`                             |

| Output                       | Description                               |
| ---------------------------- | ----------------------------------------- |
| `root-package-version`       | Full version of root package.json (x.y.z) |
| `root-package-major-version` | Only major version (x)                    |
| `is-release-candidate`       | Whether version includes -rc              |

```yml
- uses: taiga-family/actions/setup-node@main
  with:
    node-version: 22.x
```

## Setup npm

Configures npm registry token and validates it.

| Input   | Description | Default    |
| ------- | ----------- | ---------- |
| `token` | NPM token   | (required) |

```yml
- uses: taiga-family/actions/setup-npm@main
  with:
    token: ${{ secrets.NPM_TOKEN }}
```

## Setup playwright

Installs Playwright browsers with system dependencies and caching.

| Input     | Description         | Default    |
| --------- | ------------------- | ---------- |
| `version` | Custom version      | (required) |
| `deps`    | Browsers to install | `chromium` |

```yml
- uses: taiga-family/actions/setup-playwright@main
  with:
    version: 1.40.0
```

## Setup project

Adds an issue or pull request to a GitHub organization project.

| Input          | Description         | Default        |
| -------------- | ------------------- | -------------- |
| `token`        | GitHub token        | (required)     |
| `organization` | GitHub organization | `taiga-family` |
| `projectId`    | Project number      | `1`            |

```yml
- uses: taiga-family/actions/setup-project@main
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
```

## Setup variables

Sets up global CI environment variables and prints diagnostic info about the current run.

```yml
- uses: taiga-family/actions/setup-variables@main
```

**Environment variables provided:** `TUI_CI`, `NX_BRANCH`, `IS_FORK`, `IS_DEPENDABOT`, `IS_OWNER_MODE`,
`IS_MAIN_BRANCH`, `IS_RELEASE_BRANCH`, `SUPPORT_AUTO_PUSH`, `DIST`, `DIST_NEXT`, `REPO`, `NG_SERVER_PORT`,
`CI_COMMIT_SHA`, `CACHE_DIST_KEY`, `CYPRESS_SNAPSHOTS_ARTIFACTS_KEY`, `PLAYWRIGHT_SNAPSHOTS_ARTIFACTS_KEY`,
`PLAYWRIGHT_BLOB_ARTIFACTS_KEY`.

## Wait job

Waits for a specific GitHub Actions check to complete (polls every 60s, 3h timeout).

| Input   | Description            | Default    |
| ------- | ---------------------- | ---------- |
| `token` | GitHub token           | (required) |
| `job`   | Check name to wait for | (required) |

```yml
- uses: taiga-family/actions/wait-job@main
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
    job: ci
```
