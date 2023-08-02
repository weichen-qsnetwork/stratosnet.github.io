# SDS IPFS Command Handbook
##IPFS Client
The `ppd ipfs` command launches an RPC-style API over HTTP client to allow user interact with a SDS Network. The API aligns 
with the Kubo RPC API of IPFS so that any application that supports IPFS Kubo RPC API could be updated to support SDS network
with little effort. The client needs to communicate with a SDS resource node to interact with the network. For setting up a 
SDS resource node please refer to  [Setup and run a SDS Resource Node](https://github.com/stratosnet/stratosnet.github.io/blob/main/docs/docs-resource-node/setup-and-run-a-sds-resource-node.md)

``` { .yaml .no-copy }
ppd ipfs -h

ipfs api server attached to node demon

Usage:
  ppd ipfs [flags]
  ppd ipfs [command]

Available Commands:
  migrate     migrate ipfs file to sds

Flags:
  -h, --help                 help for ipfs
      --httpRpcUrl string    http rpc url (default "http://127.0.0.1:9301")
      --ipcEndpoint string   ipc endpoint path
  -p, --port string          port (default "6798")
  -m, --rpcMode string       use httpRpc or ipc (default "ipc")

Global Flags:
  -c, --config string   configuration file path  (default "./config/config.toml")
  -r, --home string     path for the node (default "<root directory of your resource node>")

Use "ppd ipfs [command] --help" for more information about a command.

```

<br>

There are two modes to communicate to a SDS resouce node and it could be switched by the --rpcMode flag

 - `httpRpc` mode is to send RPC request over http. In this mode the `httpRpcUrl` flag must point to the rpc port of the 
resource node 
    ``` shell
    ppd ipfs --rpcMode httpRpc --httpRpcUrl http://<node url>:<node rpc port>
    ```
 - `ipc` mode is to send PRC request over IPC (Inter-process communication). The path to the ipc endpoint in mode is set 
    by the flag `ipcEndpoint` flag. The default path will be used when flag is not set.
    ```  shell
    ppd ipfs --rpcMode ipc --ipcEndpoint <path to the ipc endpoint>
    ```
<br>

##RPC Commands
###/api/v0/add
Upload a file to SDS.
Arguments
- arg [string]: The path to the file on the local driver. Required: yes.

####cURL Example
``` shell
curl -X POST  "localhost:6798/api/v0/add?arg=testfile"
```

###/api/v0/get
Download a file from SDS.
Arguments
- arg [string]: sdm path to the file in the SDS. The format is `<sdm://account/filehash>`. Required: yes.

!!! tip

    Every file uploaded to SDS is attributed with a unique file hash.

    View the file hash for each of your files when you `list` your uploaded files.

    The downloaded files will be saved into the folder `download` by default under the root directory of your resource node, like

####cURL Example
```shell
curl -X POST  "localhost:6798/api/v0/get?arg=sdm://st172v4u8ysfgaphjs8uyy0svvc6d6tzl6gp07kn4/v05ahm51l6v6tm2vqc682b9sicom61fgkoqdl0pg"
```

###/api/v0/list
Lists all files uploaded by account (wallet).
Arguments
- page [string]: page number. Each page contains 20 elements. Required: no.

####cURL Example
```shell
curl -X POST  "localhost:6798/api/v0/list?page=0"
```

##IPFS Migrate
The `ppd ipfs migrate` command migrates a file from IPFS to SDS network. It first downloads the file from the IPS by the 
given CID and then uploads it to the SDS network.

```shell
ppd ipfs migrate <filecid> <filename>
```
`filecid` is the cid of the file to downloaded from IPFS.  
`filename` is an optional parameter. When it is given, the file will be renamed to the given `filename` before it is uploaded 
to the SDS network.

Example:
```shell
ipfs -m httpRpc migrate QmSgvgwxZGaBLqkGyWemEDqikCqU52XxsYLKtdy3vGZ8uq spaceship.jpg
```
