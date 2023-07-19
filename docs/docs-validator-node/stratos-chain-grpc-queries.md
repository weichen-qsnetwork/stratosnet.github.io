---
title: Stratos Chain stchaind gRPC queries
description: Stratos Network Full-Chain stchaind gRPC queries.
---

Cosmos SDK gRPC definitions have been documented [here](https://crypto.org/docs/resources/cosmos-grpc-docs.html#cosmos.auth.v1beta1.QueryAccountRequest)

## Register Module

<br>

---

### gRPC Gateway

| Method Name              | Request Type                                                                    | Response Type                                                                                                                                                                                                                                                                                                               | Description                                                               |
|--------------------------|---------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| ResourceNode             | QueryResourceNodeRequest <br>fields:{"network_addr":string}                     | QueryResourceNodeResponse <br>fields:{"node":ResourceNode}                                                                                                                                                                                                                                                                  | Get info of a registered resource node                                    |
| MetaNode                 | QueryMetaNodeRequest <br>fields:{"network_addr":string}                         | QueryMetaNodeResponse <br>fields:{"node":MetaNode}                                                                                                                                                                                                                                                                          | Get info of a registered meta node                                        |
| Params                   | QueryParamsRequest <br>fields:{}                                                | QueryParamsResponse <br>fields:{"params":Params}                                                                                                                                                                                                                                                                            | Get params of Register Module                                             |
| DepositByNode            | QueryDepositByNodeRequest <br>fields:{"network_addr":string, query_type:uint32} | QueryDepositByNodeResponse <br>fields:{"deposit_info":DepositInfo }                                                                                                                                                                                                                                                         | Get deposit info of a specific node                                       |                                  
| DepositByOwner           | QueryDepositByOwnerRequest <br>fields:{"owner_addr":string}                     | QueryDepositByOwnerResponse <br>fields:{"deposit_infos":[]DepositInfo, <br>"pagination": cosmos.base.query.v1beta1.PageResponse }                                                                                                                                                                                           | Get all deposit info of a specific owner                                  |
| DepositTotal             | QueryDepositTotalRequest <br>fields:{}                                          | QueryDepositTotalResponse <br>fields:{"resource_nodes_total_deposit":cosmos.base.v1beta1.Coin, <br>"meta_nodes_total_deposit":cosmos.base.v1beta1.Coin, <br>"total_bonded_deposit":cosmos.base.v1beta1.Coin, <br>"total_unbonded_deposit":cosmos.base.v1beta1.Coin, <br>"total_unbonding_deposit":cosmos.base.v1beta1.Coin} | Query total deposit state of all registered resource nodes and meta nodes |
| BondedResourceNodeCount  | QueryBondedResourceNodeCountRequest <br>fields:{}                               | QueryBondedResourceNodeCountResponse <br>fields:{"number": uint64}                                                                                                                                                                                                                                                          | Get params of Register Module                                             |
| BondedMetaNodeCount      | QueryBondedMetaNodeCountRequest <br>fields:{}                                   | QueryBondedMetaNodeCountResponse <br>fields:{"number": uint64}                                                                                                                                                                                                                                                              | Get params of Register Module                                             |

<br>

---

ResourceNode:

| Field           | Type                              | Label |
|-----------------|-----------------------------------|-------|
| network_address | string                            |       |
| pubkey          | google.protobuf.Any               |       |
| suspend         | bool                              |       |
| status          | cosmos.staking.v1beta1.BondStatus |       |
| tokens          | string                            |       |
| owner_address   | string                            |       |
| description     | Description                       |       |
| creation_time   | google.protobuf.Timestamp         |       |
| node_type       | uint32                            |       |

<br>

---

MetaNode:

| Field           | Type                              | Label |
|-----------------|-----------------------------------|-------|
| network_address | string                            |       |
| pubkey          | google.protobuf.Any               |       |
| suspend         | bool                              |       |
| status          | cosmos.staking.v1beta1.BondStatus |       |
| tokens          | string                            |       |
| owner_address   | string                            |       |
| description     | Description                       |       |
| creation_time   | google.protobuf.Timestamp         |       |

<br>

---

Description:

| Field            | Type   | Label |
|------------------|--------|-------|
| moniker          | string |       |
| identity         | string |       |
| website          | string |       |
| security_contact | string |       |
| details          | string |       |

<br>

---

Params:

| Field                     | Type                     | Label |
|---------------------------|--------------------------|-------|
| bond_denom                | string                   |       |
| unbonding_threashold_time | google.protobuf.Duration |       |
| unbonding_completion_time | google.protobuf.Duration |       |
| max_entries               | uint32                   |       |

<br>

---

DepositInfo:

| Field              | Type                              | Label |
|--------------------|-----------------------------------|-------|
| network_address    | string                            |       |
| pubkey             | google.protobuf.Any               |       |
| suspend            | bool                              |       |
| status             | cosmos.staking.v1beta1.BondStatus |       |
| tokens             | string                            |       |
| owner_address      | string                            |       |
| description        | Description                       |       |
| creation_time      | google.protobuf.Timestamp         |       |
| node_type          | uint32                            |       |
| bonded_deposit     | cosmos.base.v1beta1.Coin          |       |
| un_bonding_deposit | cosmos.base.v1beta1.Coin          |       |
| un_bonded_deposit  | cosmos.base.v1beta1.Coin          |       |

<br>


### - List

List all available grpc queries in Register Module

Request:

```
grpcurl -plaintext 127.0.0.1:9090 list stratos.register.v1.Query
```
Response:

``` { .yaml .no-copy }
stratos.register.v1.Query.ResourceNode
stratos.register.v1.Query.MetaNode
stratos.register.v1.Query.Params
stratos.register.v1.Query.DepositByNode
stratos.register.v1.Query.DepositByOwner
stratos.register.v1.Query.DepositTotal
stratos.register.v1.Query.BondedResourceNodeCount
stratos.register.v1.Query.BondedMetaNodeCount

```

<br>

### - ResourceNode

Get info of a registered resource node

Request:

```
grpcurl -plaintext -d '{"network_addr":"stsds1xg2jzku8gptq6sjpjd9zus8qec7a39phcj8md9"}' 127.0.0.1:9090 stratos.register.v1.Query.ResourceNode
```

Response:

```json
{
 "node": {
  "networkAddress": "stsds1xg2jzku8gptq6sjpjd9zus8qec7a39phcj8md9",
  "pubkey": {
   "@type": "/cosmos.crypto.ed25519.PubKey",
   "key": "JZhVcpSQU7rXzlaiqjpSW4u2n6lE/Q+xHG2GFaUkFkA="
  },
  "status": "BOND_STATUS_BONDED",
  "tokens": "2000000000000000000",
  "ownerAddress": "st1vvysda6ylqz2adauqg4djsz4rx6hv6mqv9fepp",
  "description": {
   "moniker": "stsds1xg2jzku8gptq6sjpjd9zus8qec7a39phcj8md9"
  },
  "creationTime": "2023-01-14T00:28:21.819038836Z",
  "nodeType": 4
 }
}
```

<br>


### - MetaNode

Get info of a registered meta node

Request:

```
grpcurl -plaintext -d '{"network_addr":"stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64"}' 127.0.0.1:9090 stratos.register.v1.Query.MetaNode
```

Response:

```json
{
 "node": {
  "networkAddress": "stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64",
  "pubkey": {
   "@type": "/cosmos.crypto.ed25519.PubKey",
   "key": "ltODy8zL5IjJwCutlIexqlBb3GH0+aHZOrpT7f/aKnQ="
  },
  "status": "BOND_STATUS_BONDED",
  "tokens": "100000000000000000000",
  "ownerAddress": "st1a8ngk4tjvuxneyuvyuy9nvgehkpfa38hm8mp3x",
  "description": {
   "moniker": "snode://stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64@127.0.0.1:8888"
  },
  "creationTime": "0001-01-01T00:00:00Z"
 }
}
```

<br>


### - Params

Get params of Register Module

Request:

```
grpcurl -plaintext 127.0.0.1:9090 stratos.register.v1.Query.Params
```

Response:

```json
{
 "params": {
  "bondDenom": "wei",
  "unbondingThreasholdTime": "15552000s",
  "unbondingCompletionTime": "1209600s",
  "maxEntries": 16,
  "resourceNodeRegEnabled": true
 }
}
```

<br>


### - DepositByNode

Get deposit info of a specific node

Request:

```
grpcurl -plaintext -d '{"network_addr":"stsds12xeswat6yy3dg9e3q3ekl6dlnld4g9yhjz70c4","query_type": 1 }' 127.0.0.1:9090 stratos.register.v1.Query.DepositByNode
```

Response:

```json
{
  "deposit_info": {
    "network_address": "stsds12xeswat6yy3dg9e3q3ekl6dlnld4g9yhjz70c4",
    "pubkey": {
      "type_url": "/cosmos.crypto.ed25519.PubKey",
      "value": "CiD+uMRSnq9prSATpE8XPvAh9FM6xaQWri1mg1vihC7Feg=="
    },
    "suspend": false,
    "status": "BOND_STATUS_BONDED",
    "tokens": "2000000000000000000000000",
    "owner_address": "st19n4aa9gvjms5ax0zzrgzhpsdgg8hfy7p4m43xd",
    "description": {
      "moniker": "stratos-foundation",
      "identity": "",
      "website": "",
      "security_contact": "",
      "details": ""
    },
    "creation_time": {
      "seconds": "-62135596800",
      "nanos": 0
    },
    "node_type": 0,
    "bonded_deposit": {
      "denom": "wei",
      "amount": "2000000000000000000000000"
    },
    "un_bonding_deposit": {
      "denom": "wei",
      "amount": "0"
    },
    "un_bonded_deposit": {
      "denom": "wei",
      "amount": "0"
    }
  }
}
```

<br>


### - DepositByOwner

Get all deposit info of a specific owner

Request:

```
grpcurl -plaintext -d '{"owner_addr":"st19n4aa9gvjms5ax0zzrgzhpsdgg8hfy7p4m43xd", "pagination": {"limit":20}}' 127.0.0.1:9090 stratos.register.v1.Query.DepositByOwner
```

Response:

```json
{
  "deposit_infos": [
    {
      "network_address": "stsds12xeswat6yy3dg9e3q3ekl6dlnld4g9yhjz70c4",
      "pubkey": {
        "type_url": "/cosmos.crypto.ed25519.PubKey",
        "value": "CiD+uMRSnq9prSATpE8XPvAh9FM6xaQWri1mg1vihC7Feg=="
      },
      "suspend": false,
      "status": "BOND_STATUS_BONDED",
      "tokens": "2000000000000000000000000",
      "owner_address": "st19n4aa9gvjms5ax0zzrgzhpsdgg8hfy7p4m43xd",
      "description": {
        "moniker": "stratos-foundation",
        "identity": "",
        "website": "",
        "security_contact": "",
        "details": ""
      },
      "creation_time": {
        "seconds": "-62135596800",
        "nanos": 0
      },
      "node_type": 0,
      "bonded_deposit": {
        "denom": "wei",
        "amount": "2000000000000000000000000"
      },
      "un_bonding_deposit": {
        "denom": "wei",
        "amount": "0"
      },
      "un_bonded_deposit": {
        "denom": "wei",
        "amount": "0"
      }
    }
  ],
  "pagination": {
    "next_key": "",
    "total": "1"
  }
}
```

<br>


### - DepositTotal

Query total deposit state of all registered resource nodes and meta nodes

Request:

```
 grpcurl -plaintext 127.0.0.1:9090 stratos.register.v1.Query.DepositTotal
```

Response:

```json
{
  "resource_nodes_total_deposit": {
    "denom": "wei",
    "amount": "26003000000000000000000"
  },
  "meta_nodes_total_deposit": {
    "denom": "wei",
    "amount": "8000000000000000000000000"
  },
  "total_bonded_deposit": {
    "denom": "wei",
    "amount": "8026003000000000000000000"
  },
  "total_unbonded_deposit": {
    "denom": "wei",
    "amount": "0"
  },
  "total_unbonding_deposit": {
    "denom": "wei",
    "amount": "0"
  }
}
```

<br>


### - BondedResourceNodeCount

Queries total number of Bonded ResourceNodes

Request:

```
grpcurl -plaintext 127.0.0.1:9090 stratos.register.v1.Query.BondedResourceNodeCount
```

Response:

```json
{
  "number": "2"
}
```

<br>


### - BondedMetaNodeCount

Queries total number of Bonded MetaNodes

Request:

```
grpcurl -plaintext 127.0.0.1:9090 stratos.register.v1.Query.BondedMetaNodeCount
```

Response:

```json
{
  "number": "4"
}
```

<br>


---

## SDS Module

### gRPC Gateway

| Method Name | Request Type                                                           | Response Type                                                         | Description                                                                 |
|-------------|------------------------------------------------------------------------|-----------------------------------------------------------------------|-----------------------------------------------------------------------------|
| Fileupload  | QueryFileUploadRequest <br>fields:{"file_hash":string}                 | QueryFileUploadResponse <br>fields:{"file_info":FileInfo}             | Query uploaded file info by hash                                            |
| SimPrepay   | QuerySimPrepayRequest <br>fields:{"amount":[]cosmos.base.v1beta1.Coin} | QuerySimPrepayResponse <br>fields:{"noz":string}                      | Simulate prepay to query the noz that can be purchased at the current price |
| NozPrice    | QueryNozPriceRequest <br>fields:{}                                     | QueryNozPriceResponse <br>fields:{"price":string}                     | Query the current price of noz                                              |
| NozSupply   | QueryNozSupplyRequest <br>fields:{}                                    | QueryNozSupplyResponse <br>fields:{"remaining":string,"total":string} | Query noz supply                                                            |
| Params      | QueryParamsRequest <br>fields:{}                                       | QueryParamsResponse <br>fields:{"params":Params}                      | Get params of SDS Module                                                    |

FileInfo:

| Field     | Type   | Label |
|-----------|--------|-------|
| height    | string |       |
| reporters | bytes  |       |
| uploader  | string |       |

Params:

| Field      | Type   | Label |
|------------|--------|-------|
| bond_denom | string |       |


### - List

List all available grpc queries in SDS Module

Request:

```
 grpcurl -plaintext 127.0.0.1:9090 list stratos.sds.v1.Query
```

Response:

``` { .yaml .no-copy }
stratos.sds.v1.Query.Fileupload
stratos.sds.v1.Query.SimPrepay
stratos.sds.v1.Query.NozPrice
stratos.sds.v1.Query.NozSupply
stratos.sds.v1.Query.Params
```

<br>


### - Fileupload

Query uploaded file info by hash

Request:

```
 grpcurl -plaintext -d '{"file_hash":"v05ahm51dd62ise3fo7ojqub90p0ql2c3jg37hk8"}' 127.0.0.1:9090 stratos.sds.v1.Query.Fileupload
```

Response:

```json
{
    "file_info": {
        "height": "4109",
        "reporters": "DwAAAAAAAAA=",
        "uploader": "st18986jyng5vsprmtzkdxla80jrw7qyc6wl73h0u"
    }
}
```

<br>


### - SimPrepay

Simulate prepay to query the noz that can be purchased at the current price

Request:

```
 grpcurl -plaintext -d '{"amount":[{"amount":"100000000000","denom":"wei"}]}' 127.0.0.1:9090 stratos.sds.v1.Query.SimPrepay
```

Response:

```json
{
    "noz": "98736"
}
```

<br>


### - NozPrice

Query the current price of noz

Request:

```
 grpcurl -plaintext 127.0.0.1:9090 stratos.sds.v1.Query.NozPrice
```

Response:

```json
{
    "price": "1012791644248016784459322"
}
```

<br>


### - NozSupply

Query noz supply

Request:

```
 grpcurl -plaintext 127.0.0.1:9090 stratos.sds.v1.Query.NozSupply
```

Response:

```json
{
    "remaining": "7949398620856330560",
    "total": "8000080000000000000"
}
```

<br>


### - Params

Get params of SDS Module

Request:

```
 grpcurl -plaintext 127.0.0.1:9090 stratos.sds.v1.Query.Params
```

Response:

```json
{
    "params": {
        "bond_denom": "wei"
    }
}
```

<br>


---

## POT Module

### gRPC Gateway

| Method Name       | Request Type                                                                                                                         | Response Type	                                                                                                                    | Description                    |
|-------------------|--------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| VolumeReport      | QueryVolumeReportRequest <br>fields:{"epoch":int64}                                                                                  | QueryVolumeReportResponse <br>fields:{"report_info":ReportInfo,"height":int64 }                                                   | Get pot volume report by epoch |
| RewardsByEpoch    | QueryRewardsByEpochRequest <br>fields:{"epoch":int64,"wallet_address":string,<br>pagination":cosmos.base.query.v1beta1.PageResponse} | QueryRewardsByEpochResponse <br>fields:{"rewards":[]Reward,"height":int64,<br>"pagination":cosmos.base.query.v1beta1.PageRequest} | Query pot reward by epoch      |
| RewardsByOwner    | QueryRewardsByOwnerRequest <br>fields:{"wallet_address":string}                                                                      | QueryRewardsByOwnerResponse <br>fields:{"rewards":RewardByOwner,"height":int64}                                                   | Get pot reward by owner        |
| SlashingByOwner   | QuerySlashingByOwnerRequest <br>fields:{"wallet_address":string}                                                                     | QuerySlashingByOwnerResponse <br>fields:{"slashing":string,"height":int64}                                                        | Get pot slashing by owner      |
| Params            | QueryParamsRequest <br>fields:{}                                                                                                     | QueryParamsResponse <br>fields:{"params":Params}                                                                                  | Get params of POT Module       |
| TotalMinedToken   | QueryTotalMinedTokenRequest <br>fields:{}                                                                                            | QueryTotalMinedTokenResponse <br>fields:{"total_mined_token": cosmos.base.v1beta1.Coin}                                           | Get total mined token          |
| CirculationSupply | QueryCirculationSupplyRequest <br>fields:{}                                                                                          | QueryCirculationSupplyResponse <br>fields:{"circulation_supply":[]cosmos.base.v1beta1.Coin}                                       | Get circulation supply         |


ReportInfo:

| Field     | Type    | Label |
|-----------|---------|-------|
| epoch     | int64   |       |
| reference | string  |       |
| tx_hash   | string  |       |
| reporter  | string  |       |

Reward:

| Field                    | Type                      | Label     |
|--------------------------|---------------------------|-----------|
| wallet_address           | string                    |           |
| reward_from_mining_pool  | cosmos.base.v1beta1.Coin  | repeated  |
| reward_from_traffic_pool | cosmos.base.v1beta1.Coin  | repeated  |

RewardByOwner:

| Field                 | Type                     | Label    |
|-----------------------|--------------------------|----------|
| wallet_address        | string                   |          |
| mature_total_reward   | cosmos.base.v1beta1.Coin | repeated |
| immature_total_reward | cosmos.base.v1beta1.Coin | repeated |

Params:

| Field                | Type                     | Label    |
|----------------------|--------------------------|----------|
| bond_denom           | string                   |          |
| reward_denom         | string                   |          |
| mature_epoch         | int64                    |          |
| mining_reward_params | MiningRewardParam        | repeated |
| community_tax        | string                   |          |
| initial_total_supply | cosmos.base.v1beta1.Coin |          |

MiningRewardParam:

| Field                          | Type                     | Label |
|--------------------------------|--------------------------|-------|
| total_mined_valve_start        | cosmos.base.v1beta1.Coin |       |
| total_mined_valve_end          | cosmos.base.v1beta1.Coin |       |
| mining_reward                  | cosmos.base.v1beta1.Coin |       |
| block_chain_percentage_in_bp   | string                   |       |
| resource_node_percentage_in_bp | string                   |       |
| meta_node_percentage_in_bp     | string                   |       |


### - List

List all available grpc queries in POT Module

Request:

```
 grpcurl -plaintext 127.0.0.1:9090 list stratos.pot.v1.Query
```

Response:

``` { .yaml .no-copy }
stratos.pot.v1.Query.VolumeReport
stratos.pot.v1.Query.RewardsByEpoch
stratos.pot.v1.Query.RewardsByOwner
stratos.pot.v1.Query.SlashingByOwner
stratos.pot.v1.Query.Params
stratos.pot.v1.Query.TotalMinedToken
stratos.pot.v1.Query.CirculationSupply
```

<br>


### - VolumeReport

Get pot volume report by epoch

Request:

```
grpcurl -plaintext -d '{"epoch": 2 }' 127.0.0.1:9090 stratos.pot.v1.Query.VolumeReport
```

Response:

```json
{
  "report_info": {
    "epoch": "2",
    "reference": "volume_report_stsds12s8q4hhh0tnvrqfd4u4dn988lfewfsmzn2wp36_2",
    "tx_hash": "7DBECF6582285983B25999BB0D3B8FFBADE0468543DFBF5BF7961162DB0129A9",
    "reporter": "stsds12s8q4hhh0tnvrqfd4u4dn988lfewfsmzn2wp36"
  },
  "height": "18519"
}
```

<br>


### - RewardsByEpoch

Query pot reward by epoch

Request:

```
grpcurl -plaintext -d '{"epoch": 2, "wallet_address": "st18jxmc78ws5wq7q7umr6plpz8x0d9qtzu98v8em" }' 127.0.0.1:9090 stratos.pot.v1.Query.RewardsByEpoch
```

Response:

```json
{
  "rewards": [
    {
      "reward_from_mining_pool": [
        {
          "denom": "gwei",
          "amount": "7813154022"
        }
      ],
      "reward_from_traffic_pool": [
        {
          "denom": "wei",
          "amount": "1016440675459"
        }
      ],
      "wallet_address": "st18jxmc78ws5wq7q7umr6plpz8x0d9qtzu98v8em"
    }
  ],
  "height": "18980",
  "pagination": {
    "next_key": "",
    "total": "0"
  }
}
```

<br>


### - RewardsByOwner

Get pot reward by owner

Request:

```
grpcurl -plaintext -d '{"wallet_address": "st18jxmc78ws5wq7q7umr6plpz8x0d9qtzu98v8em"} ' 127.0.0.1:9090 stratos.pot.v1.Query.RewardsByOwner
```

Response:

```json
{
  "rewards": {
    "mature_total_reward": [],
    "immature_total_reward": [
      {
        "denom": "gwei",
        "amount": "93757827824"
      },
      {
        "denom": "wei",
        "amount": "88509725445757"
      }
    ],
    "wallet_address": "st18jxmc78ws5wq7q7umr6plpz8x0d9qtzu98v8em"
  },
  "height": "18998"
}
```

<br>


### - SlashingByOwner

Get pot slashing by owner

Request:

```
grpcurl -plaintext -d '{"wallet_address": "st16uzr20lx072gexwjuvg94hz3t8y73u4085s9sw"} ' 127.0.0.1:9090 stratos.pot.v1.Query.SlashingByOwner
```

Response:

```json
{
 "slashing": "0",
 "height": "2977"
}
```

<br>


### - Params

Get params of POT Module

Request:

```
 grpcurl -plaintext 127.0.0.1:9090 stratos.pot.v1.Query.Params
```

Response:

```json
{
  "params": {
    "mining_reward_params": [
      {
        "total_mined_valve_start": {
          "denom": "gwei",
          "amount": "0"
        },
        "total_mined_valve_end": {
          "denom": "gwei",
          "amount": "16819200000000000"
        },
        "mining_reward": {
          "denom": "gwei",
          "amount": "80000000000"
        },
        "block_chain_percentage_in_bp": "2000",
        "resource_node_percentage_in_bp": "6000",
        "meta_node_percentage_in_bp": "2000"
      },
      {
        "total_mined_valve_start": {
          "denom": "gwei",
          "amount": "16819200000000000"
        },
        "total_mined_valve_end": {
          "denom": "gwei",
          "amount": "25228800000000000"
        },
        "mining_reward": {
          "denom": "gwei",
          "amount": "40000000000"
        },
        "block_chain_percentage_in_bp": "2000",
        "resource_node_percentage_in_bp": "6200",
        "meta_node_percentage_in_bp": "1800"
      },
      {
        "total_mined_valve_start": {
          "denom": "gwei",
          "amount": "25228800000000000"
        },
        "total_mined_valve_end": {
          "denom": "gwei",
          "amount": "29433600000000000"
        },
        "mining_reward": {
          "denom": "gwei",
          "amount": "20000000000"
        },
        "block_chain_percentage_in_bp": "2000",
        "resource_node_percentage_in_bp": "6400",
        "meta_node_percentage_in_bp": "1600"
      },
      {
        "total_mined_valve_start": {
          "denom": "gwei",
          "amount": "29433600000000000"
        },
        "total_mined_valve_end": {
          "denom": "gwei",
          "amount": "31536000000000000"
        },
        "mining_reward": {
          "denom": "gwei",
          "amount": "10000000000"
        },
        "block_chain_percentage_in_bp": "2000",
        "resource_node_percentage_in_bp": "6600",
        "meta_node_percentage_in_bp": "1400"
      },
      {
        "total_mined_valve_start": {
          "denom": "gwei",
          "amount": "31536000000000000"
        },
        "total_mined_valve_end": {
          "denom": "gwei",
          "amount": "32587200000000000"
        },
        "mining_reward": {
          "denom": "gwei",
          "amount": "5000000000"
        },
        "block_chain_percentage_in_bp": "2000",
        "resource_node_percentage_in_bp": "6800",
        "meta_node_percentage_in_bp": "1200"
      },
      {
        "total_mined_valve_start": {
          "denom": "gwei",
          "amount": "32587200000000000"
        },
        "total_mined_valve_end": {
          "denom": "gwei",
          "amount": "40000000000000000"
        },
        "mining_reward": {
          "denom": "gwei",
          "amount": "2500000000"
        },
        "block_chain_percentage_in_bp": "2000",
        "resource_node_percentage_in_bp": "7000",
        "meta_node_percentage_in_bp": "1000"
      }
    ],
    "bond_denom": "wei",
    "reward_denom": "gwei",
    "mature_epoch": "2016",
    "community_tax": "20000000000000000",
    "initial_total_supply": {
      "denom": "wei",
      "amount": "100000000000000000000000000"
    }
  }
}
```

<br>


### - TotalMinedToken

Get total mined token

Request:

```
grpcurl -plaintext 127.0.0.1:9090 stratos.pot.v1.Query.TotalMinedToken
```

Response:

```json
{
  "total_mined_token": {
    "denom": "gwei",
    "amount": "959999999923"
  }
}
```

<br>


### - CirculationSupply

Get circulation supply

Request:

```
grpcurl -plaintext 127.0.0.1:9090 stratos.pot.v1.Query.CirculationSupply
```

Response:
```json
{
  "circulation_supply": [
    {
      "denom": "wei",
      "amount": "441331088285529367702468752"
    }
  ]
}
```

<br>

---

<br>