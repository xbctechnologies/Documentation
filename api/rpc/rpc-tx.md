## tx_sendTransaction
모든 트랜잭션을 발생시킬 때 사용한다.  자산이 있는 주소 간의 트랜잭션 사용 시 해당 API 를 사용한다.

### curl 호출
```bash
curl -X POST --data \
'{
 "jsonrpc": "1.0",
 "method": "tx_sendTransaction",
 "id": 1,
 "params": [
 {
 "isSync": true,
 "targetChainId": "1A",
 "sender": "0x50e08b31bb7274d3a089cfdf479a19bf219dfe79",
 "receiver": "0x50e08b31bb7274d3a089cfdf479a19bf219dfe79",
 "fee": "1000000000000000000",
 "amount": "15000000000000000000",
 "time": "1552467451692082",
 "txnonce": 1,
 "v": 1,
 "r": "96217051370877044515258205273288366789290694090725685654072453715653677125316",
 "s": "20426264354636534714893706143253515442543260343366842220855468651708705795622",
 "payloadType": 1,
 "payloadBody": "eyJpbnB1dCI6IndvbmlUZXN0In0="
 }
 ]
}' 127.0.0.1:7979
```

### response 응답
```json
{
  "jsonrpc": "1.0",
  "id": 1,
  "result": {
    "txHash": "0xcdf0218072e23b8e557d288ca9ed69b6acfa83538c67608d52cf07fcfc12570a",
    "txSize": 10,
    "dataTxHash": "0x",
    "dataAccountAddr": "0x0000000000000000000000000000000000000000"
  }
}
 
{
  "jsonrpc": "1.0",
  "id": 1,
  "error": {
    "code": 356,
    "message": "Nonce Too Low"
  }
}
 
{
  "jsonrpc": "1.0",
  "id": 1,
  "error": {
    "code": 323,
    "message": "The signature is invalid or Account Address MisMatch (sender Address, recoverPub.Address() is misMatch (sender:50e08b31bb7274d3a089cfdf479a19bf219dfe79, recoverAddr:1a1ee9ce13f1deeea431085b3448b4389ee850c6))"
  }
}
```

### Parameters
| Index | Parameter | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | isSync | bool | optional | 트랜잭션이 블록에 포함되어 블록이 생성된 이후에 응답 받을지 여부 (default: false) |
| 2 | targetChainId | string | mandatory | 조회하려는 계좌의 체인 id |
| 3 | sender | byte[20] | mandatory | 트랜잭션 요청하는 계정주소 (보내는 이) |
| 4 | receiver | byte[20] | mandatory | 트랜잭션 수신 대상이 되는 계정주소 (받는 이) |
| 5 | fee | bigInteger | mandatory | 트랜잭션 수수료 |
| 6 | amount | bigInteger | mandatory | 전송하는 코인의 양 |
| 7 | time | bigInteger | mandatory | UTC microsecond 단위로 기입 (16자리, ex. 1552467451698082) |
| 8 | txNonce | uInt64 | mandatory | 보내는 이에 의해 보내진 트랜잭션의 개수를 의미하며, 기입 시 조회되는 개수 + 1 로 기입한다. |
| 9 | v | int | mandatory | ECDSA 가 복원한 공개 키 4개 중 어떤 공개를 사용 할지를 지정한 값 |
| 10 | s | bigInteger | mandatory | 서명 데이터 |
| 11 | r | bigInteger | mandatory | 서명 데이터 |
| 12 | payloadType | int | mandatory | 트랜잭션의 종류. 일반 송금 트랜잭션의 경우 CommonType "1" 지정 |
| 13 | payloadBody | string | mandatory | 트랜잭션의 종류에 따른 Body 값 |

### Returns
| Index | Key | Value | Type | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | txHash | 해당 트랜잭션 전송 요청 주소 | byte | 해당 트랜잭션의 해쉬 |
| 2 | txSize | 해당 트랜잭션의 전체 사이즈 | int | 해당 트랜잭션의 사이즈 |
| 3 | dataTxHash | 해당 사항 없음 (default: 0x) | byte | 요청한 트랜잭션 payLoadType:FileType(2) 인 경우, 보낸 이가 서명한 데이터트랜잭션 해쉬. 실제 노드에 반영된 트랜잭션의 주소는 txHash |
| 4 | dataAccountAddr | 해당 사항 없음 (default: 0x0000000000000000000000000000000000000000) | byte | 요청한 트랜잭션 payLoadType:FileType(2) 인 경우, 데이터어카운트주소 |

