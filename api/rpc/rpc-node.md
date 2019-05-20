## node_getXNodeStatus
해당 노드에서 노드정보, 동기화 상태, 밸리데이터 정보등을 조회

```bash
curl -X POST --data \
'{
 "jsonrpc": "1.0",
 "method": "node_getXNodeStatus",
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
        "xchainID": "1A",
        "node_info": {
            "id": "90c7978a1cde78986786ce9f6e62ae49486575ef",
            "listen_addr": "192.168.20.207:9004",
            "network": "IntraNet",
            "version": "0.22.5",
            "channels": [
                "State:1A",
                "Data:1A",
                "Vote:1A",
                "VoteSetBits:1A",
                "Blockchain:1A",
                "Mempool:1A",
                "Evidence:1A",
                "Pex:1A"
            ],
            "xNodeVersion": "1.0",
            "moniker": "iMac-di-kmpak.local",
            "other": [
                "amino_version=0.10.1",
                "p2p_version=0.5.0",
                "consensus_version=v1/0.2.2",
                "rpc_version=0.7.0/3",
                "tx_index=on"
            ]
        },
        "sync_info": {
            "latest_block_hash": "0x47038bb6ba324d11510e2559601887131478544634a94fcbca390f1b5dffd17e",
            "latest_app_hash": "0x0dfcb65c759c5f5aa705ea470efed78dd4e5c258",
            "latest_block_height": 8694,
            "latest_block_time": "2019-03-18T14:21:08.590096+09:00",
            "catching_up": false
        },
        "validator_info": {
            "address": "0x947e885b41b32eb82e599ab81b7565677bdab020",
            "pub_key": "0x025279023ab1f14660951d8dbd7244d69f9bdf76341c21c797d79142d5107ccada",
            "voting_power": 2000000
        }
    }
}
```

### Parameters
| Index | Parameter | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | targetChainId | string | mandatory | 조회하려는 계좌의 체인 id |

### Returns
| Index | Parameter | Sub Parameter | Type | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | targeChainId |  | string |  |
| 2 | node_info | id | string |  |
|  |  | listen_addr | string |  |
|  |  | network | string |  |
|  |  | version | string |  |
|  |  | channels | structure |  |
|  |  | xNodeVersion | string |  |
|  |  | moniker | string |  |
|  |  | other | structure |  |
| 3 | sync_info | latest_block_hash | string |  |
|  |  | latest_app_hash | string |  |
|  |  | latest_block_height | long |  |
|  |  | latest_block_time | string |  |
|  |  | catching_up | bool |  |
| 4 | validator_info | address | string |  |
|  |  | pub_key | string |  |
|  |  | voting_power | long |  |
