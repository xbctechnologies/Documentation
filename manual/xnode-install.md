# Install/Start

### Download docker image
```bash
$ docker pull xblockchain/xnode:1.0.0
```

### Run xnode container
Create a data directory on a suitable volume on your host system, e.g. `$HOME/.xnode`.
And start your xnode container like this:

```bash
// Mainnet
$ docker run -d -p 8282:28282 -v $HOME/.xnode:/xnodedata \
      xblockchain/xnode:1.0.0
```

```bash
// Testnet
$ docker run -d -p 8282:28282 -v $HOME/.xnode:/xnodedata \
      xblockchain/xnode:1.0.0 \
      --testnet --seedlist bf17fa643c6997e03f581ab2004cd374c530f8b7@52.78.229.153:28282
```


The `-v $HOME/.xnode:/xnodedata` part of the command mounts the `$HOME/.xnode` directory from the underlying host system as `/xnodedata` inside the container, where xnode will write its block data and log files.

The `-p 8282:28282` part of the command forwards the `8282` port of host system to the `28282` port inside the container, which the xnode will accept connections from other xnode peers.

If you want for the xnode to run as RPC Server, you should add `-p {local_host_port}:27979` and append `--rpcenable` to the command.  
If you want for the xnode to run as WebSocket Server, you should add `-p {local_host_port}:27978` and append `--wsenable` to the command.  

For example, you could run xnode container like this:  
```bash
// Mainnet
$ docker run -d -p 8282:28282 -p 7979:27979 -p 7978:27978 -v $HOME/.xnode:/xnodedata \
      xblockchain/xnode:1.0.0 --rpcenable --wsenable
```

```bash
// Testnet
$ docker run -d -p 8282:28282 -p 7979:27979 -p 7978:27978 -v $HOME/.xnode:/xnodedata \
      xblockchain/xnode:1.0.0 --rpcenable --wsenable \
      --testnet --seedlist bf17fa643c6997e03f581ab2004cd374c530f8b7@52.78.229.153:28282
```

The `--seedlist ...` indicates a seed node for just **testnet** not mainnet.  
**The seed node of mainnet will be released on this site when the X.Blockchain's Mainnet is started.**  

Refer to [XNODE Command line options](#xnode-command-line-options) for more command line options.

</br>

### Run xnode container with Docker-compose

Create a data directory on a suitable volume on your host system, e.g. `$HOME/.xnode`.

```shell
$ mkdir -p $HOME/.xnode
```

To use xnode setup edit the `docker-compose.yml` file.
```
version: '3'

# same as
# docker run -d -p 8282:28282 -p 7979:27979 -p 7978:27978 \
#     -v $HOME/.xnode:/xnodedata xblockchain/xnode:1.0.0

services:
   xnode:
      image: xblockchain/xnode:1.0.0
      volumes:
         - $HOME/.xnode:/xnodedata
      ports:
         - 8282:28282
         - 7979:27979
         - 7978:27978
```


To use rpc xnode setup edit the `docker-compose.yml` file.
```
version: '3'

# same as
# docker run -d -p 8282:28282 -p 7979:27979 -p 7978:27978 \
#     -v $HOME/.xnode:/xnodedata xblockchain/xnode:1.0.0 \
#     --testnet --rpcenable --wsenable \
#     --seedlist 07a26056b9a38f0588a8397685b9344606e4bc89@52.78.229.153:8282

services:
   xnode:
      image: xblockchain/xnode:1.0.0
      volumes:
         - $HOME/.xnode:/xnodedata
      ports:
         - 8282:28282
         - 7979:27979
         - 7978:27978
      command:
         - "--testnet"
         - "--rpcenable"
         - "--wsenable"
         - "--seedlist=07a26056b9a38f0588a8397685b9344606e4bc89@52.78.229.153:8282"
```

To run an xnode docker cluster run the following:

```shell
$ docker-compose up -d
```
