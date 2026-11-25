# Shared CI standards — Pages build pilot (proposal)

The seven Wave 1 product Pages workflows currently share identical blob 52cd1791f68bfe61ed93fbd7b9c7e8a0b9d8e5c0. The reusable workflow extracts only the build and artifact upload steps. Core CI, React build checks, browser acceptance, deployment permissions, environment and concurrency stay with callers.

## Security and acceptance
- No input shell interpolation, secrets inheritance or deployment permission. Caller contents: read; checkout credentials not persisted.
- Only pull_request, push to main and authorized manual dispatch callers are supported. Never use pull_request_target or privileged untrusted code execution.
- Action references resolve to immutable commits of the existing major tags. Their action.yml entrypoints were inspected. The Pages archive follows the official upload-pages-artifact v3 layout (artifact.tar inside github-pages artifact); direct SHA-pinned upload-artifact avoids the composite's floating nested action. The workflow rejects symlinks, hardlinks and special files. Dependency review remains required; no security certification.
- The fixed build entrypoint executes caller repository code on a GitHub-hosted runner. Outputs must include index.html and contain no symbolic links.
- Keep existing caller deployment job and its pages: write/id-token: write permissions. Do not add enablement or admin tokens to the workflow. Check Settings → Pages → Source: GitHub Actions before rollout; missing site errors must be classified as configuration/permission failures, not build failures.
- Before pilot, verify workflow syntax with actionlint, action source/security review, caller access, required-check migration and exact-head baseline. Use one existing product only, reference the merged reviewed workflow commit SHA, retain all core tests and deployment restrictions. Nested reusable job check names may differ; coordinate required checks before migration.
- Verify actual PR build and main build/artifact/deploy, HTTP routes and assets, and compare prior acceptance. Roll back to original workflow if any check fails. No migration to other products until the pilot is accepted.

## Pilot status
No caller changed. Shared workflow CI uses actionlint 1.7.12 with the published release SHA-256 digest; actual CI and live caller deployment must be verified separately. Reusable build alone cannot verify repository Pages settings. The existing successful product deployments must remain untouched until reviewed migration.
