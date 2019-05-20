## block_getBlock
블록의 해시값으로 블록 정보를 조회한다.

```bash
curl -X POST --data \
'{
 "jsonrpc": "1.0",
 "method": "block_getBlock",
 "id": 1,
 "params": [
 "1A", "0x1c6ae6a05751ade84f49ad134bf9292675a8899ca8ccf88cb6196b3d6613218f"
  ]
 }' 127.0.0.1:7979
```

```json
{
    "jsonrpc": "1.0",
    "id": 2,
    "result": {
        "chainID": "1A",
        "blockNo": 2,
        "blockHash": "0x1c6ae6a05751ade84f49ad134bf9292675a8899ca8ccf88cb6196b3d6613218f",
        "blockSize": 937,
        "transactionRoot": "0x",
        "transactions": [],
        "numTxs": 0,
        "total_txs": 0,
        "time": 1552369015001370910
    }
}
```

### Parameters
| Index | Parameter | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | targetChainId | string | mandatory | 조회하려는 계좌의 체인 id |
| 2 | blockHash | string | mandatory | 조회하려는 블럭의 해쉬값 |

### Returns
| Index | Key | Value | Type | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | chainId | 조회 요청한 체인 id | string | 해당 체인id (ex: A-assetChain, D-dataChain) |
| 2 | blockNo | 조회 요청한 블록 Height | int64 | 해당 블록넘버 |
| 3 | blockHash | 조회 요청한 블록 Hash | byte | 해당 블록해쉬 |
| 4 | blockSize | 조회 요청한 블록사이즈 | int | 해당 블록사이즈 |
| 5 | transactionRoot | 조회 요청한 블록 안에 든 transaction 들의 Hash | byte | 해당 블록에 포함된 트랜잭션들의 해쉬 |
| 6 | transactions | 조회 요청한 블록 안에 든 transaction Hash | [] byte | 해당 블록에 포함된 트랜잭션 배열의 해쉬 |
| 7 | numTxs | 조회 요청한 블록 안에 든 transaction 개수 | int64 | 해당 블록에 포함된 트랜잭션 개수 |
| 8 | total_txs | 조회 요청한 블록 기준, 현재까지 총 transaction 개수 | int64 | 해당 블록 기준, 총 트랜잭션 개수 |
| 9 | time | 조회 요청한 블록이 기록된 time | big integer | 해당 블록이 생성 될 때 시간 |

### Error Message
| Error Code | Message | Description |
| -------- | -------- | -------- |
| 0 | Height must be greater then 0 | 조회 요청한 블록넘버가 0보다 작거나 같은 경우, 조회 불가능 |
| 8 | Not found targetChainId | 조회 요청한 체인id 가 현재 노드에서 찾을 수 없는 경우 (–chainId 옵션 체크) |
| 342 | The latest block can not be verified. or not exist block | 조회 요청한 블록넘버가 현재 싱크받은 블록넘버보다 높은 경우, 조회 불가능 |


</br>

## block_getBlockByNumber
블럭의 Height 값으로 블럭 정보를 조회하며 block_getBlock와 동일한 값을 리턴한다.

```bash
curl -X POST --data \
'{
 "jsonrpc": "1.0",
 "method": "block_getBlockByNumber",
 "id": 1,
 "params": [
 "1A", 3
  ]
 }' 127.0.0.1:7979
```

```json
{
    "jsonrpc": "1.0",
    "id": 2,
    "result": {
        "chainID": "1A",
        "blockNo": 2,
        "blockHash": "0x1c6ae6a05751ade84f49ad134bf9292675a8899ca8ccf88cb6196b3d6613218f",
        "blockSize": 937,
        "transactionRoot": "0x",
        "transactions": [],
        "numTxs": 0,
        "total_txs": 0,
        "time": 1552369015001370910
    }
}
```

### Parameters
| Index | Parameter | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | targetChainId | string | mandatory | 조회하려는 계좌의 체인 id. (ex: A-assetChain, D-dataChain) |
| 2 | blockHeight (Number) | int64 | mandatory | 블록 Height |

### Returns
| Index | Key | Value | Type | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | chainId | 조회 요청한 체인 id | string | 해당 체인id (ex: A-assetChain, D-dataChain) |
| 2 | blockNo | 조회 요청한 블록 Height | int64 | 해당 블록넘버 |
| 3 | blockHash | 조회 요청한 블록 Hash | byte | 해당 블록해쉬 |
| 4 | blockSize | 조회 요청한 블록사이즈 | int | 해당 블록사이즈 |
| 5 | transactionRoot | 조회 요청한 블록 안에 든 transaction 들의 Hash | byte | 해당 블록에 포함된 트랜잭션들의 해쉬 |
| 6 | transactions | 조회 요청한 블록 안에 든 transaction Hash | [] byte | 해당 블록에 포함된 트랜잭션 배열의 해쉬 |
| 7 | numTxs | 조회 요청한 블록 안에 든 transaction 개수 | int64 | 해당 블록에 포함된 트랜잭션 개수 |
| 8 | total_txs | 조회 요청한 블록 기준, 현재까지 총 transaction 개수 | int64 | 해당 블록 기준, 총 트랜잭션 개수 |
| 9 | time | 조회 요청한 블록이 기록된 time | big integer | 해당 블록이 생성 될 때 시간 |

