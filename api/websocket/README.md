# Suscribe
이벤트 구독을 요청한다. 필터링 적용된 이벤트 응답을 수신한다.

## NewBlock subscribe request JSON data
```json
{
    "jsonrpc": "1.0",
    "method": "subscribe",
    "id": "0",
    "params": {
        "query": "xb.event = 'NewBlock'",
        "chainId": "1A"
    }
}
```

## NewBlock event JSON data
```json
{
  "jsonrpc": "2.0",
  "id": "0#event",
  "result": {
    "query": "xb.event = 'NewBlock'",
    "data": {
      "type": "xblockchain/internal/api/apires/block",
      "value": {
        "chainID": "1A",
        "blockNo": "14936",
        "blockHash": "0x75360411aa50f66713714122d229af8a499b0bc478d753ee0863fd7ae93f33b0",
        "blockSize": "745",
        "transactionRoot": "0x",
        "transactions": [],
        "numTxs": "0",
        "total_txs": "6",
        "time": 1553059618528876359
      }
    }
  }
}
```

## Tx Event(Block Height > 0) request JSON data
```json
{
    "jsonrpc": "1.0",
    "method": "subscribe",
    "id": "0",
    "params": {
        "query": "xb.event = 'Tx' AND bk.height > 0",
        "chainId": "1A"
    }
}
```

## Tx Event (Block Height > 0) JSON Data
```json
{
  "jsonrpc": "2.0",
  "id": "0#history",
  "result": {
    "query": "xb.event = 'tx' AND bk.height > 0",
    "data": {
      "type": "xblockchain/internal/api/apires/tx",
      "value": {
        "targetChainId": "1A",
        "dataAccount": "0x0000000000000000000000000000000000000000",
        "blockHash": "0xec09606fcdc9fb331430265f3090a01e71eb81fa6095c1f50e920ac1f14134e8",
        "blockNumber": "104",
        "txHash": "0xc4874b9a9946fcd34890809d6a163b09bb95fadb65fde560cd45516350f0bb43",
        "txIndex": 0,
        "txSize": "176",
        "dataTxHash": "0x",
        "dataReserved": null,
        "sender": "0x50e08b31bb7274d3a089cfdf479a19bf219dfe79",
        "receiver": "0xdffdd2f1aeaf72edca1c091b5ed0c8812c97e327",
        "txNonce": "6",
        "amount": "1000000000000000000",
        "fee": "1000000000000000000",
        "time": "1552556035020000",
        "v": "1",
        "r": "55051246125217708013328695833245903164321347708113149746882841882087061333976",
        "s": "15925876057360571628319720032804024157010540670468534907757277732466738491876",
        "payloadType": 1,
        "payloadBody": "eyJpbnB1dCI6InRlc3QxIn0="
      }
    }
  }
}
```
> [!NOTE]
> Block Height 조건이 있는 경우, 조건에 맞는 히스토리 데이터를 모두 전송한다.

## Parameters
| Index | Key | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | query | string | mandatory | 구독할 이벤트 종류와 필터링 조건. `ex) xb.event = '이벤트 종류' AND 필터링 조건1 AND 필터링 조건2 ...` |
| 2 | chainId | string | mandatory | 이벤트 구독 요청할 체인 ID |

### 이벤트 종류
| Key | Event | Description |
| -------- | -------- | -------- |
| xb.event | NewBlock | 새로운 블럭이 Commit 될 때마다 이벤트 발생 |
|  | Tx | 새로운 트랜잭션이 블럭에 쓰여질 때마다 이벤트 발생 |

### 필터링 조건 ( = , > 연산자 사용 )
| Key | Type | Description |
| -------- | -------- | -------- |
| bk.height | int | 모든 이벤트를 특정 block_height 값으로 필터링 |
| tx.hash | hex_string | Tx 이벤트를 특정 tx_hash로 필터링 |
| tx.type | int | Tx 이벤트를 특정 tx_type으로 필터링 |
| tx.sender | hex_string | Tx 이벤트를 특정 tx_sender로 필터링 |
| tx.receiver | hex_string | Tx 이벤트를 특정 tx_receiver로 필터링 |

## Event Returns

### xb.event = 'NewBlock'

| Index | Key | Type | Description |
| -------- | -------- | -------- | -------- |
| 1 | chainID | string | 블록이 포함되어 있는 체인 ID  (ex: A-assetChain, D-dataChain) |
| 2 | blockNo | int64 | 해당 블록 넘버 |
| 3 | blockHash | byte | 해당 블록 해쉬 |
| 4 | blockSize | int | 해당 블록 사이즈 |
| 5 | transactionRoot| byte | 해당 블록에 포함된 트랜잭션들의 해쉬 |
| 6 | transactions | []byte | 해당 블록에 포함된 트랜잭션 해쉬 배열 |
| 7 | numTxs | int64 | 해당 블록에 포함된 트랜잭션 개수 |
| 8 | total_txs | int64 | 해당 블록 기준, 총 트랜잭션 개수 |
| 9 | time | bigInteger | 해당 블록이 생성 될 때 시간 |

