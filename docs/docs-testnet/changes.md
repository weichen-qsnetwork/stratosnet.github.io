---
title: Stratos Testnet Changes
description: Stratos Network testnet (mesos) changes.
---

# Highlight Changes in mesos

After testing and building the binaries v0.10.0, we released the latest version of Stratos-Testnet, with a new chain-id: `mesos`.

This version covers several breakthrough improvements and significant performance enhancements. We will highlight the major exciting updates in this section.

<br>

## - stratos-chain gRPC APIs

All query APIs support gRPC now.

<br>

## - Security enhanced

We fixed some security issues and enhanced the verification logic of some transactions.

<br>

## - Mining reward denom

The token denom of mining reward will use `wei`.
The previous token denom `utros` in `tropos-5`,is NOT in use anymore.
All exists mining reward will be converted into `wei`.
``` { .yaml .no-copy }
1utros = 1,000,000,000wei
```

<br>

## - Software Upgrade

Go-ethereum and btcd versions are updated to avoid security issues

Correspondingly, if you prefer to build and compile the binary from source code, you need to have [`Go 1.19+`](https://golang.org/doc/install) installed, rather than the former `Go 1.18+`.

<br>

## - UpdateDeposit

In `mesos`, previous `updateStake` is renamed to `updateDeposit`.

`updateDeposit` functionality only allow to deposit stake to the sds node, don't support withdraw stake anymore. Use `deactivate` cmd to withdraw stake of sds node, 

<br>

## - Clearexpshare

New command `clearexpshare` to clear all expired share links

<br>

## - Withdraw

For more convenient withdrawal matured reward, pp provides a new `withdraw <amount> <fee> optional<targetAddr> optional<gas> ` command, which is used to replace direct withdraw operation on stratos-chain (from address is the configured node wallet).

<br>

## - Send

For more convenient transfer, pp provides a new `send <toAddress> <amount> <fee> optional<gas> ` command, which is used to replace direct transfer operation on stratos-chain (from address is the configured node wallet).

<br>


---

<br>