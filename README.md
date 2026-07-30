# YOUR_ORG/.github — centralized CI/CD

The org-level `.github` repo. Anything **identical across repos AND governed as
policy** lives here so it's edited in one place. Repo-specific things stay in
each service repo (see `sample-repo`).

## What's centralized here

| Path                                         | Type              | Why centralized                                                |
| -------------------------------------------- | ----------------- | -------------------------------------------------------------- |
| `.github/workflows/reusable-branch-name.yml` | Reusable workflow | Pure policy — one regex org-wide. Exempts `release-please--*`   |
| `.github/workflows/reusable-ai-review.yml`   | Reusable workflow | Tune prompt + security posture once for all repos              |
| `actions/setup-dotnet/action.yml`            | Composite action  | Shared setup _steps_ (SDK + restore) reused by build/test jobs |
| `CODEOWNERS`                                 | Config            | Org-wide default reviewer fallback                             |
| `examples/`                                  | Reference only    | Starter workflows for the other two repo shapes — **not active** |

The `ai-review` gate reads each consumer repo's `.github/review-guidelines.md`
for its rubric — deliberately not `CLAUDE.md`, which Claude Code claims as its own
interactive project-context file. The rubric stays per-repo; only the gate is
centralized.

## examples/ — the other repo shapes

`sample-repo` demonstrates the **microservice** shape. These are the other two,
kept here as reference because they have no home repo yet. They sit outside
`.github/workflows/`, so they never run from this repo — copy them into the target
repo's `.github/workflows/`.

| Path | Shape | Purpose |
|---|---|---|
| `examples/shared-library/main.yml` | Shared library | Build + pack on merge. Publishes nothing |
| `examples/shared-library/release-publish.yml` | Shared library | Publish stable version, gated by the `packages` environment |
| `examples/android/main.yml` | Android app | Unsigned AAB on merge, as a run artifact |
| `examples/android/release-android.yml` | Android app | Signed AAB attached to the release |

## How consumers reference these

- Reusable workflow: `YOUR_ORG/.github/.github/workflows/reusable-ai-review.yml@main`
  (the double `.github` is correct: repo name is `.github`, path is `.github/workflows/...`)
- Composite action: `YOUR_ORG/.github/actions/setup-dotnet@main`


## What is NOT here (stays per-repo)

Build/test _commands_, Docker build, deploy workflows + environment gating, and
platform-specific builds — those are repo-specific. See the
service repo's README for the split rationale.
