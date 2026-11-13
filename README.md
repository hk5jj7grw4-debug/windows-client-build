# Windows Client Build

This public repository contains a manually dispatched Windows build workflow only.
Application source, packaging scripts, configuration, and release notes remain in a separate private repository.

The workflow checks out a version tag using a read-only deploy key, builds it on a standard Windows runner,
and uploads verified packages directly to the configured private ReleaseHub service.
It does not create GitHub Releases, commit binaries, upload Actions artifacts, or use shared build caches.
Private compiler and packaging output is kept in temporary runner files and removed after the job.
Public Actions logs still show workflow steps, status, version tags, and sanitized failure codes.

## Configuration

Repository Secrets:

| Secret | Purpose |
| --- | --- |
| `SOURCE_REPOSITORY` | Private repository in `owner/repository` format |
| `SOURCE_READ_KEY` | SSH deploy key with read-only access to that repository |
| `RELEASEHUB_BASE_URL` | Private distribution service URL |
| `RELEASEHUB_CI_TOKEN` | Upload credential scoped to the application |
| `RELEASEHUB_APP_TOKEN` | Application update/download credential embedded in the client |
| `RELEASEHUB_ROBOT_MANAGER_TOKEN` | Component-manager download credential embedded in the client |

The private repository provides `scripts/Publish-ReleaseHub.ps1`.
Only repository writers can dispatch this workflow. Pull requests and forks do not trigger builds.

## Build

Run **Build Windows Client** from the Actions page and supply an existing source tag, such as `v1.2.3`.
A successful job means both installer and portable packages have been cached and checksum-verified by ReleaseHub.
