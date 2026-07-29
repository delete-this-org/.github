# YOUR_ORG/.github — centralized CI/CD

The org-level `.github` repo. Anything **identical across repos AND governed as
policy** lives here so it's edited in one place. Repo-specific things stay in
each service repo (see `sample-service-scaffold/`).

## What's centralized here

| Path                                         | Type              | Why centralized                                                |
| -------------------------------------------- | ----------------- | -------------------------------------------------------------- |
| `.github/workflows/reusable-branch-name.yml` | Reusable workflow | Pure policy — one regex org-wide                               |
| `.github/workflows/reusable-ai-review.yml`   | Reusable workflow | Tune prompt + security posture once for all repos              |
| `actions/setup-dotnet/action.yml`            | Composite action  | Shared setup _steps_ (SDK + restore) reused by build/test jobs |
| `CODEOWNERS`                                 | Config            | Org-wide default reviewer fallback                             |

## How consumers reference these

- Reusable workflow: `YOUR_ORG/.github/.github/workflows/reusable-ai-review.yml@main`
  (the double `.github` is correct: repo name is `.github`, path is `.github/workflows/...`)
- Composite action: `YOUR_ORG/.github/actions/setup-dotnet@main`


## What is NOT here (stays per-repo)

Build/test _commands_, Docker build, deploy workflows + environment gating, and
platform-specific builds — those are repo-specific. See the
service repo's README for the split rationale.
