## account_getBalance
해당 체인에서 주소의 계좌 잔액을 조회할 수 있다.

```bash
curl -X POST --data \
'{
 "jsonrpc": "1.0", "method": "account_getList", "id": 1
}' 127.0.0.1:7979
```

```json
{
    "jsonrpc": "1.0",
    "id": 1,
    "result": [
        "0x1ee825cd7431ef523fe6fc3c0a2fbc564dd9c672",
        "0x3eaf1ba2d57e117e809f6fd01acaaf33878c6df1",
        "0x947e885b41b32eb82e599ab81b7565677bdab020",
        "0x40b3122d169b1e7e84d056967d6ca87185f96ea8",
        "0xfa99acc71a461831ed7cc641e5b34a9872480f4e",
        "0x50e08b31bb7274d3a089cfdf479a19bf219dfe79",
        "0xdffdd2f1aeaf72edca1c091b5ed0c8812c97e327",
        "0x8c81e2c5b0c643b803085fd93bf055e927e7beab",
        "0x7ffae6edaa9553c9a3e6ac3e7e82dc1bc94c7452",
        "0x91579fafd75ba8647120cfa1f8333dcd6a5343af",
        "0x9717adc4fc65ccc272464dc67147edd04f3a6396"
    ]
}
```

### Parameters
없음

### Returns
| Type | Description |
| -------- | -------- |
| []byte | 해당 노드에 저장된 keystore 파일 기준, 모든 Account 주소 값을 반환 |

</br>

## account_getAccount
계좌주소를 이용하여 계좌에 대한 정보를 구해 온다.

```bash
curl -X POST -d \
'{
 "jsonrpc": "1.0", "method": "account_getAccount", "id": 1,
 "params": [ "1A", "0x1ee825cd7431ef523fe6fc3c0a2fbc564dd9c672"]
 }' 127.0.0.1:7979
```

```json
{
    "jsonrpc": "1.0",
    "id": 1,
    "result": {
        "txNo": 0,
        "balance": "98000000000000000000000000",
        "bondedBalance": "2000000000000000000000000",
        "bondingList": [
            {
                "validatorAccountAddr": "0x1ee825cd7431ef523fe6fc3c0a2fbc564dd9c672",
                "bondingBalance": "2000000000000000000000000"
            }
        ],
        "unBondingList": null
    }
}
```

### Parameters
| Index | Parameter | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | address | string | mandatory | 조회하려는 계좌 주소 |
| 2 | targetChainId | string | mandatory | 조회하려는 계좌의 체인 id |

### Returns
| Index | Key | Value | Type | SubKey |  SubType | Description |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| 1 | txNo | 트랜잭션 인덱스값 | long | | | |
| 2 | balance | account 총 밸런스 | big integer | | | |
| 3 | bondedBalance |  | big integer | | | |
| 4 | bondingList | bonding 리스트 | list | validatorAccountAddr | String | 밸리데이터 계좌 주소 |
|  |  |  |  | bondingBalance | BigInteger | 밸리데이터의 본딩 밸런스 |
| 5 | unBondingList | account 총 밸런스 | list | validatorAccountAddr | String | 밸리데이터 계좌 주소 |
| |  |  |  | unBondingBalance | big integer | 밸리데이터의 언본딩 밸런스 |
| |  |  |  | rewardBalance | big integer | 요청한 account 예측되는 보상 밸런스 |
| |  |  |  | breakBlockNo | long |  |

### Error Message
| Error Code | Message | Description |
| -------- | -------- | -------- |
| 200 | Not found account | 해당 account 주소를 찾을 수 없을 때 |

</br>

## account_getLastTxNonce
계좌에 대한 마지막 nonce 값을 구해 온다.

```bash
curl -X POST -d \
'{
 "jsonrpc": "1.0", "method": "account_getLastTxNonce", "id": 1,
 "params": [ "1A", "0x1ee825cd7431ef523fe6fc3c0a2fbc564dd9c672"]
 }' 127.0.0.1:7979
```

```json
{
    "jsonrpc": "1.0",
    "id": 1,
    "result": 0
}
```

### Parameters
| Index | Parameter | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | address | string | mandatory | 조회하려는 계좌 주소 |
| 2 | targetChainId | string | mandatory | 조회하려는 계좌의 체인 id |

### Returns
| Type | Description |
| -------- | -------- |
| long | 블럭의 마지막 nonce값 |