### Error Message
| Error Code | Message | Description |
| -------- | -------- | -------- |
| 0 | Tx payload body unmarshal err | 입력한 payloadBody 디코딩 실패 시 에러 반환 |
| 0 | PayloadBody encode err | 입력한 payloadBody 인코딩 실패 시 에러 반환 |
| 0 | sender Address, recoverPub.Address() is misMatch | 트랜잭션 서명 확인 시, 복호화한 주소값과 보낸이의 주소값이 일치하지 않는 경우 |
| 7 | The time value is not a microsecond value | 요청한 트랜잭션 타임 값이, microsecond 단위가 아닌 경우 |
| 8 | Not found targetChainId | 조회 요청한 체인 id 가 현재 노드에서 찾을 수 없는 경우 (–chainId 옵션 체크) |
| 301 | Sender can't nil | 필수 항목인, 보내는 이 주소가 없는 경우 |
| 302 | Receiver can't nil | 필수 항목인, 받는 이 주소가 없는 경우 |
| 345 | The time value is required when signing a transaction | 필수 항목인, 타임 값이 없는 경우 |
| 356 | Nonce Too Low | 입력한 txNonce 값이, 이미 사용된 경우 (account_getNonce API 조회 후, 결과 값이 +1 로 설정하여 전송) |

</br>

## tx_getTransaction
X.BlockChain 상에 존재하는 트랜잭션 상세 정보를 조회할 수 있다. 조회 시, 필수 입력항목은 ChainId, TransactionHash 값이다.

```bash
curl -X POST --data '{
 "jsonrpc": "1.0",
 "method": "tx_getTransaction",
 "id": 1,
 "params": [
 "1A.15D","0xdb0a317e105c45549742d317571c9e5a09b70663af98087cf9666857c66106a2"
  ]
 }' 127.0.0.1:7979
```

```json
{
  "jsonrpc": "1.0",
  "id": 1,
  "result": {
    "targetChainId": "1A.15D",
    "dataAccount": "0x487c1031b9fe6c3f3f6964c0e5e7fbcde3dd83c1",
    "blockHash": "0xd9bd38600465b943f3eea7041b6df0bce1020cb07bcbae0bf676144ad6ab8ff3",
    "blockNumber": "3",
    "txHash": "0xdb0a317e105c45549742d317571c9e5a09b70663af98087cf9666857c66106a2",
    "txIndex": 0,
    "txSize": 95,
    "dataTxHash": "0x7f83b7705773c041dd13adb7cceb19f622321ed3483e68b728af259463e1de8d",
    "dataReserved": "MHgyZTk5OTBjMGJiNDU0MGExZDkyMDVhM2YzOTI1NjE5MDVjMDY2NGY3MTNmYmI3NzQ1YWQ0YTNlYWIzODI4YmY3QDFU",
    "sender": "0x50e08b31bb7274d3a089cfdf479a19bf219dfe79",
    "receiver": "0x50e08b31bb7274d3a089cfdf479a19bf219dfe79",
    "txNonce": 1,
    "amount": "0",
    "fee": "1000000000000000000",
    "time": "1548154438865",
    "v": "1",
    "r": "15160360740265101029508133248946195847746361516352245349652409618589262845638",
    "s": "9287463525785499026386373551007245890465551741530932466180673197996161000134",
    "payloadType": 2,
    "payloadBody": "eyJvcCI6MSwiZGF0YUhhc2giOiJCdDVuOXNXcGZ5U0c3U0tDb2Vzd1ZuVHNaVU82UlJpY0FNSVorTEVKeUh3PSIsInJlc2VydmVkIjoibm8gdXNlIiwiaW5mbyI6Im5vVXNlIiwiYXV0aG9ycyI6WyIweDUwZTA4YjMxYmI3Mjc0ZDNhMDg5Y2ZkZjQ3OWExOWJmMjE5ZGZlNzkiLCIweDNlYWYxYmEyZDU3ZTExN2U4MDlmNmZkMDFhY2FhZjMzODc4YzZkZjEiXX0="
  }
}
```

