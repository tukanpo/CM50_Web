# CM50 WebGL deployment

This public repository contains only the automation used to build and publish
the CM50 WebGL player. The private CM50 source and developer documentation are
not mirrored here.

The workflow accepts a full commit SHA from `Tukanpo/cm50`, requires it to
match the current `main` tip, checks out exactly that commit with read-only
GitLab deploy credentials, builds the Unity WebGL player, and publishes only
the generated player to GitHub Pages.

## Required release-build environment

Create an environment named `cm50-release-build`, restrict its deployment
branches to `main`, and add these environment secrets:

- `GITLAB_DEPLOY_USER`: username of a GitLab deploy token scoped to
  `Tukanpo/cm50` with `read_repository` only
- `GITLAB_DEPLOY_TOKEN`: matching GitLab deploy token
- `UNITY_SERIAL`: Unity Personal serial used by the existing release pipeline
- `UNITY_EMAIL`: Unity ID email
- `UNITY_PASSWORD`: Unity ID password

The workflow does not run for pull requests or ordinary pushes, and the build
job rejects refs other than `main`. Initially it can only be started manually
by a repository maintainer with a full 40-character CM50 commit SHA. A later
migration step will connect the GitLab release pipeline through a
`publish-webgl` `repository_dispatch` event.

The Unity `Library` directory is deliberately not cached because Actions
caches in a public repository are not an appropriate place for derived data
from the private CM50 source.

GitHub Pages must use **GitHub Actions** as its publishing source. The expected
project-site URL is `https://tukanpo.github.io/CM50_Web/`.
