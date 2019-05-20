# Run XNode container as RPC node

For running the RPC node, you could run XNode container as below.

```bash
$ docker run -d -p 8282:28282 -p 7979:27979 -p 7978:27978 \
      -v $HOME/.xnode:/xnodedata xblockchain/xnode:1.0.0 \
      --rpcenable --rpcport "7979" \
--seedlist 07a26056b9a38f0588a8397685b9344606e4bc89@52.78.229.153:8282
```

The "--rpcenable" means it set the node as RPC node.

And "--rpcport" indicates RPC port number of the node.




Refer to XNODE Command line options for more command line options.

</br>






# Run XNode container as web socket node

For running the web socket node, you could run XNode container as below.

```bash
$ docker run -d -p 8282:28282 -p 7979:27979 -p 7978:27978 \
      -v $HOME/.xnode:/xnodedata xblockchain/xnode:1.0.0 \
      --rpcenable --rpcport "7979" \
--wsenable –wsport "7978" \
      --seedlist 07a26056b9a38f0588a8397685b9344606e4bc89@52.78.229.153:8282
```

The "--wsenable" means it set the node as web socket node.

And "--wsport" indicates web socket port number of the node.




Refer to XNODE Command line options for more command line options.

</br>

# XExplorer Quick Start

You can select the block chain by using chain ID in the web page(URL: http://xexplorer.xblockchain.tools:3000/chain).


<p align="center">
<img src="./images/blockchain_list.png">
</br>
</br>

Main viewer shows block/transaction information, governance rules for the block chain.


<p align="center">
<img src="./images/main_viewer.png">
</br>
</br>

The block list page provides the information for block’s hash, height, size and date.

You can get detail block information by selecting specific block.


<p align="center">
<img src="./images/block_list.png">
</br>
</br>

The transaction list page displays the information for transaction’s block, sender, receiver, amount, time.


<p align="center">
<img src="./images/transaction_list.png">
</br>
</br>

By choosing the transaction, it shows detail transaction information, which contains sender, receiver, amount, fee, transaction hash/type.


<p align="center">
<img src="./images/detail_transaction_viewer.png">
</br>

