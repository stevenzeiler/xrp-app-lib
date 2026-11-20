# XRP App Lib

A small JavaScript library for creating and importing XRP wallets, reading balances and sending payments.

[![CircleCI](https://circleci.com/gh/stevenzeiler/xrp-app-lib.svg?style=svg)](https://circleci.com/gh/stevenzeiler/xrp-app-lib)

> **Status: dormant since 2016.** Written in 2014 and 2015 during Steven Zeiler's Ripple Labs era, with a last revival in November 2016. It depends on very old tooling (gulp 3, `gulp-6to5`, `ripple-lib` 0.17 and a bundled `ripple-lib` 0.9.4). It has not been run on a current Node.js as part of this documentation pass, so treat the examples as design notes rather than tested instructions. See [docs/STATUS.md](docs/STATUS.md).

## Install

    npm install --save xrp-app-lib

The package entry point is `build/index.js`, which is committed to the repo and also produced by `npm run build`. As committed, it requires a missing `./decodeURI` module, so loading it from a clone of this repo fails (see [docs/STATUS.md](docs/STATUS.md)).

## Usage

The public surface is a single `XRPLib` object. The signatures below are the shape of the API, not typed code.

    Wallet createWallet()

    Wallet importWalletFromSecret(PrivateKey)

    Account importAccountFromAddress(PublicKey)

    Promise<Decimal> updateBalance(Account)
    Promise<Decimal> updateBalance(Wallet)

    Promise<Payment> sendPayment({
      to: Account,
      from: Wallet,
      amount: Decimal
    })

Example, with a placeholder secret (never commit a real one):

    var XRPLib = require('xrp-app-lib')

    var wallet = XRPLib.importWalletFromSecret('sXXXXXXXX')
    XRPLib.updateBalance(wallet).then(function(balance) {
      // balance in XRP
    })

## Config

There are no environment variables. The Ripple servers are hard-coded in the source: `wss://s1.ripple.com:443` for balances (`src/account.js`) and `s1.ripple.com` port 443 for payments (`src/wallet.js`).

## How it works

- `Account` holds an address (called `publicKey` in this library) and fetches balances with `RippleAPI` from `ripple-lib`.
- `Wallet` extends `Account`, adds the secret (called `privateKey`), can generate a new wallet, and submits payments through the older `ripple-lib` 0.9.4 `Remote` API.
- `src/` is ES6 and is transpiled to `build/` with gulp and `gulp-6to5`.
- `Errors` exports `InvalidPublicKey` and `InvalidPrivateKey`.
- `Listener` is re-exported from the `xrp-account-listener-browserify` package to watch an account for transactions.

## Status

Dormant. Known problems are listed in [docs/STATUS.md](docs/STATUS.md). The license in `LICENSE` is the BipCot NoGov Software License, while `package.json` says ISC; that mismatch is waiting on Steven.
