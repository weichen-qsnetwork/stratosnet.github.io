---
title: Full-Chain Node
description: This document describes how to setup and run a Stratos-chain full-node in testnet. It offers sample code, implementation tips and reference material.
---

## Introduction

Stratos blockchain facilitates all decentralized ledger transactions and functionalities, providing settlement services and related financial payment services for network providers and network users in an efficient, fair and transparent manner.

The Stratos-chain full-nodes are dedicated servers with sufficient computing power that participate in block generation cycle. It is necessary in order to be a validator.

In practice, running a full-node only implies running a non-compromised and up-to-date version of the software with low network latency and without downtime. It is encouraged to run a full-node even if you do not plan to be a validator.

The Stratos-chain validator is a full-node that participates in the Stratos Chain block generation cycle and also voting for the validity of a block proposed.


<br>

---

## Requirements

Here are the required hardware/software to run a Stratos-chain full-node:

<b>Minimum Hardware Requirements</b>

| CPU | RAM | Storage | Stake |
| --- | --- | ------- | ----- |
| 8 Cores[¹](#requirements), 2.5GHz[²](#requirements) | 32 GB | 2 TB | 1 STOS[³](#requirements) |

<small> ¹ &nbsp;&nbsp; Can be achieved using dual CPU server configurations (eg. 2cpu x 8cores, as long as the frequency per core is respected).<br>
² &nbsp;&nbsp; 2.5GHz refers to Base Frequency, not Turbo/Boost Frequency. <br>
³ &nbsp;&nbsp; Minimum stake is 1 stos until all 100 validator spots are filled. After that, is marked decided.</small>

<b>Software (tested version)</b>

* Ubuntu 18.04+
* Go 1.20+ linux/amd64


<br>

---

## Setup Environment

In order to run a Stratos-chain full-node, you may need to build `stratos-chain` source code yourself which requires `Go 1.19+`, `git`, `curl` and `make` installed.

This process depends on your operating system.

<br>

### Linux Users
 

The following example is based on Ubuntu 18.04+ 64-bit(Debian) and assumes you are using a terminal environment by default.

Please run the equivalent commands if you are running other Linux distributions.

```shell
# Update the system
sudo apt update
sudo apt upgrade
    
# Install git, snap and make(you can also install them separately as your needs)
sudo apt install git build-essential curl tmux libgmp3-dev flex bison --yes
    
# Install PBC library
wget https://crypto.stanford.edu/pbc/files/pbc-0.5.14.tar.gz
tar xfz pbc-0.5.14.tar.gz && cd pbc-0.5.14
./configure
make
sudo make install
sudo ldconfig
```

<br>

### Windows Users


It is possible to build and run the software on Windows. However, we did not test it on Windows completely.

It may give you unexpected results, or it may require additional setup.

An alternative option is to install a separate virtual Linux system using [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or [VMware Workstation](https://www.vmware.com/ca/products/workstation-player/workstation-player-evaluation.html).

<br>

---

## Setup a Stratos-chain full-node

<br>

### Create a user account

To create a separated and more secure environment, it is recommended to create a separated user account `stratos` to run your node.

```shell
sudo adduser stratos --home /home/stratos
```

Once the user account `stratos` is created, switch and login the system using `stratos`. You will proceed with the following steps in context of that user.

<br>

### Get release files

!!! tip

    There are two ways to get the these binary executables:

    - Download pre-compiled executabled (for Ubuntu 18.04+ x86_64).
    - Download source code and compile it yourself.
    
    Please choose only one of them based on your operating system.

<br>

#### Pre-compiled executables

The following binary `stchaind` has been built and ready to be downloaded directly.

```shell
# Make sure we are inside the $HOME folder
cd $HOME
wget https://github.com/stratosnet/stratos-chain/releases/download/v0.12.0/stchaind
```

<br>

- Check the granularity

```shell
# Make sure we are inside the $HOME folder and check these two binary executables
cd $HOME

# Check granularity
md5sum stchaind

## Expected output
## 0d4a0fd5173fa273f6150b28e48086a3  stchaind
```

<br>

- Add execute permission to this binary

```shell
# Make sure the file can be executed
chmod +x stchaind
```

<br>

- Add the binary to the search path

```shell
mkdir ~/bin 
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.profile 
source ~/.profile
mv stchaind ~/bin 
```

- Verify installation

```shell
stchaind version

# Should return v0.12.0
```

!!! tip

    💡 This binary is built for Ubuntu 18.04+ amd64. if you have other Linux kernels or you have any issues with the pre-compiled binary please, follow the next step to build your own binary from source code.

    Otherwise, continue with [Networks](#networks).


<br>

#### Compile the source code

Before the following steps, please make sure you have `Go 1.19+` installed .

```shell
# Check if go is already installed:
go version

# If it's not, you can install it with snapd:
sudo apt install snapd
sudo snap install go --classic
```

Alternatively, you can follow the official instructions: [link](https://golang.org/doc/install)

<br>

- Build the extracted source code

```shell
git clone https://github.com/stratosnet/stratos-chain.git
cd stratos-chain
git checkout tags/v0.12.0
make build
```

<br>

- Installing the binary executable


```shell
mkdir ~/bin 
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.profile 
source ~/.profile
cp build/stchaind ~/bin 
```

- Verify installation

```shell
stchaind version

# Should return v0.12.0
```

<br>

---

### Networks

!!! important ""

    Currently, there are two live blockchains you can join:

    - `Mainnet` (Stratos) which is using real tokens and it's a production environment. 

    - `Testnet` (Mesos) which is using test tokens. You can setup a validator here at first if you want to test your system, see how things work, etc, without the risk of losing real tokens if something goes wrong.

    - This guide applies to both, with a few small differences:


    | Variable⤵  | Mainnet | Testnet |
    | :------- | :------- | :----- |
    | chain-id | stratos-1 | mesos-1 |
    | keyring-backend | file / os / pass | test |

### keyring-backend

!!! important ""

    On Testnet, the `keyring's backend` is `test`, i.e., `--keyring-backend=test`

    - The `test` backend is a password-less variation of the `file` backend. Keys are stored unencrypted on disk.

    On Mainnet, the `keyring's backend` can be `file`, `os` or `pass` e.g., `--keyring-backend=file`

    - The `file` backend stores the keyring encrypted within the app's configuration directory. This keyring will request a password each time it is accessed. (Recommended)
    - The `os` backend relies on operating system-specific defaults to handle key storage securely since operating system's default credentials managers are designed to meet users' most common needs and provide them with a comfortable experience without compromising on security.
    - The `pass` backend uses the pass utility to manage on-disk encryption of keys' sensitive data and metadata. Keys are stored inside gpg encrypted files within app-specific directories. More info at passwordstore.org

---

### Initialize the node


```sh
# Make sure we are inside the home directory
cd $HOME

# Create folders and initialize the node
stchaind init "<your_node_moniker>" --chain-id <network_chain_id>

# ignore the output since you need to download the genesis file 
```

!!! tip ""

    💡 You can choose any `your_node_moniker`. This will be your node name.

    💡 `network_chain_id` is `stratos-1` for Mainnet or `mesos-1` for Testnet. See [Networks](#networks).


<br>

- Download the `genesis.json` and `config.toml` files

=== "Mainnet"

    ```shell
    wget https://raw.githubusercontent.com/stratosnet/mainnet/main/genesis/genesis.json
    wget https://raw.githubusercontent.com/stratosnet/mainnet/main/config.toml
    ```

=== "Testnet"

    ```shell
    wget https://raw.githubusercontent.com/stratosnet/stratos-chain-testnet/main/genesis.json
    wget https://raw.githubusercontent.com/stratosnet/stratos-chain-testnet/main/config.toml
    ```

!!! tip ""

    💡 We strongly recommend using this downloaded `config.toml` for v0.12.0, instead of the ones for previous versions to avoid any mismatching. 

   <br>

- Change `moniker` in the downloaded `config.toml` file

Please change your node moniker by modifying the `config.toml` file. Open this file with an editor, search `moniker` (usually at Line #18) in the file to find the “moniker” field. 

Change it to any value you like. It’s your node name that will show on the network.

```shell
# A custom human readable name for this node
moniker = "<your_node_moniker>"
```
<br>

- Move the downloaded `config.toml` and `genesis.json` files to `stchaind` folder (default in `$HOME/.stchaind/config/`). Replace if you already have these files.

```shell
mv config.toml $HOME/.stchaind/config/
mv genesis.json $HOME/.stchaind/config/
```

<br>

---

## Directory structure

After you finished the above steps, your `$HOME` folder should include the following directories and files.

``` { .yaml .no-copy }
.
├── ...
├── .stchaind
│   ├── config
│   │   ├── app.toml
│   │   ├── config.toml
│   │   ├── genesis.json
│   │   ├── node_key.json
│   │   └── priv_validator_key.json
│   ├── data
│   │    └── priv_validator_state.json 
│   └── keyring-test
├── ...
```

!!! tip

    💡 By default, directory `.stchaind` will be created in the `$HOME` folder. The `.stchaind` folder contains the nodes` configurations and data.

<br>

---

## Start the full-chain node

!!! tip

    Joining the network at a later time will require your node to download all the past blocks which, depending on how far ahead the network is, could take hours or even days.

    Stratos Chain now supports StateSync which enables your node to use a snapshot of the current chain and start the sync from there, which will only take a couple of minutes.

    You can find the StateSync Doc [here](../how-to-start-with-state-sync/).

There are three ways to run your Stratos-chain full-node. 

Please choose ONE of them to start the node.

<br>

### In foreground


```shell
# Make sure we are inside the home directory
cd $HOME

# run your node
stchaind start

# Use `Ctrl+c` to stop the node.
```

<br>

### In background


```shell
# Make sure we are inside the home directory
cd $HOME

# run your node in backend
tmux new -s stchaind
stchaind start 
```

Use the following Linux Command to stop your node.

```shell
pkill stchaind
```

<br>

### As service

All below steps require root privileges

<br>

- Create the service file

Create the `/lib/systemd/system/stratos.service` file with the following content

```toml
[Unit]
Description=Stratos Chain Node
After=network-online.target

[Service]
User=stratos
ExecStart=/home/stratos/stchaind start --home=/home/stratos/.stchaind
Restart=on-failure
RestartSec=3
LimitNOFILE=8192

[Install]
WantedBy=multi-user.target
```

!!! tip

    💡 In the [service] section:

    - `User` is your system login username
    - `ExecStart` designates the absolute path to the binary `stchaind`
    - `--home` is the absolute path to your node folder.
    - We used the default values for these variables. If you use a different username, group or folder to hold your node data instead of the default values, please modify these values according to your situations. Make sure the above values are correct.

<br>

- Start your service

Once you have successfully created the service, you need to enable and start it by running

```shell
systemctl daemon-reload
systemctl enable stratos.service
systemctl start stratos.service
```

<br>

#### Service operations

- Check the service status

```shell
systemctl status stratos.service
```

- Check service log

```shell
journalctl -u stratos.service -f 

# exit with ctrl+c
```

- Stop the service

```shell
systemctl stop stratos.service
```

<br>

---

## Check node status

Once you start your full-node, it will connect to the peers and start syncing. You can check the status of the node by running the following command

```shell
# Check the status of the node
stchaind status
```

The output will be similar to

```json
stchaind status
{
    "NodeInfo": {
        "protocol_version": {
            "p2p": "8",
            "block": "11",
            "app": "0"
        },
        "id": "16a0758d175cbf5c08d41dffa73eb5c0190869ed",
        "listen_addr": "tcp://0.0.0.0:26656",
        "network": "test-chain",
        "version": "0.37.4",
        "channels": "40202122233038606100",
        "moniker": "node",
        "other": {
            "tx_index": "on",
            "rpc_address": "tcp://127.0.0.1:26657"
        }
    },
    "SyncInfo": {
        "latest_block_hash": "697A2DB243E5191C6D85285A2ADD4924526924969C6C70FE71827C9FE41D4373",
        "latest_app_hash": "E978F87BB23D351B853F5F0CF9FBBBA4464FF5D7CE3746BF3E2357F28CBCE041",
        "latest_block_height": "497",
        "latest_block_time": "2023-01-11T01:10:37.562405326Z",
        "earliest_block_hash": "139676534FECFA507D56A06B03BD528E70ACA6D4DB6560219707011966613DE4",
        "earliest_app_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
        "earliest_block_height": "1",
        "earliest_block_time": "2023-01-09T17:08:58.4890503Z",
        "catching_up": false
    },
    "ValidatorInfo": {
        "Address": "18A7169C1B427D994133F7B3D4504E92789DB37C",
        "PubKey": {
            "type": "tendermint/PubKeyEd25519",
            "value": "69gothWTE9FJBZ5gBjjSNhg8y/5SsI1hBaD81Dum7lo="
        },
        "VotingPower": "500000"
    }
}
```

If the `catching_up` value is `false` in the `sync_info` section, it means that you are fully synced.

If it is `true`, it means your node is still syncing.

<br>

---

## Setup a wallet


Once the node finishes catch-up, you are ready to operate your node for various transactions(tx) and queries.

In order to hold the tokens that you will later delegate to your validator node, or pay staking for your SDS resource node, first, you need to create a local wallet account.

<br>

### Create a new wallet

To create a new wallet account, type the following command


```shell
stchaind keys add <your wallet name> --hd-path="m/44'/606'/0'/0/0" --keyring-backend=<your chosen keyring backend>
```

!!! tip

    💡 Choose a keyring-backend suited for the network you are running this chain installation on. See [keyring-backend](#keyring-backend).

    💡 Enter a wallet name that you will easily remember. This name will be used inside other commands later.





After creating a new local wallet account, you will get its `address` and `pubkey`.

In addition, you will have a secret recovery phrase(mnemonic phrase) which can be used to recover an existing wallet account and should be kept secret.

Example:

``` { .yaml .no-copy }
stchaind keys add myWallet --hd-path="m/44'/606'/0'/0/0" --keyring-backend=test

- name: myWallet
type: local
address: st1x2c6gy4vr8alsyzuqr2x8x8xxtvs97sk3jt6dp
pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"A7HCZTlHEarBPabkOgId5SlyQKdqEsbXJHit7y9LXRy+"}'
mnemonic: ""


**Important** write this mnemonic phrase in a safe place.
It is the only way to recover your account if you ever forget your password.

venue chest pattern tool certain identify adult theme thing public foster promote pave topple thing uncle brisk suffer present popular envelope wrap holiday goddess
```

<br>

### Recover an existing wallet

If you already have a Stratos wallet account, you can recover it by typing the following command

```shell
stchaind keys add <your wallet name> --recover --hd-path="m/44'/606'/0'/0/0" --keyring-backend=<your chosen keyring backend>
```

!!! tip

    💡 Choose a keyring-backend suited for the network you are running this chain installation on. See [keyring-backend](#keyring-backend).

    💡 Enter a wallet name that you will easily remember. This name will be used inside other commands later.

Example:

```shell
stchaind keys add myWallet1 --recover --hd-path="m/44'/606'/0'/0/0" --keyring-backend=test  
```

<br>

After the above `keys add` command executed, a `keyring-*` folder will be created which contains your wallets' information with their addresses. 
 
 The `keyring-*` folder looks like

``` { .yaml .no-copy }
.
├── 32b1a412ac19fbf8105c00d46398e632d902fa16.address
├── d0c57269c450f81234307a33bd148ac4f90549e5.address
├── myWallet1.info
└── myWallet.info
```

<br>

### Check your wallet

There are two ways to check your local wallets

<br>

- Check all local wallet accounts

```shell
stchaind keys list --keyring-backend=<keyring's backend> 
```

Example:

``` { .properties .no-copy }
stchaind keys list --keyring-backend=test
   - address: st16uzr20lx072gexwjuvg94hz3t8y73u4085s9sw
     name: user0
     pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"A/wF15Wd3ogCXstE7S4Zf3DA4KXb0W7exQhP004PLTi3"}'
     type: local
   - address: st1dz20dmhjkuc2tur3amgl8t45w807a640leh8p0
     name: user1
     pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"AgnhB5EkHL8+jD0/zRDR11nIpfOirTRrjgCX6uibhmDW"}'
     type: local
   - address: st1lkcrz3ktt2p7ppu9arglpqcn94pcdd9a9pmatf
     name: user10
     pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"A2sZ2Z9BU9oDELC06Gj8Lfc5UycxTaPux3sEIq8sIzSW"}'
     type: local
   - address: st16czjk2ym0prgvy4gl970t84vrp96s5kayfqmf2
     name: user2
     pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"AwfcJTOVWdx6ai61cy8VGJ1SdWHzwm2CCmr/+PwSpFeR"}'
     type: local
   - address: st17patveqxcq42rguc7nayr2g3jtawpzvhfmmumt
     name: user3
     pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"AtFxbuB4s+2SYzImGPIBwe0H0mKCXbIPu1T63OvbgE/3"}'
     type: local
```

<br>

- Check a specific local wallet account

```shell
stchaind keys show <your wallet name> --keyring-backend=<keyring's backend> 
```

Example:

``` { .properties .no-copy }
stchaind keys show myWallet1 --keyring-backend=test
   - address: st16rzhy6wy2rupydps0gem69y2cnus2j09n42ksx
     name: myWallet1
     pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"A13YKi3/7p9FsFPTfVgxEO0YK8bnDHmBPfA3ID+k37ET"}'
     type: local
