# base-image

Base images derived from [hotio/base](https://github.com/hotio/base), retaining its GPL license, runtime layout, VPN services and container automation.

The `workflows` branch owns hotio's shared `build-on-call.yml` and `update-on-call.yml`. The `alpinevpn` and `noblevpn` branches contain image sources and callers pointing to `edbfi/base-image@workflows`. Pushes to image branches build and publish through hotio's architecture matrix, metadata and smoke-test logic. This repository is separate from the unified application CI system.

The edbfi changes are workflow references, documentation URLs, startup branding and explicit workflow permissions. Upstream Renovate, account-wide maintenance and Discord notifications are omitted. The internal `hotio` runtime account and upstream attribution remain unchanged.

Hotio's metadata updater and website tag writer require an Actions secret named `PERSONAL_TOKEN` with the appropriate repository access. Branch protection must also permit their upstream direct-write behavior; restoring workflow files alone does not configure those integrations. Image publication uses `GITHUB_TOKEN` with package write permission.

Upstream refreshes are prepared by `edbfi/repo-patches` using `tools/refresh_upstream.py`. It reconstructs an upstream tree and reapplies the edbfi customizations, retaining a patch and recovery bundle. Apply the reviewed result through a PR on each destination branch; branch history is preserved. `.upstream.json` records the source revision. Ordinary incremental updates can still use `tools/prepare_sync.py`.

Documentation: https://web.edb.fi/containers/base-image/.
