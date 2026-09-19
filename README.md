# base-image

Native amd64 and arm64 base images derived from [hotio/base](https://github.com/hotio/base), retaining its GPL license, runtime layout and VPN services.

The `workflows` branch contains shared build tooling. `alpinevpn` and `noblevpn` contain their respective Dockerfiles, pinned upstream metadata and runtime files. Documentation belongs at [web.edb.fi](https://web.edb.fi/containers/base-image); that site is prepared separately.

## Validation and publishing

`ci / required` requires workflow/metadata checks and real native builds of both image variants on both architectures. Builds load locally and exercise startup, configured UID, `/config` ownership and required tools. Artifacts retain package inventories and smoke logs. The default smoke deliberately disables VPN, Privoxy and Unbound; it does not certify provider connectivity or privileged VPN routing.

The **Publish tested image** workflow is manual. Run it from the image branch (`alpinevpn` or `noblevpn`) and select that same image branch. Both architectures are rebuilt and smoke-tested before any registry upload. Publishing requires successful final-branch CI and checks that the workflow, source branch and tested revision match. It publishes `ghcr.io/edbfi/base-image:<branch>` and a commit-qualified alias; version aliases apply when metadata declares a version. Pull-request CI has no registry write permission. Initial publication is part of repository migration validation; the existence of this configuration alone does not establish that tags are available.

The **Prepare image metadata update** workflow produces a patch and recovery bundle only. Review and apply the candidate on a feature branch, open a PR, and require full image CI before merging. It does not push to any branch. No hourly mutation, website write, account-wide maintenance or external notification is performed by these workflows.

Upstream synchronization is prepared by `edbfi/repo-patches` from the explicit `.upstream.json` revision. Conflicts require review; synchronization never replaces published branch history. Keep the upstream license, internal `hotio` runtime account and attribution intact.

Shared Renovate defaults use `edbfi/automation` at `v3.0.1`, with native PR automerge behind required CI. Run local validation with `python3 -m unittest discover -s tools -p 'test_*.py'`, `actionlint`, and `shellcheck tools/smoke.sh`.

Shared CI and Renovate presets use automation `v3.0.1`. Renovate owns dependency
PR merging through its native rebase strategy, preserving commit author sign-offs.
Strict, GitHub Actions-sourced required CI and PR policy checks must pass on an
up-to-date branch; the automated merger has no bypass. The read-only PR policy
check preserves sign-offs, Conventional Commit titles, reviews and hold labels.
Independent policy events run to completion without cancelling one another.
The shared release-age policy remains active, and Renovate configuration updates
require manual merging. The custom checked merger remains retired.
Native architecture builds and every existing container smoke assertion remain
mandatory; image publication remains an explicit manual operation after CI.
