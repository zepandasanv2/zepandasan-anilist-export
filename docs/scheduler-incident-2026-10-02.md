# Incident: scheduled workflows did not produce runs

Date: 2026-10-02. Status: unresolved; external scheduling workaround pending activation.

## Observations (UTC)

- The initial manual export succeeded on 2026-10-01 at 15:46:46.
- The Friday schedule `0 5 * * 5` was expected on 2026-10-02 at 05:00. No schedule run was visible during our checks.
- A manual export at 10:20:52 succeeded: https://github.com/zepandasanv2/zepandasan-anilist-export/actions/runs/36994963247
- An hourly test (`17 * * * *`) was published on main before 11:17. No schedule run was visible at 11:47.
- A separate diagnostic workflow containing only `date -u` succeeded manually at 11:51:37: https://github.com/zepandasanv2/zepandasan-anilist-export/actions/runs/37003332824
- Its cron was changed to `0 * * * *` before 12:00. At 12:06, no scheduled run was visible. This short observation does not prove that this particular run was permanently dropped.

The workflows were active on main (the default branch); Actions was enabled; the repository was neither archived nor a fork. The cron expressions are valid. Manual success verifies execution but does not verify scheduler delivery. We have not established the root cause, a platform outage, or a permanent scheduler defect.

## References

- Official documented scheduling limitations: https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule
- Similar first-hand report, 2026-09-29 (not a confirmed diagnosis of this repository): https://github.com/orgs/community/discussions/209036
- Minimal reproduction reported by another user: https://github.com/orgs/community/discussions/202919

## Mitigation and verification

Prepare cron-job.org to send workflow_dispatch every Friday at 05:00 UTC. See [external scheduler setup](external-scheduler.md). A successful HTTP request only verifies that GitHub accepted a dispatch; the resulting Actions run must also succeed. External runs appear as workflow_dispatch, not schedule.

Keep the original Friday GitHub schedule as a fallback while the external scheduler is pending. Hourly export tests have ended; the minimal diagnostic remains available manually. When the external scheduler is validated, remove the native Friday schedule to avoid duplicate weekly dispatches. Do not call the workaround operational until a real cron-job.org request and the corresponding Actions run have been verified.
