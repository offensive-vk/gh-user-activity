# GitHub Activity in Readme

Fetch and Updates Your Readme Markdown with the Recent GitHub activity of a github user.

## Instructions

- Add the comment `<!--START_SECTION:activity-->` (entry point) within `README.md`.
- Add the comment `<!--END_SECTION:activity-->` (exit point) withing the  `README.md`.

```yml
name: Update Activity
on:
  schedule:
    - cron: "30 1 * * *"
  workflow_dispatch:
jobs:
  build:
    name: Update Recent Activity
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - uses: actions/checkout@v4
      - uses: offensive-vk/auto-user-activity@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

The above job runs every half an hour, you can change it as you wish based on the [cron syntax](https://jasonet.co/posts/scheduled-actions/#the-cron-syntax).

Please note that only those public events that belong to the following list show up:-

- `IssueEvent`
- `ReleaseEvent`
- `IssueCommentEvent`
- `PullRequestEvent`

You can find an example [here](https://github.com/offensive-vk/auto-user-activity/blob/master/.github/workflows/test.yml).

### Override defaults

Use the following `input params` to customize it for your use case:-

| Input Param        | Default Value                                                            | Description                                               |
| ------------------ | ------------------------------------------------------------------------ | --------------------------------------------------------- |
| `GH_USERNAME`      | Your GitHub username                                                     | Username for which to generate the activity overview      |
| `COMMIT_NAME`      | github-actions[bot]                                                      | Name of the committer                                     |
| `COMMIT_EMAIL`     | 41898282+github-actions[bot]@users.noreply.github.com                    | Email of the committer                                    |
| `COMMIT_MSG`       | :zap: Update README with the recent activity                             | Commit message used while committing to the repo          |
| `EMPTY_COMMIT_MSG` | :memo: empty commit to keep workflow active after 60 days of no activity | Commit message used when there are no updates             |
| `MAX_LINES`        | 5                                                                        | The maximum number of lines populated in your readme file |
| `TARGET_FILE`      | README.md                                                                | The file to insert recent activity into                   |

```yml
name: Update Readme
on:
  schedule:
    - cron: "0 */2 * * *"
  workflow_dispatch:
jobs:
  build:
    name: Update Activity
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - uses: actions/checkout@v4
      - uses: offensive-vk/auto-user-activity@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          COMMIT_MSG: "Specify a custom commit message"
          MAX_LINES: 10
          COMMIT_NAME: GitHub Activity Readme
```
