## network_getPeerCnt
현재 연결하는 노드에 연결되어있는 총 노드의 갯수를 조회한다.

```bash
curl -X POST --data \
'{
 "jsonrpc": "1.0",
 "method": "network_getPeerCnt",
 "id": 1,
 "params": [
 "1A"
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

### Returns
| Index | Type | Description |
| -------- | -------- | -------- |
| 1 | long | 연결되어 있는 주변 노드(peer)의 개수 |

</br>

## network_getPeers
현재 연결하는 노드에 연결되어있는 총 노드의 정보를 조회한다.

```bash
curl -X POST --data \
'{
 "jsonrpc": "1.0",
 "method": "network_getPeers",
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
    "result": [
        {
            "id": "7cbb09f7d5a5b3321e0fb36e3e353c36d3f5f39b",
            "listenAddr": "192.168.20.207:9002",
            "network": "IntraNet"
        },
        {
            "id": "88faa4dd087f9c031017a9f1ef970170bd77d18a",
            "listenAddr": "192.168.20.207:9003",
            "network": "IntraNet"
        },
        {
            "id": "90c7978a1cde78986786ce9f6e62ae49486575ef",
            "listenAddr": "192.168.20.207:9004",
            "network": "IntraNet"
        },
        {
            "id": "90353fb94aa537e047e19e031ccb8863d2224f8c",
            "listenAddr": "192.168.20.207:26656",
            "network": "IntraNet"
        }
    ]
}
```

### Parameters
| Index | Parameter | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | targetChainId | string | mandatory | 조회하려는 계좌의 체인 id |

### Returns
| Index | Key | Type | Description |
| -------- | -------- | -------- | -------- |
| 1 | id | string | 노드의 id |
| 2 | listenAddr | string | p2p 네트웍 url |
| 3 | network | string | 네트웍 이름 |

</br>

## network_addPeer
요청한 노드에게 파라미터로 준 노드와의 피어연결을 하고 address book목록에도 추가한다. 

```bash
curl -X POST --data \
'{
 "jsonrpc": "1.0",
 "method": "network_addPeer",
 "id": 1,
 "params": [
 "1A", ["7ec442b039eb47ed4e84b4cff61deb8dc2e5d059@localhost:9003"],false
  ]
 }' 127.0.0.1:7979
```

```json
{
    "jsonrpc": "1.0",
    "id": 2,
    "result": true
}
```

### Parameters
| Index | Parameter | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | targetChainId | string | mandatory | 조회하려는 계좌의 체인 id |
| 2 | peers | string list | mandatory | 추가하고자 하는 peer노드의 ID@[ip]:[port] 형태의 문자열 리스트 |
| 3 | persistence | bool | mandatory | persistence 노드 추가할지 여부 |

### Returns
| Index | Key | Type | Description |
| -------- | -------- | -------- | -------- |
| 1 | result | bool | 피어노드로 추가했는지 여부 |

</br>

## network_removePeer
요청한 노드에게 파라미터로 준 노드와의 네트웍 연결을 끊고 address book목록에도 제외한다. 

```bash
curl -X POST --data \
'{
 "jsonrpc": "1.0",
 "method": "network_removePeer",
 "id": 1,
 "params": [
 "1A", ["7ec442b039eb47ed4e84b4cff61deb8dc2e5d059@localhost:9004"]
  ]
 }' 127.0.0.1:7979
```

```json
{
    "jsonrpc": "1.0",
    "id": 2,
    "result": true
}
```

### Parameters
| Index | Parameter | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | targetChainId | string | mandatory | 조회하려는 계좌의 체인 id |
| 2 | peers | string list | mandatory | 추가하고자 하는 peer노드의 ID@[ip]:[port] 형태의 문자열 리스트 |

### Returns
| Index | Key | Type | Description |
| -------- | -------- | -------- | -------- |
| 1 | result | bool | remove 되었는지 여부 |

</br>

## network_getNetMode
타겟 노드의 네트웍 이름을 조회

```bash
curl -X POST --data \
'{
 "jsonrpc": "1.0",
 "method": "network_getNetMode",
 "id": 1,
 "params": []
 }' 127.0.0.1:7979
```

```json
{
    "jsonrpc": "1.0",
    "id": 1,
    "result": "Mainnet"
}
```

### Parameters
없음

### Returns
| Index | Key | Type | Description |
| -------- | -------- | -------- | -------- |
| 1 | result | string | 네트웍 이름 (예:Mainnet") |