```

<br>

---

## Faucet

Faucet will only be available on Testnet to get test tokens into your wallet.

```shell
curl --header "Content-Type: application/json" --request POST --data '{"denom":"stos","address":"your wallet address"} ' https://faucet-mesos.thestratos.org/credit
```

!!! tip

    Replace "your wallet address" with your st1xx wallet address

    💡1stos = 1,000,000,000gwei = 1,000,000,000,000,000,000wei

<br>

- Check wallet account balance

You can query your account info using this command:

```shell
stchaind query account <your wallet address>
```

Example:

``` { .properties .no-copy }
stchaind query account st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx
|
'@type': /cosmos.auth.v1beta1.BaseAccount
account_number: "1"
address: st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx
pub_key: null
sequence: "0"
```

<br>

  You can query your wallet balances using this command:

```shell
stchaind query bank balances <your wallet address>
```

Example:

``` { .properties .no-copy }
stchaind query bank balances st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
|
balances:
- amount: "9998000000000000000"
  denom: wei
pagination:
next_key: null
total: "0
```

<br>

- Try your first tx - `send`


 This tx command will send an amount of tokens from one wallet address to another:

 ```shell
 stchaind tx bank send <from address> <to address> <amount> --keyring-backend=<keyring's backend> --chain-id=<current chain-id> --gas=auto --gas-prices=1000000000wei
 ```
 
!!! tip ""

    * For `chain-id` and `keyring-backend`, see [Networks](#networks).
    * Make sure your `<from address>` has enough tokens
    * Please wait for around 7 seconds for block generation after a transaction.

Example:

Let us assume:

* `from address`: st1dz20dmhjkuc2tur3amgl8t45w807a640leh8p0
* `to address`: st123wun5lnwerdrt0mk2uxtusgawpfr228a0sseg
* `amount`: 10stos

``` { .properties .no-copy }
stchaind tx bank send st1dz20dmhjkuc2tur3amgl8t45w807a640leh8p0 st123wun5lnwerdrt0mk2uxtusgawpfr228a0sseg 10stos \
--chain-id=mesos-1  --keyring-backend=test --gas=100000 --gas-prices=1000000000wei -y

code: 0
codespace: ""
data: ""
events: []
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: BA96CF87646592487ABB9DDDE8FA86FE71441226281B04E15C5C66EDE415FBC6
```

<br>

---

## Validator

At this point, you have a Full-Chain Node. 

Full-Chain nodes are also important to the network as they are able to handle queries from a client and provide scale for the validator. They are also able to mantain historical information about the state of the chain.

But they are not able to accept transactions from clients, validate them and insert them into the blockchain, like Validators do. So they won't be earning any rewards.

To convert your Full-Node to a Validator, please follow the next guide.

[How To Become a Validator](../how-to-become-a-validator/)

<br>

---

<br>