### xb.event = 'Tx'

| Index | Key | Type | Description |
| -------- | -------- | -------- | -------- |
| 1 | targetChainId | string | 해당 체인id (ex: A-assetChain, D-dataChain) |
| 2 | dataAccount | byte | 트랜잭션 payloadType(2)인 경우에(데이터 트랜잭션), 데이터 어카운트 주소 |
| 3 | blockHash | byte | 트랜잭션이 포함되어 있는 블록의 해쉬 |
| 4 | blockNumber | int64 | 트랜잭션이 포함되어 있는 블록의 넘버 |
| 5 | txHash| byte | 해당 트랜잭션 해쉬 |
| 6 | txIndex | uint32 | 해당 블록에 포함된 트랜잭션 해쉬 배열 |
| 7 | txSize | int | 해당 블록에 포함된 트랜잭션 개수 |
| 8 | dataTxHash | byte | 트랜잭션 payloadType(2) 인 경우에, 보내는 이가 서명한 기준 해쉬값 (default: 0x) |
| 9 | dataReserved | []byte | 트랜잭션 payloadType(2) 인 경우에, 부모체인 관계 기록 필드 `(commonTxHash@AssetChainId)` |
| 10 | sender | byte | 트랜잭션을 요청하는 계정 주소 (보내는 이) |
| 11 | receiver | byte | 트랜잭션을 수신하는 계정 주소 (받는 이) |
| 12 | txNonce | uint64 | 트랜잭션의 sender가 지금까지 보낸 총 트랜잭션 개수 |
| 13 | amount | bigInteger | 전송한 코인의 양 |
| 14 | fee | bigInteger | 트랜잭션 수수료 |
| 15 | time | bigInteger | 트랜잭션 발생 시간. UTC microsecond 단위로 기입 (16자리, ex. 1552467451698082) |
| 16 | v | string | publicKey를 복구하기 위한 ID 및 재실행 공격을 예방하기 위한 값 |
| 17 | r | bigInteger | ECDSA 전자서명 출력 값 |
| 18 | s | bigInteger | ECDSA 전자서명 출력 값 |
| 19 | payloadType | int32 | 트랜잭션의 종류. 일반 송금 트랜잭션의 경우 CommonType "1" 지정 |
| 20 | payloadBody | string | 트랜잭션의 종류에 따른 Body 값 |

## Event Message
| Code | Message | Data | Description |
| -------- | -------- | -------- | -------- |
| -32603 | Internal error | "ChainId is required" | 필수 파라미터인 체인 ID를 전송하지 않은 경우 |
| -32603 | Internal error | "failed to parse query: parse error near g (line 1 symbol 38 - line 1 symbol 39):\n\"\u003e\"\n" | 잘못된 쿼리를 보낸 경우 (파싱 오류) |
| -32603 | Internal error | "EventBus is not created yet [chainId : 2A]" | 아직 생성 되지 않은 체인 ID에 이벤트 구독 요청한 경우 |

</br>

# Unsubscribe
구독하던(요청했던) 특정 이벤트를 취소한다.

## NewBlock Unubscribe Request JSON Data
```json
{
    "jsonrpc": "1.0",
    "method": "unsubscribe",
    "id": "0",
    "params": {
        "query": "xb.event = 'NewBlock'",
        "chainId": "1A"
    }
}
```

## Parameters
| Index | Key | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | query | string | mandatory | 구독할 이벤트 종류와 필터링 조건. `(구독할 때 사용했던 쿼리와 일치해야 함)` |
| 2 | chainId | string | mandatory | 구독 취소 요청할 체인 ID (구독할 때 사용했던 체인ID) |

## Returns
없음

## Event Message
| Code | Message | Data | Description |
| -------- | -------- | -------- | -------- |
| -32603 | Internal error | "ChainId is required" | 필수 파라미터인 체인 ID를 전송하지 않은 경우 |
| -32603 | Internal error | "failed to parse query: parse error near g (line 1 symbol 38 - line 1 symbol 39):\n\"\u003e\"\n" | 잘못된 쿼리를 보낸 경우 (파싱 오류) |
| -32603 | Internal error | "EventBus is not created yet [chainId : 2A]" | 아직 생성 되지 않은 체인 ID에 이벤트 구독 요청한 경우 |

</br>

# unsubscribe_all
현재 구독 중인 모든 이벤트를 취소한다.

## Unsubscribe_All Request JSON Data
```json
{
    "jsonrpc": "1.0",
    "method": "unsubscribe_all",
    "id": "0",
    "params": {
        "chainId": "1A"
    }
}
```

## Parameters
| Index | Key | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | chainId | string | mandatory | 구독 전체 취소 요청할 체인 ID |

## Returns
없음

## Event Message
| Code | Message | Data | Description |
| -------- | -------- | -------- | -------- |
| -32603 | Internal error | "ChainId is required" | 필수 파라미터인 체인 ID를 전송하지 않은 경우 |
| -32603 | Internal error | "EventBus is not created yet [chainId : 2A]" | 아직 생성 되지 않은 체인 ID에 이벤트 구독 요청한 경우 |
