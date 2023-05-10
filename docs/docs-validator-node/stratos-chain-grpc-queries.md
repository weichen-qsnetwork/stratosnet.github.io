Cosmos SDK gRPC definitions have been documented [here](https://crypto.org/docs/resources/cosmos-grpc-docs.html#cosmos.auth.v1beta1.QueryAccountRequest)

## Register Module

### gRPC Gateway

| Method Name              | Request Type                                                                   | Response Type                                                                                                                   | Description                                                               |
|--------------------------|--------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| ResourceNode             | QueryResourceNodeRequest <br>fields:{"network_addr":string}                    | QueryResourceNodeResponse <br>fields:{"node":ResourceNode}                                                                      | Get info of a registered resource node                                    |
| MetaNode                 | QueryMetaNodeRequest <br>fields:{"network_addr":string}                        | QueryMetaNodeResponse <br>fields:{"node":MetaNode}                                                                              | Get info of a registered meta node                                        |
| Params                   | QueryParamsRequest <br>fields:{}                                               | QueryParamsResponse <br>fields:{"params":Params}                                                                                | Get params of Register Module                                             |
| StakeByNode              | QueryStakeByNodeRequest <br>fields:{"network_addr":string, query_type:uint32}  | QueryStakeByNodeResponse <br>fields:{"staking_info":StakingInfo }                                                               | Get staking info of a specific node                                       |                                  
| StakeByOwner             | QueryStakeByOwnerRequest <br>fields:{"owner_addr":string}                      | QueryStakeByOwnerResponse <br>fields:{"staking_infos":[]StakingInfo, <br>"pagination": cosmos.base.query.v1beta1.PageResponse } | Get all staking info of a specific owner                                  |
| StakeTotal               | QueryTotalStakeRequest <br>fields:{}                                           | QueryTotalStakeResponse <br>fields:{"totalStakes":TotalStakesResponse}                                                          | Query total staking state of all registered resource nodes and meta nodes |
| BondedResourceNodeCount  | QueryBondedResourceNodeCountRequest <br>fields:{}                              | QueryBondedResourceNodeCountResponse <br>fields:{"number": uint64}                                                              | Get params of Register Module                                             |
| BondedMetaNodeCount      | QueryBondedMetaNodeCountRequest <br>fields:{}                                  | QueryBondedMetaNodeCountResponse <br>fields:{"number": uint64}                                                                  | Get params of Register Module                                             |

**ResourceNode**:

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

**MetaNode**:

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

**Description**:

| Field            | Type   | Label |
|------------------|--------|-------|
| moniker          | string |       |
| identity         | string |       |
| website          | string |       |
| security_contact | string |       |
| details          | string |       |

**Params**:

| Field                     | Type                     | Label |
|---------------------------|--------------------------|-------|
| bond_denom                | string                   |       |
| unbonding_threashold_time | google.protobuf.Duration |       |
| unbonding_completion_time | google.protobuf.Duration |       |
| max_entries               | uint32                   |       |

**StakingInfo**:

| Field            | Type                              | Label |
|------------------|-----------------------------------|-------|
| network_address  | string                            |       |
| pubkey           | google.protobuf.Any               |       |
| suspend          | bool                              |       |
| status           | cosmos.staking.v1beta1.BondStatus |       |
| tokens           | string                            |       |
| owner_address    | string                            |       |
| description      | Description                       |       |
| creation_time    | google.protobuf.Timestamp         |       |
| node_type        | uint32                            |       |
| bonded_stake     | cosmos.base.v1beta1.Coin          |       |
| un_bonding_stake | cosmos.base.v1beta1.Coin          |       |
| un_bonded_stake  | cosmos.base.v1beta1.Coin          |       |

**TotalStakesResponse**:

| Field                      | Type                     | Label |
|----------------------------|--------------------------|-------|
| resource_nodes_total_stake | cosmos.base.v1beta1.Coin |       |
| meta_nodes_total_stake     | cosmos.base.v1beta1.Coin |       |
| total_bonded_stake         | cosmos.base.v1beta1.Coin |       |
| total_unbonded_stake       | cosmos.base.v1beta1.Coin |       |
| total_unbonding_stake      | cosmos.base.v1beta1.Coin |       |


<details>
    <summary><code> List </code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; List all available grpc queries in Register Module</summary>
<br>

Request:
```http request
 grpcurl -plaintext 127.0.0.1:9090 list stratos.register.v1.Query
```
Response:
```shell
stratos.register.v1.Query.ResourceNode
stratos.register.v1.Query.MetaNode
stratos.register.v1.Query.Params
stratos.register.v1.Query.StakeByNode
stratos.register.v1.Query.StakeByOwner
stratos.register.v1.Query.StakeTotal
stratos.register.v1.Query.BondedResourceNodeCount
stratos.register.v1.Query.BondedMetaNodeCount
```
</details>
<br>

<details>
    <summary><code> ResourceNode </code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Get info of a registered resource node</summary>
<br>

Request:
```http request
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
</details>
<br>


<details>
    <summary><code> MetaNode </code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Get info of a registered meta node</summary>
<br>

Request:
```http request
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
</details>
<br>


<details>
    <summary><code> Params</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Get params of Register Module</summary>
<br>

Request:
```http request
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
</details>
<br>


<details>
    <summary><code> StakeByNode </code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Get staking info of a specific node</summary>
<br>

Request:
```http request
grpcurl -plaintext -d '{"network_addr":"stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64","query_type": 1 }' 127.0.0.1:9090 stratos.register.v1.Query.StakeByNode
```
Response:
```json
{
 "stakingInfo": {
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
  "creationTime": "0001-01-01T00:00:00Z",
  "bondedStake": {
   "denom": "wei",
   "amount": "100000000000000000000"
  },
  "unBondingStake": {
   "denom": "wei",
   "amount": "0"
  },
  "unBondedStake": {
   "denom": "wei",
   "amount": "0"
  }
 }
}
```
</details>
<br>

<details>
    <summary><code> StakeByOwner </code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Get all staking info of a specific owner</summary>
<br>

Request:
```http request
grpcurl -plaintext -d '{"owner_addr":"st1a8ngk4tjvuxneyuvyuy9nvgehkpfa38hm8mp3x", "pagination": {"limit":20}}' 127.0.0.1:9090 stratos.register.v1.Query.StakeByOwner
```

Response:
```json
{
 "stakingInfos": [
  {
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
   "creationTime": "0001-01-01T00:00:00Z",
   "bondedStake": {
    "denom": "wei",
    "amount": "100000000000000000000"
   },
   "unBondingStake": {
    "denom": "wei",
    "amount": "0"
   },
   "unBondedStake": {
    "denom": "wei",
    "amount": "0"
   }
  }
 ],
 "pagination": {
  "total": "1"
 }
}
```
</details>
<br>

<details>
    <summary><code>StakeTotal</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Query total staking state of all registered resource nodes and meta nodes</summary>
<br>

Request:
```http request
 grpcurl -plaintext 127.0.0.1:9090 stratos.register.v1.Query.StakeTotal
```
Response:
```shell
{
    "totalStakes": {
        "resourceNodesTotalStake": {
            "denom": "wei",
            "amount": "0"
        },
        "metaNodesTotalStake": {
            "denom": "wei",
            "amount": "100000000000000000000"
        },
        "totalBondedStake": {
            "denom": "wei",
            "amount": "100000000000000000000"
        },
        "totalUnbondedStake": {
            "denom": "wei",
            "amount": "0"
        },
        "totalUnbondingStake": {
            "denom": "wei",
            "amount": "0"
        }
    }
}
```
</details>
<br>

<details>
    <summary><code>BondedResourceNodeCount</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Queries total number of Bonded ResourceNodes</summary>
<br>

Request:
```http request
grpcurl -plaintext 127.0.0.1:9090 stratos.register.v1.Query.BondedResourceNodeCount
```
Response:
```shell
{
  "number": "2"
}
```
</details>
<br>

<details>
    <summary><code>BondedMetaNodeCount</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Queries total number of Bonded MetaNodes</summary>
<br>

Request:
```http request
grpcurl -plaintext 127.0.0.1:9090 stratos.register.v1.Query.BondedMetaNodeCount
```
Response:
```shell
{
  "number": "4"
}
```
</details>
<br>


***

## SDS Module

### gRPC Gateway

| Method Name | Request Type                                                           | Response Type                                                         | Description                                                                 |
|-------------|------------------------------------------------------------------------|-----------------------------------------------------------------------|-----------------------------------------------------------------------------|
| Fileupload  | QueryFileUploadRequest <br>fields:{"file_hash":string}                 | QueryFileUploadResponse <br>fields:{"file_info":FileInfo}             | Query uploaded file info by hash                                            |
| SimPrepay   | QuerySimPrepayRequest <br>fields:{"amount":[]cosmos.base.v1beta1.Coin} | QuerySimPrepayResponse <br>fields:{"noz":string}                      | Simulate prepay to query the noz that can be purchased at the current price |
| NozPrice    | QueryNozPriceRequest <br>fields:{}                                     | QueryNozPriceResponse <br>fields:{"price":string}                     | Query the current price of noz                                              |
| NozSupply   | QueryNozSupplyRequest <br>fields:{}                                    | QueryNozSupplyResponse <br>fields:{"remaining":string,"total":string} | Query noz supply                                                            |
| Params      | QueryParamsRequest <br>fields:{}                                       | QueryParamsResponse <br>fields:{"params":Params}                      | Get params of SDS Module                                                    |

**FileInfo**:

| Field     | Type   | Label |
|-----------|--------|-------|
| height    | string |       |
| reporters | bytes  |       |
| uploader  | string |       |

**Params**:

| Field      | Type   | Label |
|------------|--------|-------|
| bond_denom | string |       |


<details>
    <summary><code> List </code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; List all available grpc queries in SDS Module</summary>
<br>

Request:
```http request
 grpcurl -plaintext 127.0.0.1:9090 list stratos.sds.v1.Query
```
Response:
```shell
stratos.sds.v1.Query.Fileupload
stratos.sds.v1.Query.SimPrepay
stratos.sds.v1.Query.NozPrice
stratos.sds.v1.Query.NozSupply
stratos.sds.v1.Query.Params
```
</details>
<br>

<details>
    <summary><code>Fileupload</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Query uploaded file info by hash</summary>
<br>

Request:
```http request
 grpcurl -plaintext -d '{"file_hash":"v05ahm51dd62ise3fo7ojqub90p0ql2c3jg37hk8"}' 127.0.0.1:9090 stratos.sds.v1.Query.Fileupload
```
Response:
```shell
{
    "file_info": {
        "height": "4109",
        "reporters": "DwAAAAAAAAA=",
        "uploader": "st18986jyng5vsprmtzkdxla80jrw7qyc6wl73h0u"
    }
}
```
</details>
<br>


<details>
    <summary><code>SimPrepay</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Simulate prepay to query the noz that can be purchased at the current price</summary>
<br>

Request:
```http request
 grpcurl -plaintext -d '{"amount":[{"amount":"100000000000","denom":"wei"}]}' 127.0.0.1:9090 stratos.sds.v1.Query.SimPrepay
```
Response:
```shell
{
    "noz": "98736"
}
```
</details>
<br>


<details>
    <summary><code>NozPrice</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Query the current price of noz</summary>
<br>

Request:
```http request
 grpcurl -plaintext 127.0.0.1:9090 stratos.sds.v1.Query.NozPrice
```
Response:
```shell
{
    "price": "1012791644248016784459322"
}
```
</details>
<br>


<details>
    <summary><code>NozSupply</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Query noz supply</summary>
<br>

Request:
```http request
 grpcurl -plaintext 127.0.0.1:9090 stratos.sds.v1.Query.NozSupply
```
Response:
```shell
{
    "remaining": "7949398620856330560",
    "total": "8000080000000000000"
}
```
</details>
<br>


<details>
    <summary><code>Params</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Get params of SDS Module</summary>
<br>

Request:
```http request
 grpcurl -plaintext 127.0.0.1:9090 stratos.sds.v1.Query.Params
```
Response:
```shell
{
    "params": {
        "bond_denom": "wei"
    }
}
```
</details>
<br>



***

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


**ReportInfo**:

| Field     | Type    | Label |
|-----------|---------|-------|
| epoch     | int64   |       |
| reference | string  |       |
| tx_hash   | string  |       |
| reporter  | string  |       |

**Reward**:

| Field                    | Type                      | Label     |
|--------------------------|---------------------------|-----------|
| wallet_address           | string                    |           |
| reward_from_mining_pool  | cosmos.base.v1beta1.Coin  | repeated  |
| reward_from_traffic_pool | cosmos.base.v1beta1.Coin  | repeated  |

**RewardByOwner**:

| Field                 | Type                     | Label    |
|-----------------------|--------------------------|----------|
| wallet_address        | string                   |          |
| mature_total_reward   | cosmos.base.v1beta1.Coin | repeated |
| immature_total_reward | cosmos.base.v1beta1.Coin | repeated |

**Params**:

| Field                | Type                     | Label    |
|----------------------|--------------------------|----------|
| bond_denom           | string                   |          |
| reward_denom         | string                   |          |
| mature_epoch         | int64                    |          |
| mining_reward_params | MiningRewardParam        | repeated |
| community_tax        | string                   |          |
| initial_total_supply | cosmos.base.v1beta1.Coin |          |

**MiningRewardParam**:

| Field                          | Type                     | Label |
|--------------------------------|--------------------------|-------|
| total_mined_valve_start        | cosmos.base.v1beta1.Coin |       |
| total_mined_valve_end          | cosmos.base.v1beta1.Coin |       |
| mining_reward                  | cosmos.base.v1beta1.Coin |       |
| block_chain_percentage_in_bp   | string                   |       |
| resource_node_percentage_in_bp | string                   |       |
| meta_node_percentage_in_bp     | string                   |       |


<details>
    <summary><code> List </code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; List all available grpc queries in POT Module</summary>
<br>

Request:
```http request
 grpcurl -plaintext 127.0.0.1:9090 list stratos.pot.v1.Query
```
Response:
```shell
stratos.pot.v1.Query.VolumeReport
stratos.pot.v1.Query.RewardsByEpoch
stratos.pot.v1.Query.RewardsByOwner
stratos.pot.v1.Query.SlashingByOwner
stratos.pot.v1.Query.Params
stratos.pot.v1.Query.TotalMinedToken
stratos.pot.v1.Query.CirculationSupply
```
</details>
<br>


<details>
    <summary><code>VolumeReport</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Get pot volume report by epoch</summary>
<br>

Request:
```http request
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
</details>
<br>


<details>
    <summary><code>RewardsByEpoch</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Query pot reward by epoch </summary>
<br>

Request:
```http request
grpcurl -plaintext -d '{"epoch": 2, "wallet_address": "st18jxmc78ws5wq7q7umr6plpz8x0d9qtzu98v8em" }' 127.0.0.1:9090 stratos.pot.v1.Query.RewardsByEpoch
```
Response:
```json
{
  "rewards": [
    {
      "reward_from_mining_pool": [
        {
          "denom": "utros",
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
</details>
<br>


<details>
    <summary><code>RewardsByOwner</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Get pot reward by owner </summary>
<br>

Request:
```http request
grpcurl -plaintext -d '{"wallet_address": "st18jxmc78ws5wq7q7umr6plpz8x0d9qtzu98v8em"} ' 127.0.0.1:9090 stratos.pot.v1.Query.RewardsByOwner
```
Response:
```json
{
  "rewards": {
    "mature_total_reward": [],
    "immature_total_reward": [
      {
        "denom": "utros",
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
</details>
<br>



<details>
    <summary><code>SlashingByOwner</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Get pot slashing by owner </summary>
<br>

Request:
```http request
grpcurl -plaintext -d '{"wallet_address": "st16uzr20lx072gexwjuvg94hz3t8y73u4085s9sw"} ' 127.0.0.1:9090 stratos.pot.v1.Query.SlashingByOwner
```
Response:
```json
{
 "slashing": "0",
 "height": "2977"
}
```
</details>
<br>



<details>
    <summary><code>Params</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Get params of POT Module</summary>
<br>

Request:
```http request
 grpcurl -plaintext 127.0.0.1:9090 stratos.pot.v1.Query.Params
```
Response:
```json
{
  "params": {
    "mining_reward_params": [
      {
        "total_mined_valve_start": {
          "denom": "utros",
          "amount": "0"
        },
        "total_mined_valve_end": {
          "denom": "utros",
          "amount": "16819200000000000"
        },
        "mining_reward": {
          "denom": "utros",
          "amount": "80000000000"
        },
        "block_chain_percentage_in_bp": "2000",
        "resource_node_percentage_in_bp": "6000",
        "meta_node_percentage_in_bp": "2000"
      },
      {
        "total_mined_valve_start": {
          "denom": "utros",
          "amount": "16819200000000000"
        },
        "total_mined_valve_end": {
          "denom": "utros",
          "amount": "25228800000000000"
        },
        "mining_reward": {
          "denom": "utros",
          "amount": "40000000000"
        },
        "block_chain_percentage_in_bp": "2000",
        "resource_node_percentage_in_bp": "6200",
        "meta_node_percentage_in_bp": "1800"
      },
      {
        "total_mined_valve_start": {
          "denom": "utros",
          "amount": "25228800000000000"
        },
        "total_mined_valve_end": {
          "denom": "utros",
          "amount": "29433600000000000"
        },
        "mining_reward": {
          "denom": "utros",
          "amount": "20000000000"
        },
        "block_chain_percentage_in_bp": "2000",
        "resource_node_percentage_in_bp": "6400",
        "meta_node_percentage_in_bp": "1600"
      },
      {
        "total_mined_valve_start": {
          "denom": "utros",
          "amount": "29433600000000000"
        },
        "total_mined_valve_end": {
          "denom": "utros",
          "amount": "31536000000000000"
        },
        "mining_reward": {
          "denom": "utros",
          "amount": "10000000000"
        },
        "block_chain_percentage_in_bp": "2000",
        "resource_node_percentage_in_bp": "6600",
        "meta_node_percentage_in_bp": "1400"
      },
      {
        "total_mined_valve_start": {
          "denom": "utros",
          "amount": "31536000000000000"
        },
        "total_mined_valve_end": {
          "denom": "utros",
          "amount": "32587200000000000"
        },
        "mining_reward": {
          "denom": "utros",
          "amount": "5000000000"
        },
        "block_chain_percentage_in_bp": "2000",
        "resource_node_percentage_in_bp": "6800",
        "meta_node_percentage_in_bp": "1200"
      },
      {
        "total_mined_valve_start": {
          "denom": "utros",
          "amount": "32587200000000000"
        },
        "total_mined_valve_end": {
          "denom": "utros",
          "amount": "40000000000000000"
        },
        "mining_reward": {
          "denom": "utros",
          "amount": "2500000000"
        },
        "block_chain_percentage_in_bp": "2000",
        "resource_node_percentage_in_bp": "7000",
        "meta_node_percentage_in_bp": "1000"
      }
    ],
    "bond_denom": "wei",
    "reward_denom": "utros",
    "mature_epoch": "2016",
    "community_tax": "20000000000000000",
    "initial_total_supply": {
      "denom": "wei",
      "amount": "100000000000000000000000000"
    }
  }
}
```
</details>
<br>


<details>
    <summary><code>TotalMinedToken</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Get total mined token </summary>
<br>

Request:
```http request
grpcurl -plaintext 127.0.0.1:9090 stratos.pot.v1.Query.TotalMinedToken
```
Response:
```json
{
  "total_mined_token": {
    "denom": "utros",
    "amount": "959999999923"
  }
}
```
</details>
<br>


<details>
    <summary><code>CirculationSupply</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Get circulation supply </summary>
<br>

Request:
```http request
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
</details>
<br>
