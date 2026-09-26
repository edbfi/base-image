# base-image

Native amd64 and arm64 base images derived from [hotio/base](https://github.com/hotio/base), retaining its GPL license, runtime layout and VPN services.

The `workflows` branch contains shared build tooling. `alpinevpn` and `noblevpn` contain their respective Dockerfiles, pinned upstream metadata and runtime files. Documentation belongs at [web.edb.fi](https://web.edb.fi/containers/base-image); that site is prepared separately.

## Validation and publishing

`ci / required` requires workflow/metadata checks and real native builds of both image variants on both architectures. Builds load locally and exercise startup, configured UID, `/config` ownership and required tools. Artifacts retain package inventories and smoke logs. The default smoke deliberately disables VPN, Privoxy and Unbound; it does not certify provider connectivity or privileged VPN routing.

The **Publish tested image** workflow is manual. Run it from the image branch (`alpinevpn` or `noblevpn`) and select that same image branch. Both architectures are rebuilt and smoke-tested before any registry upload. Publishing requires successful final-branch CI and checks that the workflow, source branch and tested revision match. Branch protections and rulesets are not required. It publishes `ghcr.io/edbfi/base-image:<branch>` and a commit-qualified alias; version aliases apply when metadata declares a version. Pull-request CI has no registry write permission. Initial publication is part of repository migration validation; the existence of this configuration alone does not establish that tags are available.

The **Prepare image metadata update** workflow produces a patch and recovery bundle only. Review and apply the candidate on a feature branch, open a PR, and require full image CI before merging. It does not push to any branch. No hourly mutation, website write, account-wide maintenance or external notification is performed by these workflows.

Upstream synchronization is prepared by `edbfi/repo-patches` from the explicit `.upstream.json` revision. Conflicts require review; synchronization never replaces published branch history. Keep the upstream license, internal `hotio` runtime account and attribution intact.