### Parameters
| Index | Parameter | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | targetChainId | string | mandatory | 조회하려는 계좌의 체인 id. (ex: A-assetChain, D-dataChain) |
| 2 | txHash | byte | mandatory | 트랜잭션 해쉬 |

### Returns
| Index | Key | Value | Type | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | targetChainId | 해당 트랜잭션 XChainID | string | 해당 체인id (ex: A-assetChain, D-dataChain) |
| 2 | dataAccount | 데이터 트랜잭션 전송 시, 데이터어카운트 주소 (default: 0x0000000000000000000000000000000000000000) | byte | 해당 트랜잭션은 payloadType(2) 인 경우에만 해당 |
| 3 | blockHash | 해당 트랜잭션 실린 블록 해쉬 | byte |  |
| 4 | blockNumber | 해당 트랜잭션 실린 블록 Height | int64 |  |
| 5 | txHash | 해당 트랜잭션 전송된 주소 | byte | 트랜잭션이 블록에 포함되어 블록이 생성된 이후에 응답 받을지 여부 (default: false) |
| 6 | txIndex | 해당 트랜잭션 실린 블록 안에서 인덱스값 | uint32 |  |
| 7 | txSize | 해당 트랜잭션 사이즈 | int | |
| 8 | dataTxHash | 데이터 트랜잭션 전송 시, 보내는 이가 서명한 기준 해쉬값 (default: 0x) | byte | 해당 트랜잭션은 payloadType(2) 인 경우에만 해당 |
| 9 | dataReserved | 데이터 트랜잭션 전송 시, 부모체인 관계 기록 필드 | byte | 해당 트랜잭션은 payloadType(2) 인 경우에만 해당, commonTxHash@AssetChainId |
| 10 | sender | 해당 트랜잭션 요청한 계정주소 (보낸 이) | byte[20] | |
| 11 | receiver | 해당 트랜잭션 수신 대상된 계정 주소 (받는 이) | byte[20] | |
| 12 | txNonce | 해당 트랜잭션 보낸 이 기준 트랜잭션 처리된 개수 | uint64 | |
| 13 | amount | 해당 트랜잭션 전송한 코인 양 | bigInteger |  |
| 14 | fee | 해당 트랜잭션 수수료 | bigInteger |  |
| 15 | time | 해당 트랜잭션 기입된 시간 | bigInteger | UTC microsecond 단위로 기입 (16자리, ex. 1552467451698082) |
| 16 | v | ECDSA 가 복원한 공개 키 4개 중 어떤 공개를 사용 할지를 지정한 값 | int |  |
| 17 | r | 서명 데이터 | bigInteger | |
| 18 | s | 서명 데이터 | bigInteger | |
| 19 | payloadType | 해당 트랜잭션 종류 | int | (1) CommonType - 일반 송금 트랜잭션
| | | | | (2) FileType - 데이터 트랜잭션 |
| | | | | (3) BondingType - 자산 동결 트랜잭션 |
| | | | | (4) UnBondingType - 자산 동결해제 트랜잭션 |
| | | | | (5) DelegatingType - 자산 위임 트랜잭션 |
| | | | | (6) UnDelegatingType - 자산 위임해제 트랜잭션 |
| | | | | (7) GrProposalType - 거버넌스 제안 트랜잭션 |
| | | | | (8) GrVoteType - 거버넌스 투표 트랜잭션 |
| | | | | (9) RecoverValidatorType |
| | | | | (10) MakeXChainType - X.BlockChain 생성 트랜잭션 |
| 20 | payloadBody | 해당 트랜잭션 종류에 따른 Body 값 | string | |

### Error Message
| Error Code | Message | Description |
| -------- | -------- | -------- |
| 8 | Not found targetChainId | 조회 요청한 체인id 가 현재 노드에서 찾을 수 없는 경우 (–chainId 옵션 체크) |