# Planetarium local-demo fork

Based on RTB4FREE/bidder commit 5cadf9f43fd4fadcd67900a6b41c8bc210131a1f.
The upstream Apache-2.0 license is preserved.

This fork fixes the administrative campaign update path:
- Database.editCampaign stores the replacement campaign instead of the old one.
- It removes the single JSON-escaping layer introduced by Campaign(String)
  before persisting creative markup. Runtime activation encodes it again.

The decoding assumes the existing WebCampaign update path constructs the
replacement with Campaign(String). This is not a new general raw-Campaign API.

Build the compatibility image with:
```sh
docker build --platform linux/amd64 -f Dockerfile.hot-update -t planetarium/rtb4free-bidder:local .
```

This compiles the changed class against the pinned upstream Java 8 runtime JAR
and replaces it inside that JAR. It is not a full Maven rebuild or a dependency
security upgrade. The original Dockerfile remains available for full builds.

Demo fixtures and the real SSP/DSP regression scripts remain in
https://github.com/planetarium/x402-jev-adex-testing:
`pnpm h5:verify:ssp` and `pnpm h5:verify:settings` exercise real bidding,
repeated campaign updates, A/B winner changes, creative rendering and settings
restoration. Run them with this image and the demo stack running.
