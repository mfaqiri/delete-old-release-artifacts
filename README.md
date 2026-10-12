# Delete Old Release Artifacts

A GitHub Action that removes assets from old releases while keeping the releases themselves. Tags, titles, and changelogs are never deleted, only the attached files.

## How it works

The action looks at all releases in the repository.

- The newest releases, based on keep-latest, are always left alone.
- Any release outside that group is checked against max-age-days.
- If it is older than that, its assets are deleted.
- If it is not old enough yet, it is left alone.
- The release itself is never deleted, only its assets.

## Usage

Add this to a workflow file, for example .github/workflows/cleanup.yml

```yaml
name: Cleanup Old Release Artifacts

on:
  schedule:
    - cron: "0 15 1 1,4,7,10 *"
  workflow_dispatch:

permissions:
  contents: write

jobs:
  cleanup:
    runs-on: ubuntu-latest
    steps:
      - uses: your-username/delete-old-release-artifacts@v1
        with:
          keep-latest: "10"
          max-age-days: "90"
          dry-run: "true"
          token: ${{ secrets.GITHUB_TOKEN }}
