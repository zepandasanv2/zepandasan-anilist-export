# ZePandaSan AniList exports

Automated CSV exports of the public AniList anime list for **ZePandaSan**, generated with [AniFetch](https://github.com/Liyfez/Anifetch).

cron-job.org triggers the GitHub Actions export every **Friday at 05:00 UTC** (`0 5 * * 5`): 07:00 in Paris during summer, 06:00 during winter. The external job is enabled, and its test dispatch and resulting export succeeded on 2026-10-02. The first unattended weekly execution is still pending.

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

## Scheduling incident and workaround

See [the incident record](docs/scheduler-incident-2026-10-02.md) for observations and limitations, and [cron-job.org setup](docs/external-scheduler.md) for the external dispatch configuration.

Hourly tests have ended. Native GitHub schedules were removed to avoid duplicate dispatches. Manual execution remains available for both workflows.
