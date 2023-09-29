---
title: How to use state sync to start stratos-chain
description: This document describes how to start the stratos-chain using state sync.
---

## Introduction

State sync allows a new node to join a network by fetching a snapshot of the application state at a recent height instead of fetching and replaying all historical blocks. 

Since the application state is generally much smaller than the blocks, and restoring it is much faster than replaying blocks, this can reduce the time to sync with the network from days to minutes.

!!! warning

	State Sync feature only works with new node installations.

---

## Start a node using stateSync:

=== "MAINNET StateSync"

	### 1. Get the last block height

	```shell
	curl -s http://rpc.thestratos.org/block | jq -r '.result.block.header.height'
	```

	```
	Example response:
	326121
	```

	### 2. Get the hash for the block

	Since snapshots are generated every 1,000 blocks, you'll need to obtain the hash for the block number at 1,000-interval heights. 

	For example, in the above response we got `326121` so we will need to request the hash for height `326000`.

	If latest block would have been `374521`, we will request the hash for `374000` and so on.

	Always use the most recent block height, rounded down to the nearest lower multiple of 1,000.

	```shell
	curl -s http://rpc.thestratos.org/block?height=326000 | jq -r '.result.block_id.hash'
	```

	```
	Example response:
	C524665A353CB6C5E03D4B73B3151FA00862704A0966E01C5E97F1DE1B08B1B4
	```

	### 3. Setup config.toml

	Edit the state sync section of `.stchaind/config/config.toml` as follows:

	Remember to use the latest height rounded down to last 1,000 round number and its hash.

	```toml
	#######################################################
	###         State Sync Configuration Options        ###
	#######################################################
	[statesync]
	# State sync rapidly bootstraps a new node by discovering, fetching, and restoring a state machine
	# snapshot from peers instead of fetching and replaying historical blocks. Requires some peers in
	# the network to take and serve state machine snapshots. State sync is not attempted if the node
	# has any local state (LastBlockHeight > 0). The node will have a truncated block history,
	# starting from the height of the snapshot.
	enable = true

	# RPC servers (comma-separated) for light client verification of the synced state machine and
	# retrieval of state data for node bootstrapping. Also needs a trusted height and corresponding
	# header hash obtained from a trusted source, and a period during which validators can be trusted.
	#
	# For Cosmos SDK-based chains, trust_period should usually be about 2/3 of the unbonding time (~2
	# weeks) during which they can be financially punished (slashed) for misbehavior.
	rpc_servers = "35.187.203.203:26657,35.187.189.109:26657"
	trust_height = 326000
	trust_hash = "C524665A353CB6C5E03D4B73B3151FA00862704A0966E01C5E97F1DE1B08B1B4"
	trust_period = "168h0m0s"

	# Time to spend discovering snapshots before initiating a restore.
	discovery_time = "15s"

	# Temporary directory for state sync snapshot chunks, defaults to the OS tempdir (typically /tmp).
	# Will create a new, randomly named directory within, and remove it when done.
	temp_dir = "./tmp"

	# The timeout duration before re-requesting a chunk, possibly from a different
	# peer (default: 1 minute).
	chunk_request_timeout = "10s"

	# The number of concurrent chunk fetchers to run (default: 1).
	chunk_fetchers = "4"
	```

=== "TESTNET StateSync"

	### 1. Get the last block height

	```shell
	curl -s http://rpc-mesos.thestratos.org/block | jq -r '.result.block.header.height'
	```

	```
	Example response:
	326121
	```

	### 2. Get the hash for the block

	Since snapshots are generated every 1,000 blocks, you'll need to obtain the hash for the block number at 1,000-interval heights. 

	For example, in the above response we got `326121` so we will need to request the hash for height `326000`.

	If latest block would have been `374521`, we will request the hash for `374000` and so on.

	Always use the most recent block height, rounded down to the nearest lower multiple of 1,000.

	```shell
	curl -s http://rpc-mesos.thestratos.org/block?height=326000 | jq -r '.result.block_id.hash'
	```

	```
	Example response:
	C524665A353CB6C5E03D4B73B3151FA00862704A0966E01C5E97F1DE1B08B1B4
	```

	### 3. Setup config.toml

	Edit the state sync section of `.stchaind/config/config.toml` as follows:

	Remember to use the latest height rounded down to last 1,000 round number and its hash.

	```toml
	#######################################################
	###         State Sync Configuration Options        ###
	#######################################################
	[statesync]
	# State sync rapidly bootstraps a new node by discovering, fetching, and restoring a state machine
	# snapshot from peers instead of fetching and replaying historical blocks. Requires some peers in
	# the network to take and serve state machine snapshots. State sync is not attempted if the node
	# has any local state (LastBlockHeight > 0). The node will have a truncated block history,
	# starting from the height of the snapshot.
	enable = true

	# RPC servers (comma-separated) for light client verification of the synced state machine and
	# retrieval of state data for node bootstrapping. Also needs a trusted height and corresponding
	# header hash obtained from a trusted source, and a period during which validators can be trusted.
	#
	# For Cosmos SDK-based chains, trust_period should usually be about 2/3 of the unbonding time (~2
	# weeks) during which they can be financially punished (slashed) for misbehavior.
	rpc_servers = "35.160.97.156:26657,rpc-mesos.thestratos.org:80"
	trust_height = 326000
	trust_hash = "C524665A353CB6C5E03D4B73B3151FA00862704A0966E01C5E97F1DE1B08B1B4"
	trust_period = "168h0m0s"

	# Time to spend discovering snapshots before initiating a restore.
	discovery_time = "15s"

	# Temporary directory for state sync snapshot chunks, defaults to the OS tempdir (typically /tmp).
	# Will create a new, randomly named directory within, and remove it when done.
	temp_dir = "./tmp"

	# The timeout duration before re-requesting a chunk, possibly from a different
	# peer (default: 1 minute).
	chunk_request_timeout = "10s"

	# The number of concurrent chunk fetchers to run (default: 1).
	chunk_fetchers = "4"
	```

### 4. Disable JSON-RPC

The EVM RPC will prevent your node from starting using state sync, so you can temporarily disable it by editing `.stchaind/config/app.toml`:

You can re-enable it once node's sync is up to date (node restart required).

```
[json-rpc]
# Enable defines if the gRPC server should be enabled.
enable = false
```

### 5. Start the node

Node can now be started with the usual command:

```shell
stchaind start
```

---

## Become snapshot provider



To provide the StateSync snapshot, the operator must enable the `snapshot-interval` in the node configuration:

```toml
# snapshot-interval specifies the block interval at which local state sync snapshots are
# taken (0 to disable). Must be a multiple of pruning-keep-every.
snapshot-interval = 1000

# snapshot-keep-recent specifies the number of recent snapshots to keep and serve (0 to keep all).
snapshot-keep-recent = 2
```

---

<br>
