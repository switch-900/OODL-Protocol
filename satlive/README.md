# SatLive support for an on-chain OODL runtime

This repository currently contains the OODL Protocol landing page and README, not a dedicated versioned Bitcoin inscription application. The landing page is not retroactively updateable by adding source code to GitHub.

The shared, dependency-free SatLive runtime is staged in [satlive.mjs](./satlive.mjs). It is intended for an **actual future OODL HTML inscription launcher**, compiled into the immutable first launcher before it is published.

## Launcher integration

```js
import { startSatLive } from './satlive.mjs';

// Replace the sample placeholders and create a *new* launcher. Do not enable
// this configuration until its public P-256 signing key is pinned offline.
const OODL_SATLIVECONFIG = {
  enabled: true,
  app: 'oodl',
  sat: 'SAT_NUMBER',
  entry: 'self', // in the initial launcher only
  publicKey: { kty: 'EC', crv: 'P-256', x: 'PINNED_X', y: 'PINNED_Y' },
  loaderVersion: 1,
  allowedContentTypes: ['text/html'],
  minConfirmations: 1
};

await startSatLive(OODL_SATLIVECONFIG, () => startExistingOodlRuntime());
```

The first launcher resolves its real inscription ID from its own URL. Later versions must embed that **exact original inscription ID** instead of using `self`. Publish content and then a signed JSON manifest on the same sat. The public key is a release authority, not the sat's current owner.

Security tests and the offline signer are maintained in [the Nexus Loader SatLive source](https://github.com/switch-900/Oodinals-Nexus-Loader/tree/feat/satlive-signed-resolution-20261107/satlive). Wallet and marketplace runtime code must not import unsigned `/r/sat/.../at/-1/content` dependencies and claim end-to-end update authenticity. Do not commit private signing keys, rewrite the existing OODL landing-page runtime, or broadcast any transactions as part of this optional integration.
