# SDS Video Stream Handbook 
### Video file upload in HLS format
Streaming is the continuous transmission of audio or video files(media files) from a server to a client. 
SDS supports uploading `mp4` video file in Apple’s HLS (Http live streaming) format and can be later streamed back to the 
video player.

### Upload streaming file
There are two ways of uploading file, via command line tool or remote RPC api call.   
Please note that once a video file is uploaded via this command in streaming format, it is not allowed to be
downloaded via regular `get` command in the current version. Instead, it has to be played through the APIs that are
designed for playing streaming videos.

#### Requirements
1. To upload video in streaming format, a SDS resource node needs to be set up and joins the SDS. For detailed guideline on
   how to set up a resource node, please refer to
   [Setup and run a SDS Resource Node](https://github.com/stratosnet/stratosnet.github.io/blob/main/docs/docs-resource-node/setup-and-run-a-sds-resource-node.md)
2. It is also required to install a tool `ffmpeg` for transcoding multimedia files.  
   In Linux Terminal:
   ```shell
   sudo apt update
   sudo apt install ffmpeg
   
   # use ffmpeg -version to check its version
   ffmpeg -version
   ```
   In MacOS Terminal:

   ```shell
   brew update
   brew install ffmpeg
   ```

<br>


#### Upload via command line tool `ppd terminal`


In order to upload video stream by command line tool, you need to open A NEW COMMAND-LINE TERMINAL, and enter the root directory of the same resource node.

Then, use `ppd terminal` commands to start the interaction with resource node.

```shell
# Open a new command-line terminal
# Make sure we are inside the root directory of the same resource node
cd rsnode

# Interact with resource node through a set of "ppd terminal" subcommands
ppd terminal
```

Now, we can use the subcommand `putstream` to upload a media file

```shell
putstream <filepath>
```
> `filepath` is the absolute path of the file to be uploaded, or a relative path starting from the root directory of the resource node.

Example:
```shell
putstream example_01.mp4
```

#### Upload Via SDS RPC client
Media file can be uploaded in streaming format also via remote rpc call. For details regarding launching RPC client, please 
refer to the [RPC client doc](https://github.com/stratosnet/stratosnet.github.io/blob/main/docs/docs-resource-node/rpc-client.md).   
`putstream` is the command to be used for uploading media file in streaming format.
``` { .yaml .no-copy }
Usage:
  rpc_client putstream <filepath> [flags]

Flags:
  -h, --help   help for putstream

Global Flags:
  -u, --url string      url to the RPC server, e.g. http://3.24.59.6:8235 (default "http://127.0.0.1:4444")
  -w, --wallet string   wallet address to be used (default: the first wallet in folder ./account/)
```

Example:

```shell
rpc_client putstream /home/user/tmp/file_example_MP4_640_3MG.mp4 \
--url http://127.0.0.1:4444 \
--wallet st14rhrt576gvj6cl46tjn4pctghllmn63tm69e72
```


### Play and cache the video streaming file
#### Requirements
To play and cache the video steaming file, the resource node has to be a non-active resource node.
#### Play the cache the video streaming file
To stream the video, we use the videojs as the player in the front-end [template html file](https://github.com/stratosnet/sds/blob/main/pp/api/frontend/video_stream_template.html)
, however, any player that can play the HLS format file could be used. The way how the hls video streaming works with videojs, 
is to put the api url that gets the `.m3u8` file as the source and put `application/x-mpegURL` as the type. Then the player
will call the same url automatically with different url parameter to fetch video segment according to the metadata stored 
in `.m3u8` file. Once the client initiates a request to play the video file, the full fee is charged and the resource node also 
starts caching the whole streaming file on the local disk. Once the whole file is cached, the next play of the video will not 
download the file again from the SDS and no fee will be charged.

Steps:
1. Launch the non-active resource node and connects it to the SDS system
2. Open the [template html file](https://github.com/stratosnet/sds/blob/main/pp/api/frontend/video_stream_template.html) in
a text editor and replace the following variables in the template file
   - url - the url to the resource node server
   - internalPort - corresponds to the `internal_port` in the config of pp node
   - fileHash - file hash of the video file to be played
   - ownerWalletAddress - wallet address which owns the video file
3. Open the template html file again in chrome, and it will start playing the video. 