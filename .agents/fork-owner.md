# Hermes fork owner

Maintain `ajac-zero/hermes-agent` against `NousResearch/hermes-agent` main.
Load `maintaining-forks-with-jj-fork` and follow `AGENTS.md`. A schedule running
this prompt needs the owner's explicit permission for recurring guarded pushes.
Do not change application behavior, retire series, delete remote branches, publish
releases, or deploy. `fork/main` is generated; never edit it directly.

Run `jj fork sync --save-plan /tmp/hermes-sync-plan.json --report /tmp/hermes-sync-report.json`.
On exit 0, inspect the report to determine which series changed. Verify their
relevant builds/tests on isolated checkouts of
the candidate series **before** publishing when code changed: no universal checks
are configured in `.jj-fork.toml`. Use `scripts/run_tests.sh` for Python and the
affected JS workspace's checks/build; follow the area's guidance. If verification
cannot run or fails due to the fork, stop and report rather than publish blindly.
Do verification against candidate commits without changing the owner's jj state;
plans are bound to this checkout and become stale after jj operations.
Then run `jj fork apply /tmp/hermes-sync-plan.json --push` in this same checkout,
without intervening jj operations. If the plan is stale, save and verify a new one.

On exit 20, read the report's issues. The owner, in this clone, runs
`jj fork repair start` for each issue, with explicit allowed paths for check
failures. Assign each issue to one fixer using the reported low/medium/high tier;
retry once one tier higher if needed. The fixer works only in that task repository,
preserving commit topology and descriptions, snapshots with jj, and returns the
whole directory. Fixers never start tasks, see the owner's plan/key, or push.

For another orb, archive the complete task (including `.git` and `.jj`), transfer
it using Amp file-transfer tools (large archives via `thread_file_url`), and have
the fixer return a complete archive. Unpack into a fresh directory. The owner's
clone runs `jj fork repair submit <task directories> --save-plan /tmp/hermes-next-plan.json`,
then immediately applies a ready plan with `--push`. Handle remaining glue issues
the same way. Test this archive handoff against a disposable local fixture before
enabling automatic repairs; until then, report conflicts to the owner.

On exit 1 or an uncertain network outcome, inspect local/remote state before any
retry. Report failures without force-pushing or suppressing failed checks. Summarize
upstream movement, series checks, published refs, repairs, and any owner action.
