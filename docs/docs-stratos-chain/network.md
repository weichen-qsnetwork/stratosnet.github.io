---
id: network
title: Network
description: Stratos Testnet and Mainnet Network Details.
---

## Network details

> **_NOTE:_** Mesos network native token is **STOS** which will be used as gas fee.

=== "Mesos Testnet"

    Mesos Testnet replicates the Stratos Mainnet, which is to be used for testing. Testnet coins are separate and distinct from actual tokens/assets, and are never supposed to have any value. This allows application developers or validators/testers to experiment, without having to use real assets or worrying about breaking the main Stratos chain.

    The documentation corresponding contains details for the RPC - HTTP, WS endpoints. There is also a full node setup if you wish to setup your own full node.

    | Service | Details |
    |:---------|:---------|
    | RPC | https://rpc-mesos.thestratos.org/ |
    | REST | https://rest-mesos.thestratos.org/ |
    | Faucet | https://faucet-mesos.thestratos.org/ |
    | Block Explorer | https://explorer-mesos.thestratos.org/ |

=== "Mainnet"

    Work in progress

---

## What is Mesos?

The purpose of mesos Testnet is to validate all the technical design and network topology that can achieve the goal of our project — to provide a fully decentralized high-efficiency storage. On Mesos testnet we will continue all the testing we employed on testnet; in addition, we will try to ensure the network is functional as in the real world, to build an environment as close to the mainnet as possible.

Our goal for the mesos testnet is to ensure we can migrate to the mainnet smoothly and provide powerful decentralized storage to real corporate world users.

<br>

---

## Highlight Changes in Mesos

### Stratos Chain

After testing and building the binaries v0.10.0, we released the latest version of Stratos-Testnet, with a new chain-id: `mesos`.

This version covers several breakthrough improvements and significant performance enhancements. We will highlight the major exciting updates in this section.

---

#### - stratos-chain gRPC APIs

All API queries now support gRPC.


---

#### - Mining reward denom

The token denom of mining reward will use `wei`.
The previous token denom `utros` in `tropos-5`, is NOT in use anymore.
All exists mining reward will be converted into `wei`.

The conversion rate and more details about the mining rewards can be found <a href="https://blog.thestratos.org/tropos-incentive-testnet-reward-summary/" target="_blank">here</a>.

---

#### - Software Upgrade

Go-ethereum and btcd versions are updated to avoid security issues.

Correspondingly, if you prefer to build and compile the binary from source code, you need to have [`Go 1.19+`](https://golang.org/doc/install) installed, rather than the former `Go 1.18+`.

---

<br>

### Resource Node


#### - UpdateDeposit

In `mesos`, previous `updateStake` is renamed to `updateDeposit`.

`updateDeposit` functionality only allow to deposit stake to the sds node, don't support withdraw stake anymore. Use `deactivate` cmd to withdraw stake of sds node.

---

#### - Clearexpshare

New command `clearexpshare` to clear all expired share links

---

#### - Withdraw

For more convenient withdrawal matured reward, pp provides a new `withdraw <amount> <fee> optional<targetAddr> optional<gas> ` command, which is used to replace direct withdraw operation on stratos-chain (from address is the configured node wallet).

---

#### - Send

For more convenient transfer, pp provides a new `send <toAddress> <amount> <fee> optional<gas> ` command, which is used to replace direct transfer operation on stratos-chain (from address is the configured node wallet).

<br>

---

<br>