### Error Message
| Error Code | Message | Description |
| -------- | -------- | -------- |
| 0 | Height must be greater then 0 | 조회 요청한 블록넘버가 0보다 작거나 같은 경우, 조회 불가능 |
| 8 | Not found targetChainId | 조회 요청한 체인id 가 현재 노드에서 찾을 수 없는 경우 (–chainId 옵션 체크) |
| 342 | The latest block can not be verified. or not exist block | 조회 요청한 블록넘버가 현재 싱크받은 블록넘버보다 높은 경우, 조회 불가능 |

</br>

## block_getBlockLatestBlock
요청한 체인 ID의 가장 최신의 블럭정보를 조회한다.

```bash
curl -X POST --data \
'{
 "jsonrpc": "1.0",
 "method": "block_getBlockLatestBlock",
 "id": 1,
 "params": [
 "1A"
  ]
 }' 127.0.0.1:7979
```

```json
{
    "jsonrpc": "1.0",
    "id": 1,
    "result": {
        "chainID": "1A",
        "blockNo": 54855,
        "blockHash": "0x0d1498b75ca4ae8b8fc857f37387024ccfffbc50053405a34eac6ef4051cc75c",
        "blockSize": 947,
        "transactionRoot": "0x",
        "transactions": [],
        "numTxs": 0,
        "total_txs": 0,
        "time": 1552541734660176620
    }
}
```

### Parameters
| Index | Parameter | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | targetChainId | string | mandatory | 조회하려는 계좌의 체인 id. (ex: A-assetChain, D-dataChain) |

### Returns
| Index | Key | Value | Type | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | chainId | 조회 요청한 체인 id | string | 해당 체인id (ex: A-assetChain, D-dataChain) |
| 2 | blockNo | 조회 요청한 블록 Height | int64 | 해당 블록넘버 |
| 3 | blockHash | 조회 요청한 블록 Hash | byte | 해당 블록해쉬 |
| 4 | blockSize | 조회 요청한 블록사이즈 | int | 해당 블록사이즈 |
| 5 | transactionRoot | 조회 요청한 블록 안에 든 transaction 들의 Hash | byte | 해당 블록에 포함된 트랜잭션들의 해쉬 |
| 6 | transactions | 조회 요청한 블록 안에 든 transaction Hash | [] byte | 해당 블록에 포함된 트랜잭션 배열의 해쉬 |
| 7 | numTxs | 조회 요청한 블록 안에 든 transaction 개수 | int64 | 해당 블록에 포함된 트랜잭션 개수 |
| 8 | total_txs | 조회 요청한 블록 기준, 현재까지 총 transaction 개수 | int64 | 해당 블록 기준, 총 트랜잭션 개수 |
| 9 | time | 조회 요청한 블록이 기록된 time | big integer | 해당 블록이 생성 될 때 시간 |

### Error Message
| Error Code | Message | Description |
| -------- | -------- | -------- |
| 0 | Height must be greater then 0 | 조회 요청한 블록넘버가 0보다 작거나 같은 경우, 조회 불가능 |
| 8 | Not found targetChainId | 조회 요청한 체인id 가 현재 노드에서 찾을 수 없는 경우 (–chainId 옵션 체크) |
| 342 | The latest block can not be verified. or not exist block | 조회 요청한 블록넘버가 현재 싱크받은 블록넘버보다 높은 경우, 조회 불가능 |

</br>

## block_getBlockTxCount
선택한 블록내에서 처리되었던 트랜잭션 수를 조회

```bash

curl -X POST --data \
'{
 "jsonrpc": "1.0",
 "method": "block_getBlockTxCount",
 "id": 1,
 "params": [
 "1A", 3
  ]
 }' 127.0.0.1:7979
 
 
curl -X POST --data \
'{
 "jsonrpc": "1.0",
 "method": "block_getBlockTxCount",
 "id": 1,
 "params": [
 "1A", "0xd12163f2ea6cd3e4bfdb7a342cafa965991d58ef2ae5be56b1a7568a13ecb901"
  ]
 }' 127.0.0.1:7979
```

```json
{
    "jsonrpc": "1.0",
    "id": 2,
    "result": 0
}
```

### Parameters
| Index | Parameter | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | targetChainId | string | mandatory | 조회하려는 계좌의 체인 id. (ex: A-assetChain, D-dataChain) |
| 2 | blockHeight  | int64 | mandatory | 블록의 Height |
|  | blockHash  | string | mandatory | 블록의 해쉬값 |

### Returns
| Index | Type | Description |
| -------- | -------- | -------- | 
| 1 | long | 블럭내 처리된 트랜잭션 개수 |
