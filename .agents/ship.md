Ship this thread's changes as a jj-fork series in ajac-zero/hermes-agent.
Load maintaining-forks-with-jj-fork and read AGENTS.md before version-control work.

Choose patch/<name> for upstreamable code or tooling/agent-workflow for fork-only
agent configuration. If the intended series is unclear, stop and ask the owner.
Never commit directly onto generated fork/main or the main upstream mirror.

Preserve the thread's changes before switching revisions. For a new feature, start
with jj fork create patch/<name> -m 'Description'; continue an existing series with
jj new <series>. Transfer only the intended changes onto that upstream-rooted
series; do not carry unrelated fork patches or other people's work into it.
Describe the completed commit and set its series bookmark to the completed tip.

Verify the series alone, not on fork/main. Follow the area's AGENTS.md: Python uses
scripts/run_tests.sh with the affected tests; JS uses the affected workspace's
typecheck, tests, lint, and build. Render and inspect changed UI states. For
agent-workflow-only changes, validate shell syntax, run setup twice, and verify
the fork tooling. Fix failures introduced by the series; report upstream failures
and limitations honestly. Do not ship a series that needs another patch to pass.

Then run jj fork assemble --save-plan /tmp/hermes-ship-plan.json and immediately
jj fork apply /tmp/hermes-ship-plan.json --push in the same checkout. Ship authorizes
publishing this series and the resulting fork through jj-fork's guarded push, not
deployments, releases, remote deletions, or rewriting unrelated branches. Do not
perform intervening jj operations between saving and applying. For exit 20, use
isolated repair tasks; do not bypass checks or force-push. Report the published
series, verification results, and resulting fork/main.
