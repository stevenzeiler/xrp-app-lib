# AGENTS.md

Guidance for AI agents working in this repo. Humans: see [README.md](README.md).

## What this is

`xrp-app-lib` 1.1.2: a small XRP wallet library from 2014 to 2016. Dormant. Docs and small safe fixes are welcome; anything bigger needs Steven.

## Stack

- JavaScript, ES6 in `src/`, transpiled to `build/` by gulp 3 and `gulp-6to5`.
- Tests: mocha 2.
- Needs `ripple-lib` ^0.17.3 (RippleAPI), `ripple-lib-0.9.4`, bluebird 2, lodash 2, superagent 0.21.
- Era-appropriate runtime is Node 0.10 to 6 **(inference)**. Gulp 3 and `gulp-6to5` are known not to work on current Node.js.

## Layout

- `src/` library source: `index.js`, `account.js`, `wallet.js`, `errors.js`
- `build/` committed transpiled output (the package `main`)
- `test/` mocha specs and `wallets.json` (test fixtures)
- `circle.yml` old CircleCI config
- `docs/STATUS.md` current state of the repo

## Commands

- Install: `npm install` (expect failures or warnings on modern toolchains)
- Build: `npm run build` (runs gulp)
- Test: `npm test` (builds, then runs mocha with a 20 second timeout; tests call the live Ripple network)
- None of these were verified on a current toolchain when this file was written.

## Known issues

- `src/index.js` and the committed `build/index.js` (the package `main`) both require `./decodeURI`, which is not in the repo, so `require('xrp-app-lib')` from a clone fails until that file is restored. They also import `xrp-account-listener-browserify`, which is not in `package.json`.
- `test/wallets.json` holds wallet secrets that are in git history. Do not print, copy or reuse them.

## Rules

- Add or update tests with every change.
- Never commit secrets. Use placeholders such as `sXXXXXXXX`.
- Use Conventional Commits.
- Keep `docs/STATUS.md` current.

## Agents may and may not

- May: improve docs, add tests, open PRs on a branch.
- May not: archive the repo, publish to npm, rewrite git history, rotate or delete secrets, or change the license, without Steven.
