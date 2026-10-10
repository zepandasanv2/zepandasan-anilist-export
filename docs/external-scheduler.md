# External scheduler: cron-job.org

Status: enabled on 2026-10-02. cron-job.org job ID: 8563840. Test request returned HTTP 204 at 12:28:14 UTC; the resulting export succeeded: https://github.com/zepandasanv2/zepandasan-anilist-export/actions/runs/37006861547 . On 2026-10-02, the user changed the schedule to once per hour at minute 00. Four hourly workflow_dispatch exports succeeded on 2026-10-02 at 13:01:13, 14:01:13, 15:01:16 and 16:01:17 UTC; latest run: https://github.com/zepandasanv2/zepandasan-anilist-export/actions/runs/37031047325 . The GitHub response reports token expiration on 2026-12-31 at 13:21:32 UTC; renew it before then. Do not put credentials in this repository or chat.

## Credential

In GitHub Settings > Developer settings > Personal access tokens > Fine-grained tokens, create a token restricted to owner `zepandasanv2` and the single repository `zepandasan-anilist-export`. Grant repository permission **Actions: Read and write**; leave other optional permissions disabled. Choose an expiration and renew it before expiry. This permission allows workflow management within that repository, not just this one dispatch. Enter the token directly in the scheduler's Authorization header. Do not reuse the broadly scoped GitHub CLI credential.

## Job configuration

- Title: ZePandaSan hourly AniList export
- URL: `https://api.github.com/repos/zepandasanv2/zepandasan-anilist-export/actions/workflows/update-anilist.yml/dispatches`
- Method: POST
- Timezone: UTC (not Europe/Paris)
- Schedule: `0 * * * *` (every hour at minute 00)
- Request body: `{"ref":"main"}`
- Headers:
  - `Authorization: Bearer <dedicated fine-grained token>`
  - `Accept: application/vnd.github+json`
  - `Content-Type: application/json`
  - `X-GitHub-Api-Version: 2022-11-28`
  - `User-Agent: zepandasan-anilist-export-scheduler`

Create the job disabled first. Use the service's test execution, confirm an accepted response (normally HTTP 204 with this API version), then check GitHub Actions for a new successful workflow_dispatch run. A 2xx response alone does not prove the CSV export succeeded. Enable the hourly job after this validation, record its job ID here (never its credential), and confirm the next execution time in the service.

## Monitoring and recovery

Check both cron-job.org request history and GitHub Actions run results. A dispatch may succeed while the workflow later fails. Use failure notifications in cron-job.org and GitHub as desired. For 401/403, check token expiration, repository selection and Actions permission; for 404, check URL and access; for 422, check main and workflow_dispatch.

The native GitHub schedule has been removed from update-anilist.yml to avoid duplicate launches. Manual execution remains available. If abandoning this workaround, disable the external job, revoke its dedicated token and restore `0 5 * * 5` in the GitHub workflow.

Sources: [cron-job.org API](https://docs.cron-job.org/rest-api.html), [GitHub workflow dispatch API](https://docs.github.com/en/rest/actions/workflows#create-a-workflow-dispatch-event).
