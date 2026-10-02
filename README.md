# ZePandaSan AniList exports

Automated CSV exports of the public AniList anime list for **ZePandaSan**, generated with [AniFetch](https://github.com/Liyfez/Anifetch).

GitHub Actions refreshes the files every **Friday at 05:00 UTC** (`0 5 * * 5`), or 06:00 in winter / 07:00 in summer in Paris. Scheduled runs may be delayed by GitHub.

## Data

All generated CSV files have stable names in `data/`:

- [Anime list](data/ZePandaSan_anime_list.csv)
- [Genre breakdown](data/ZePandaSan_genre_breakdown.csv)
- [Studio breakdown](data/ZePandaSan_studio_breakdown.csv)

## Run manually

Open **Actions > Update AniList data > Run workflow**, select `main`, and click **Run workflow**.

To export locally with Node.js 22:

```sh
mkdir -p data
npx --yes @l1e/anifetch ZePandaSan --all --csv -o ./data
```

No AniList token is needed for this public profile.

The workflow validates fresh CSV output before staging `data/`. Failed exports or validation never commit. Changed data is committed to `main` by `github-actions[bot]` with the message `chore: update AniList data`. Unchanged data produces no commit. Pushes never use force.

The workflow requests `contents: write` for its GitHub token. If an organization policy denies this permission, its administrator must allow repository-content writes for GitHub Actions. Branch protection may also need to permit the workflow's push.

GitHub can disable scheduled workflows in public repositories after 60 days without repository activity. If this happens, re-enable the workflow in Actions.

## Temporary hourly test branch

This branch uses `17 * * * *` (once per hour at minute 17). Checkout and automatic commits target the branch that runs the workflow. Manual execution can select this branch.

GitHub only triggers scheduled workflows on the default branch. This hourly schedule will not run automatically while this remains a non-default test branch. To test the scheduler, the schedule must be applied to the default branch. Restore the Friday schedule after testing.

