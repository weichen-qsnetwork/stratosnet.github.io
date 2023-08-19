---
title: Setup and run a SDS Resource Node
description: How to setup and run a Stratos SDS Resource node. Tutorial and examples.
---


The Stratos Decentralized Storage (SDS) network is a scalable, reliable, self-balancing elastic acceleration network. We can simply take it as a decentralized file system suitable for running on general-purpose hardware.

SDS is composed of many Resource Nodes that store data, and a few Meta Nodes that coordinate with each other.

Note that provides their resource(disk/bandwidth/computation power) for SDS is called Resource Node.

---

## Requirements

<br>

- <b>Minimum Hardware Requirements</b>

| Type | CPU | RAM | Storage | Bandwidth | Stake |
| ---- | --- | --- | ------- | --------- | ----- |
| TIER 1 | 8 Cores[¹](#Requirements), 2.5GHz[²](#Requirements) | 16 GB | 4 TB | Up: 50Mbps Down: 100Mbps | 800 STOS |
| TIER 2 | 16 Cores[¹](#Requirements), 2.5GHz[²](#Requirements) | 32 GB | 8 TB | Up: 100Mbps Down: 100Mbps | 1600 STOS |
| TIER 3 | 32 Cores[¹](#Requirements), 2.5GHz[²](#Requirements) | 64 GB | 16 TB | Up: 1Gbps Down 1Gbps | 3200 STOS |

<small> ¹ &nbsp;&nbsp; Can be achieved using dual CPU server configurations (eg. 2cpu x 4cores, 2cpu x 8cores, etc, as long as the frequency per core is respected).<br>
² &nbsp;&nbsp; 2.5GHz refers to Base Frequency, not Turbo/Boost Frequency. </small>

<br>

- <b>Software(tested version)</b>

    * Ubuntu 18.04+
    * Go 1.19 linux/amd64

---

## Keywords

There are some keywords that are widely used in SDS. We describe them as

* `resource node`(`PP` node): Node that participates in the Stratos Resource Network by providing their disk/bandwidth/computation power to earn rewards in the Proof-of-Traffic(PoT) model.

* `meta node`(`SP` nodes): Node that manages the tasks in the Resource Network between resource nodes, including indexing all content, auditing the traffic report and communicating between Resource Network and Stratos-chain through a relay mechanism.

* `active resource node`: A resource node that has been activated by depositing to the Stratos-chain and registering to a meta node. It is ready to receive tasks assigned by the meta node.

* `suspended resource node`: A resource node that has not satisfied the performance KPI evaluation criteria and is suspended from receiving further tasks from the meta node.

* `traffic`: The data volume evaluated in the Resource Network. The incentive for all participants in the Stratos Ecosystem is based on traffic.

* `STOS`(Stratos Tokens): The native token facilitating value circulation in Stratos Ecosystem.

* `ozone`(oz): The traffic unit used in Stratos Ecosystem.

* `epoch`: The Proof-of-Traffic evaluation periodic window. The traffic for the Resource Network is evaluated at the end of each epoch.

* `value network`: The Stratos-chain, the network that circulates all values in the Stratos Ecosystem.

---

## Setup Environment

!!! tip

    In order to run an SDS resource node, you need to build SDS source code which requires `Go 1.19`, `git`, `curl` and `make` installed.
    
    If you have installed them previously, just skip this section. Otherwise, please install them as the following

This process depends on your operating system.

<br>

- <b>Linux Users</b>


The following example is based on Ubuntu 18.04+ 64-bit(Debian) and assumes you are using a terminal environment by default.
Please run the equivalent commands if you are running other Linux distributions.

```shell
# Update the system
sudo apt update
sudo apt upgrade
    
# Install git, snap and make(you can also install them separately as your needs)
sudo apt install git build-essential curl --yes

# Prepare binary PATH:
mkdir ~/bin
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.profile
source ~/.profile
```

!!! warning
    Due to a conflict with one of our 3rd party dependencies, for the moment only Go version 1.19 can be used.

To install Go 1.19, please follow these steps:

```shell
# If you already have Go installed, check if it's version 1.19.xx:
go version

# If you have 1.18.xx or 1.20.xx, remove it using the same method you installed with. For example:
sudo snap remove go
sudo apt remove golang-go
```

Install Go 1.19:

```shell
# Do a clean-up:
sudo rm -rf /usr/local/go

# Download the Go Binary Package:
wget https://go.dev/dl/go1.19.12.linux-amd64.tar.gz

# Unzip it to /usr/local directory:
sudo tar -C /usr/local -xzf go1.19.12.linux-amd64.tar.gz

# Add the Go PATH:
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.profile
source ~/.profile

# Verify with
go version

# You should see:
# go version go1.19.12 linux/amd64

```

<br>

- <b>Windows Users</b>


It is possible to build and run the software on Windows. However, we did not test it on Windows completely.
It may give you unexpected results, or it may require additional setup.

An alternative option is to install a separate virtual Linux system using [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or [VMware Workstation](https://www.vmware.com/ca/products/workstation-player/workstation-player-evaluation.html)

---

## Compile SDS resource node

<br>

- Compile the binary executables with source code


```shell
git clone https://github.com/stratosnet/sds.git
cd sds
git checkout tags/v0.10.0
make build
```

<br>

- Installing the binary executable

Once the compilation is successful, you will find three binary executables (ppd, relayd, sdsweb) under the `target` folder. Copy them to your home binary path:

```shell
cp target/* ~/bin
```

<br>

- Verify the installation with:

```shell
ppd version
```

You should get `v0.10.0`.

---

## Create SDS resource node

<br>

### Create a root directory

To start a resource node, you need to be in a directory dedicated to your resource node.

Create a new directory, or go to the root directory of your existing node.

In the following instruction, we assume you have entered the root directory of the resource node.

```shell
# create a new folder 
cd $HOME
mkdir rsnode
# Make sure we are inside the root directory of the resource node
cd rsnode
```

<br>

### Configure SDS resource node

Next, you need to generate the configuration file and its accounts of this resource node. The command `ppd config` will help you to generate necessary configurations.

<br>

#### Generate/Recover wallet


The `ppd config` command consists of several flags or subcommand. Let take a look at its general definition using `ppd config -h`.

``` { .yaml .no-copy }
ppd config -h

create default configuration file

Usage:
  ppd config [flags]
  ppd config [command]

Available Commands:
  accounts    create accounts for the node

Flags:
  -p, --create-p2p-key   create p2p key with config file, need interactive input
  -w, --create-wallet    create wallet with config file, need interactive input
  -h, --help             help for config

Global Flags:
  -c, --config string   configuration file path  (default "./configs/config.toml")
  -r, --home string     path for the node (default "<root directory of your resource node>")

Use "ppd config [command] --help" for more information about a command.
```

<br>

!!! tip

    There are two ways to generate a configuration file and create (or recover) a wallet:

    - Option 1 will generate a configuration file and a wallet automatically.

    - Option 2 will generate a configuration file and allow you to create or recover a wallet.



<br>

When asked to `input bip39 mnemonic`,

>Input your mnemonic -> recovers an existing wallet account;

>keep it blank -> generates a new wallet account

Usage:

```shell
# Make sure we are inside the root directory of the resource node
cd ~/rsnode
# to create config with interactive key creation
ppd config -w -p
```

<details>
  <summary><b>Example (creating a new wallet account)</b></summary>

You will get a new wallet account

```shell
# Make sure we are inside the root directory of the resource node
cd ~/rsnode
          
ppd config -w -p
[INFO]2022/02/14 10:19:09 setting.go:122: The config at location ./configs/config.toml does not exist
generating default config file
[INFO]2022/02/14 10:19:09 node.go:67: No P2P key specified in config. Attempting to create one...
Enter password:                           # enter your P2P key password
Enter password again:
[INFO]2022/02/14 10:19:29 setting.go:245: finished changing configuration file  P2PAddress:  stsdsp2p1k70qsfx70s2vkyn737hu3yj50ghrn5yz53dzwx
No wallet key specified in config. Attempting to create one...
Enter wallet nickname: wallet2            # enter your wallet nickname
Enter password:                           # enter your wallet key password
Enter password again:
       
# Leaving the following blank will generate a new wallet account automatically.

input bip39 mnemonic (leave blank to generate a new one)                # Press enter to generate a new wallet
input hd-path for the account, default: "m/44'/606'/0'/0/0" :           # Press enter
[INFO]2022/02/14 10:19:48 setting.go:245: finished changing configuration file  WalletAddress:  st1cmu0e9qlypg6j2ck8v5gfty6sxj2jszz84h8gf
save wallet password to config file: Y(es)/N(o): Y                      # Press Y
save the mnemonic phase properly for future recover:
=======================================================================  
interest liberty thrive maple trip fringe nurse deal fresh sport cool hip gate indoor brown mansion what table three wise design master warm apple
=======================================================================
```
</details>

<details>
  <summary><b>Example (recovering an existing wallet account)</b></summary>

You will get the same wallet account if you already have one.

```shell
# Make sure we are inside the root directory of the resource node
cd ~/rsnode
ppd config -w -p

[INFO]2022/02/22 11:25:19 setting.go:125: The config at location ./configs/config.toml does not exist
generating default config file
[INFO]2022/02/22 11:25:19 node.go:67: No P2P key specified in config. Attempting to create one...
Enter password:               # enter your P2P key password
Enter password again:
[INFO]2022/02/22 11:25:21 setting.go:251: finished changing configuration file  P2PAddress:  stsdsp2p1arpvg0wthng0yty06ay0uup3vw7k3upm586few
No wallet key specified in config. Attempting to create one...
Enter wallet nickname: wallet1            # enter your wallet nickname
Enter password:                           # enter your wallet key password
Enter password again:
        
# Inputting the mnemonic phrase here will recovering an existing wallet account(mnemonic will not show on the screen). 

input bip39 mnemonic (leave blank to generate a new one)         # Insert your mnemonic (seed phrase) 
input hd-path for the account, default: "m/44'/606'/0'/0/0" :    # Press enter
[INFO] 2022/02/22 11:25:34 setting.go:251: finished changing configuration file  WalletAddress:  st10t5chdnhx6myggwwhfq7q39hnjhzapau9yy6tv
save wallet password to config file: Y(es)/N(o): Y               # Press Y
save the mnemonic phase properly for future recover:
=======================================================================  
climb work able lock find blind fire cement exotic outdoor eyebrow panther repeat veteran prosper speak identify wolf mind decorate genre arctic bean gauge
=======================================================================
```

</details>

<br>

#### Directory structure

After the above command executed successfully, Your `rsnode` folder should include directories and files similar to the following.

``` { .yaml .no-copy }
.
├── accounts
│   ├── st10t5chdnhx6myggwwhfq7q39hnjhzapau9yy6tv.json
│   └── stsds1hez7aewx6srjtrw3064w3qy4dk22uv0cx7jxww.json
│   └── config.toml
└── tmp
  └── logs
      └── stdout.log

```
>   `accounts` folder keeps important account info, including the `Wallet Address`(starting with `st`) and `P2P Address`(starting with `stsds`) of your SDS resource node.
>
>   `configs` folder includes all configurations for this SDS resource node. User may need to modify `configs/config.toml` file to adapt to specific requirements of the network.
>
>   `tmp` folder is hols the logs and outputs.

<br>

#### Edit configuration file

You will need to edit a few lines in the file `configs/config.toml` to setup your node.

Open config file and make the following modifications:

```shell
nano config/config.toml
```

<br>

- <b>Edit the blockchain node address:</b>

```toml
# Network address of the chain Eg: "127.0.0.1:9090"
grpc_server = '52.196.88.238:9090'
```

<br>

- <b>Edit your external ip address:</b>

Replace `99.99.99.99` with your external ip address.

This ip address and port must be accessible from the Internet. If you are behind a router, the following port must be forwarded.

```shell
[node.connectivity]
# Is the node running on an internal network? Eg: false
internal = false
# IP address of the node. Eg: "127.0.0.1"
network_address = '99.99.99.99'
# Main port for communication on the network. Must be open to the internet. Eg: "18081"
network_port = '18081'
```

!!! tip

    To find your external ip, you can run the following command in another terminal:

    ```shell
    curl ifconfig.co
    ```


<br>

- <b>Edit the first meta node to connect on first run:</b>

```toml
# The first meta node to connect to when starting the node
[node.connectivity.seed_meta_node]
p2p_address = 'stsds15dchn80r73russ7pqjddvqdny0g9vyur8ckq7j'
p2p_public_key = 'stsdspub1wg99sp4rq4vz5w8ae8uaj9mw9de4x4q8d7mg57ukwwztme7g7jjqzffkw0'
network_address = '34.74.207.194:8888'
```

<br>

<details>
  <summary><b>Example of a full config file</b></summary>

```shell
[version]
# App version number. Eg: 9
app_ver = 10
# Network connections from nodes below this version number will be rejected. Eg: 9
min_app_ver = 10
# Formatted version number. Eg: "v0.9.0"
show = 'v0.10.0'

# Configuration of the connection to the Stratos blockchain
[blockchain]
# ID of the chain Eg: "tropos-5"
chain_id = 'mesos-1'
# Multiplier for the simulated tx gas cost Eg: 1.5
gas_adjustment = 1.3
# Connect to the chain using an insecure connection (no TLS) Eg: true
insecure = true
# Network address of the chain Eg: "127.0.0.1:9090"
grpc_server = '52.196.88.238:9090'

# Structure of the home folder. Default paths (eg: "./storage" become relative to the node home. Other paths are relative to the working directory
[home]
# Key files (wallet and P2P key). Eg: "./accounts"
accounts_path = './accounts'
# Where downloaded files will go. Eg: "./download"
download_path = './download'
# The list of peers (other sds nodes). Eg: "./peers"
peers_path = './peers'
# Where files are stored. Eg: "./storage"
storage_path = './storage'

[keys]
# Address of the P2P key. Eg: "stsdsxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
p2p_address = 'stsds1hfm5p3e3qmyc32rdayc02teqsgd608xnah8ytf'
p2p_password = ''
# Address of the stratos wallet. Eg: "stxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
wallet_address = 'st19waa7al5kwlnvrxgr8099ldgfa9s499h2hza2a'
wallet_password = ''

# Configuration of this node
[node]
# Should the node start mining automatically? Eg: true
auto_start = true
# Should debug info be printed out in logs? Eg: false
debug = true
# When not 0, limit disk usage to this amount (in megabytes) Eg: 1048576 (1 TB)
max_disk_usage = 1048576

[node.connectivity]
# Is the node running on an internal network? Eg: false
internal = false
# IP address of the node. Eg: "127.0.0.1"
network_address = '3.21.117.104'
# Main port for communication on the network. Must be open to the internet. Eg: "18081"
network_port = '18081'
# Port for prometheus metrics
metrics_port = '8765'
# Port for the JSON-RPC api. See https://docs.thestratos.org/docs-resource-node/sds-rpc-for-file-operation/
rpc_port = '8135'
# Enable the node owner RPC API. This API can manipulate the node status and sign txs with the local wallet. Do not open this to the internet  Eg: false
allow_owner_rpc = false

# The first meta node to connect to when starting the node
[node.connectivity.seed_meta_node]
p2p_address = 'stsds15dchn80r73russ7pqjddvqdny0g9vyur8ckq7j'
p2p_public_key = 'stsdspub1wg99sp4rq4vz5w8ae8uaj9mw9de4x4q8d7mg57ukwwztme7g7jjqzffkw0'
network_address = '34.74.207.194:8888'

# Configuration for the monitor server
[monitor]
# Should the monitor server use TLS? Eg: false
tls = false
# Path to the TLS certificate file
cert_file_path = ''
# Path to the TLS private key file
key_file_path = ''
port = '5433'

# Configuration for video streaming
[streaming]
# Port for the internal HTTP server
internal_port = '9708'
# Port for the REST server
rest_port = '9608'

[traffic]
# Interval at which traffic is logged (in seconds) Eg: 10
log_interval = 10
# Max number of concurrent network connections. Eg: 1000
max_connections = 1000
# Max number of download messages received per second (per connection). 0 Means unlimited. 1000 ≈ 1MB/sec. Eg: 1000
max_download_rate = 0
# Max number of upload messages sent per second (per connection). 0 Means unlimited. 1000 ≈ 1MB/sec. Eg: 1000
max_upload_rate = 0

# Configuration for the web server (when running sdsweb)
[web_server]
# Location of the web server files Eg: "./web"
path = './web'
port = '18181'
```

</details>

<br>

You can save and close the config file with Ctrl + X.

---

## Run SDS resource node

<br>

- Acquire test STOS tokens from (`faucet`)

Before manipulating your resource node, you need to acquire some STOS tokens.

You can get test tokens through the faucet API

```shell
curl --header "Content-Type: application/json" --request POST --data '{"denom":"stos","address":"your-wallet-address"} ' https://faucet-mesos.thestratos.org/credit
```

> Replace _your-wallet-address_ with the wallet address generated earlier.

<br>

- Start the SDS resource node

After setting up configuration properly, filled your wallet with some tokens, you can now start your resource node.

!!! tip

    `ppd start` must be running in the background at all times so it's recommended to start it in a tmux window by running this command first:

    ```shell
    tmux new -s rsnode
    ```

```shell
# Make sure we are inside the root directory of the resource node
cd ~/rsnode

# start the resource node
ppd start
```


---

## Interact with SDS resource node

<br>

- Interactive operation via `ppd terminal`

In order to interact with the resource node, you need to open A NEW COMMAND-LINE TERMINAL, and enter the root directory of the same resource node.

Then, use `ppd terminal` commands to start the interaction with resource node.

```shell
# Open a new command-line terminal
# Make sure we are inside the root directory of the same resource node
cd ~/rsnode

# Interact with resource node through a set of "ppd terminal" subcommands
ppd terminal
```

> `ppd terminal` needs to be executed in A NEW COMMAND-LINE TERMINAL (called as `ppd terminal` terminal).
>
> All `ppd terminal` [subcommands](../ppd-terminal-subcommands) should be executed in this `ppd terminal` terminal.


<br>

- <b>Registering the resource node to a meta node</b>

The resource node(PP) should be registered to a meta node(SP) before doing anything else.

In `ppd terminal`, input one of the two following identical subcommands

```shell
rp
  
# or

registerpeer
```

<br>
  
- <b>Uploading/Downloading files without registration</b>

You do not need to activate the node if you just want to upload/download files. 

After registering your resource node(`rp` subcommand), purchase enough `ozone` using the `prepay` subcommand. 

Then, use `put` or `get` subcommands to upload/download files.

```shell
prepay <amount> <fee> [gas]

# example: prepay 1stos 6000000gwei 6000000 
```
  
```shell
put <filepath>

# example: put /home/user/files/movie.mp4
    
get <sdm://account/filehash> [saveAs]  

# example: get sdm://st172v4u8ysfgaphjs8uyy0svvc6d6tzl6gp07kn4/v05ahm51l6v6tm2vqc682b9sicom61fgkoqdl0pg
```

This is a quick way for users to upload/download their files. Resource node can go offline at any time without being punished.
On the other hand, since the resource node is not activated, users will not receive mining rewards.

<br>

- Activating the resource node with deposit

You can activate your resource node by deposit an amount of tokens. 

After it is activated successfully,your resource node starts to receive tasks from meta nodes and thus gaining mining rewards automatically.

```shell
activate <amount> <fee> [gas] 
```

> `amount` is the amount of tokens you want to deposit. 1stos = 10^9gwei = 10^18wei.
>
> `fee` is the amount of tokens to pay as a fee for the activation transaction. 10000wei would work. It will use default value if no fee amount is provided.
>
> `gas` is the amount of gas to pay for the transaction. 1000000 would be a safe number. It will use default value if no gas amount is provided.

Example:

```shell
activate 2stos 0.01stos
```


---

## Other <code>ppd terminal</code> commands

Here we introduce a set of these `ppd terminal` subcommands in brief that you can try in the `ppd terminal` terminal.

Please refer to [ppd terminal subcommands](../ppd-terminal-subcommands) for more details.

<br>

- Check the current status of a resource node

```shell
status
```

<br>

- Update deposit of an active resource node, only increase deposit is allowed

```shell
updateDeposit <depositDelta> <fee> [gas]
```

> `depositDelta` is the absolute amount of difference between the original and the updated deposit. It should be a positive valid token, in the unit of `stos`/`gwei`/`wei`.
>
> When a resource node is suspended, use this command to update its state and re-start mining by increasing its deposit.

Example:

```shell
updateDeposit 1stos 1000000gwei 1000000
```

The above command will increase 1stos to deposit, use 1000000gwei for tx fee and 1000000 for tx gas.

<br>

- Purchase `ozone`

Ozone is the unit of traffic used by SDS. Operations involving network traffic require `ozone` to be executed.

You can purchase ozone with the following command:

```shell
prepay <amount> <fee> [gas]
```

> `amount` is the amount of stos tokens you want to spend to purchase ozone.

<br>

- Query ozone balance of a node's wallet

```shell
getoz <walletAddress>
```

<br>

- Create a new wallet, input password after prompt

```shell
newwallet <walletName> 
```

<br>

- Upload a file

```shell
put <filepath>
```

> `filepath` is the location of the file to upload, starting from your resource node folder. It is better to be an absolute path.

<br>

- Upload a media file for streaming

Streaming is the continuous transmission of audio or video files(media files) from a server to a client.

In order to upload a streaming file, first you need to install a tool `ffmpeg` for transcoding multimedia files.

<br>

In Linux Terminal:

```shell
sudo apt update
sudo apt install ffmpeg
    
# use ffmpeg -version to check its version
ffmpeg -version
```

<br>

In MacOS Terminal:

```shell
brew update
brew install ffmpeg
```

Then, back to the `ppd terminal` terminal, use `putstream` command to upload a media file

```shell
putstream <filepath>
```

> `filepath` is the absolute path of the file to be uploaded, or a relative path starting from the root directory of the resource node.

<br>

- List all uploaded files

  `list` or `ls`

```shell
ls
```

<br>

- Download an uploaded file

```shell
get <sdm://account/filehash> [saveAs]
```

!!! tip

      Every file uploaded to SDS is attributed with a unique file hash.
 
     You can view the file hash for each of your files when you `list` your uploaded files.
  
     Use the optional parameter `saveAs` to rename the file

     The downloaded file will be saved into `download` folder by default under the root directory of the SDS resource node.


<br>

- Delete an uploaded file


```shell
delete <filehash>
```

<br>

- Share(public) an uploaded file

```shell
  sharefile <filehash> <duration> <is_private>
```

> `duration` is time period(in seconds) when the file share expires. Put `0` for unlimited time.
>
> `is_private` is whether the file share should be protected by a password. Put `0` for public file without password, and `1` for private file with a password.
>
> After this command has been executed successfully, SDS will provide a password to this shared file, like `m216`. Please keep this password for future use.

<br>

- List All Shared Files

```shell
allshare
```

<br>

- Download a shared file


```shell
getsharefile <sharelink> [password]
```

> Leave the `password` blank if it's a public shared file.
>
> `sharelink` could be found when your use `allshare` to list all available shared files.
>
> The downloaded files will be saved into the folder `download` by default under the root directory of your resource node.

<br>

- Cancel file share

```shell
cancelshare <shareID> 
```

> `shareID` could be found when your use `allshare` to list all available shared files

<br>

- View resource utilization


```shell
# show the resource utilization monitor
monitor
    
# hide the resource utilization monitor
stopmonitor
```

---

## Check resource node status


There are a set of Restful APIs to check resource node status and Proof of Traffic(PoT) rewards.

You can input the following APIs in an explorer directly. We list some of them here and more details as well as examples can be found in [Stratos Chain REST APIs](../../docs-stratos-chain/stratos-chain-rest-apis/)

<br>

Check node registration status(`register` module)

<br>

- Query total deposit state of all registered resource nodes and meta nodes

```shell
https://rest-mesos.thestratos.org/register/deposit
```    

<br>

- Query params of `register` module

```shell
https://rest-mesos.thestratos.org/register/params
```    

<br>

- Get all deposit info of a specific owner

```shell
https://rest-mesos.thestratos.org/register/deposit/owner/{owner wallet address}
```    

<br>

- Get info of a registered resource node

```shell
https://rest-mesos.thestratos.org/register/resource-node/{resource node network address}
```    

<br>

- Get info of a registered meta node

```shell
https://rest-mesos.thestratos.org/register/meta-node/{meta node network address}
```    

<br>

- Get total number of registered resource nodes

```shell
https://rest-mesos.thestratos.org/register/resource-count
```    

<br>
  
- Get total number of registered meta nodes

```shell
https://rest-mesos.thestratos.org/register/meta-count
```    

<br>

## Check PoT rewards

  - Query PoT rewards of a wallet_address at a specific epoch

```shell
https://rest-mesos.thestratos.org/pot/rewards/wallet/{wallet_address}?epoch={epoch}
```    

<br>

- Query current Pot rewards of a wallet_address

```shell
https://rest-mesos.thestratos.org/pot/rewards/wallet/{wallet_address}
```  

<br>

- Query owner's Pot slashing info at a specific height

```shell
https://rest-mesos.thestratos.org/pot/slashing/{wallet_adress}?height={height}
```  

<br>

- Check SDS `prepay` and `Ozone`(`SDS` module)

- Get a simulated prepay result

```shell
https://rest-mesos.thestratos.org/sds/simulatePrepay/<amount of `wei` to prepay>
```    
<br>

- Get current nozPrice

```shell
https://rest-mesos.thestratos.org/sds/nozPrice
```    
<br>

- Get current nozSupply

```shell
https://rest-mesos.thestratos.org/sds/nozSupply
```    

<br>

---

</br>