# Status

Last reviewed: 2026-11-20 (github-audit pass 02, documentation only; no code was run).

## What works

- Nothing has been verified in this pass.
- The code reads as a complete small API: wallet generation, import from secret, balance lookup, payments, error classes.
- The 2016 commits moved balance fetching to `RippleAPI` and added CircleCI.

## What's broken or unknown

- `src/index.js` and the committed `build/index.js` (the package `main`) both require `./decodeURI`, and neither `src/decodeURI.js` nor `build/decodeURI.js` is in the repo, so loading the package from a clone fails. Whether the published npm tarball included the file is not checked.
- `xrp-account-listener-browserify` is imported but not listed in `package.json`.
- Gulp 3 and `gulp-6to5` are unlikely to run on a current Node.js.
- Payments use the old `ripple-lib` 0.9.4 `Remote` API against `s1.ripple.com`; the Ripple network and server have changed since.
- The `README` signatures were design notes; `Wallet` here uses `privateKey` and `publicKey` for the secret and the address.
- `LICENSE` is the BipCot NoGov license, but `package.json` declares ISC. Steven to decide which is right.
- `test/wallets.json` (line 5) contains a wallet secret. It is probably a throwaway test wallet, which is unverified. Git history still holds it, and rotation or history cleanup is Steven's call.

## What's next

Nothing planned. Candidates if Steven wants it alive: fix the missing import, move to current `ripple-lib` or `xrpl`, replace gulp with a plain Babel or TypeScript build, and replace the fixture secrets with generated wallets.

## Not yet from the standard

- CONTRIBUTING.md
- SECURITY.md
- `.editorconfig`
- `.github` issue and PR templates
- Dependabot
- `ci.yml` with gitleaks (the old `circle.yml` remains)
- master to main rename
- CloudEvents catalog
- Product one-pager
- LICENSE decision (see above)
