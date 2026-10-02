# ZePandaSan AniList exports

Automated CSV exports of the public AniList anime list for **ZePandaSan**, generated with [AniFetch](https://github.com/Liyfez/Anifetch).

During the temporary scheduler test, GitHub Actions refreshes the files **every hour at minute 17** (`17 * * * *`). Scheduled runs may be delayed by GitHub. The normal Friday schedule (`0 5 * * 5`, 05:00 UTC) will need to be restored after testing.

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

## Temporary hourly production test

The hourly schedule was enabled on main on 2026-10-02 to diagnose missing scheduled runs. Check Actions for runs with the schedule event; manual runs do not validate the scheduler. Checkout and automatic commits target the branch running the workflow.

After testing, restore the cron to `0 5 * * 5` and update this README. The hourly test remains active until that change is published.
