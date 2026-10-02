# External scheduler: cron-job.org

Status: prepared, not yet activated. Account login and a dedicated GitHub credential are required. Do not put credentials in this repository or chat.

## Credential

In GitHub Settings > Developer settings > Personal access tokens > Fine-grained tokens, create a token restricted to owner `zepandasanv2` and the single repository `zepandasan-anilist-export`. Grant repository permission **Actions: Read and write**; leave other optional permissions disabled. Choose an expiration and renew it before expiry. This permission allows workflow management within that repository, not just this one dispatch. Enter the token directly in the scheduler's Authorization header. Do not reuse the broadly scoped GitHub CLI credential.

## Job configuration

- Title: ZePandaSan weekly AniList export
- URL: `https://api.github.com/repos/zepandasanv2/zepandasan-anilist-export/actions/workflows/update-anilist.yml/dispatches`
- Method: POST
- Timezone: UTC (not Europe/Paris)
- Schedule: `0 5 * * 5` (Friday 05:00 UTC; 07:00 Paris in summer, 06:00 in winter)
- Request body: `{"ref":"main"}`
- Headers:
  - `Authorization: Bearer <dedicated fine-grained token>`
  - `Accept: application/vnd.github+json`
  - `Content-Type: application/json`
  - `X-GitHub-Api-Version: 2022-11-28`
  - `User-Agent: zepandasan-anilist-export-scheduler`

Create the job disabled first. Use the service's test execution, confirm an accepted response (normally HTTP 204 with this API version), then check GitHub Actions for a new successful workflow_dispatch run. A 2xx response alone does not prove the CSV export succeeded. Enable the weekly job after this validation, record its job ID here (never its credential), and confirm the next execution time in the service.

## Monitoring and recovery

Check both cron-job.org request history and GitHub Actions run results. A dispatch may succeed while the workflow later fails. Use failure notifications in cron-job.org and GitHub as desired. For 401/403, check token expiration, repository selection and Actions permission; for 404, check URL and access; for 422, check main and workflow_dispatch.

After activation, remove the GitHub native schedule from update-anilist.yml to avoid duplicate launches. Manual execution remains available. If abandoning this workaround, disable the external job, revoke its dedicated token and restore `0 5 * * 5` in the GitHub workflow.

Sources: [cron-job.org API](https://docs.cron-job.org/rest-api.html), [GitHub workflow dispatch API](https://docs.github.com/en/rest/actions/workflows#create-a-workflow-dispatch-event).
