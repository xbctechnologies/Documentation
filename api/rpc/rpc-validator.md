## validator_getValidatorList
지정된 체인 id의 밸리데이터 노드들의 대한 정보를 리스트 형태로 조회

```bash
curl -X POST --data \
'{
 "jsonrpc": "1.0",
 "method": "validator_getValidatorList",
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
            "validatorAccountAddr": "0x40b3122d169b1e7e84d056967d6ca87185f96ea8",
            "totalBondingBalance": "2000000000000000000000000",
            "totalBondingBalanceOfValidator": "2000000000000000000000000",
            "rewardAmountFee": "0",
            "rewardBlocks": null,
            "delegatorMap": {
                "0x40b3122d169b1e7e84d056967d6ca87185f96ea8": {
                    "totalBondingBalance": "2000000000000000000000000",
                    "bondingList": [
                        {
                            "blockNo": 1,
                            "bondingBalance": "2000000000000000000000000"
                        }
                    ]
                }
            },
            "companyName": "Grabity",
            "companyDesc": "Grabity is a public blockchain that makes it easy to deploy a large number of blockchain businesses in mobile environments.",
            "companyUrl": "http://grabity.io",
            "companyLogoUrl": "https://grabity.io/resources/web/img/core-img/logo.png",
            "companyLat": "37.520958",
            "companyLon": "127.029161",
            "isFreezing": false,
            "freezingReason": 0,
            "freezingBlockNo": 0,
            "disconnectCnt": 0
        },
        {
            "validatorAccountAddr": "0x3eaf1ba2d57e117e809f6fd01acaaf33878c6df1",
            "totalBondingBalance": "2000000000000000000000000",
            "totalBondingBalanceOfValidator": "2000000000000000000000000",
            "rewardAmountFee": "0",
            "rewardBlocks": null,
            "delegatorMap": {
                "0x3eaf1ba2d57e117e809f6fd01acaaf33878c6df1": {
                    "totalBondingBalance": "2000000000000000000000000",
                    "bondingList": [
                        {
                            "blockNo": 1,
                            "bondingBalance": "2000000000000000000000000"
                        }
                    ]
                }
            },
            "companyName": "CertOn",
            "companyDesc": "CertOn is specialized in the field of security",
            "companyUrl": "http://www.certon.co.kr",
            "companyLogoUrl": "http://www.xblocksys.com/img/xbs_08.png",
            "companyLat": "37.520958",
            "companyLon": "127.029161",
            "isFreezing": false,
            "freezingReason": 0,
            "freezingBlockNo": 0,
            "disconnectCnt": 0
        },
        {
            "validatorAccountAddr": "0x1ee825cd7431ef523fe6fc3c0a2fbc564dd9c672",
            "totalBondingBalance": "2000000000000000000000000",
            "totalBondingBalanceOfValidator": "2000000000000000000000000",
            "rewardAmountFee": "0",
            "rewardBlocks": null,
            "delegatorMap": {
                "0x1ee825cd7431ef523fe6fc3c0a2fbc564dd9c672": {
                    "totalBondingBalance": "2000000000000000000000000",
                    "bondingList": [
                        {
                            "blockNo": 1,
                            "bondingBalance": "2000000000000000000000000"
                        }
                    ]
                }
            },
            "companyName": "Xblocksystems",
            "companyDesc": "Xblocksystems is specialized in the field of block chain and security certification.",
            "companyUrl": "http://www.xblocksys.com",
            "companyLogoUrl": "http://www.xblocksys.com/img/xbs_08.png",
            "companyLat": "37.520958",
            "companyLon": "127.029161",
            "isFreezing": false,
            "freezingReason": 0,
            "freezingBlockNo": 0,
            "disconnectCnt": 0
        },
        {
            "validatorAccountAddr": "0x947e885b41b32eb82e599ab81b7565677bdab020",
            "totalBondingBalance": "2000000000000000000000000",
            "totalBondingBalanceOfValidator": "2000000000000000000000000",
            "rewardAmountFee": "0",
            "rewardBlocks": null,
            "delegatorMap": {
                "0x947e885b41b32eb82e599ab81b7565677bdab020": {
                    "totalBondingBalance": "2000000000000000000000000",
                    "bondingList": [
                        {
                            "blockNo": 1,
                            "bondingBalance": "2000000000000000000000000"
                        }
                    ]
                }
            },
            "companyName": "Xblock",
            "companyDesc": "CertOn is specialized in the field of block chain and security certification.",
            "companyUrl": "http://www.certon.co.kr",
            "companyLogoUrl": "http://www.xblocksys.com/img/xbs_08.png",
            "companyLat": "37.520958",
            "companyLon": "127.029161",
            "isFreezing": false,
            "freezingReason": 0,
            "freezingBlockNo": 0,
            "disconnectCnt": 3
        }
    ]
}
```

### Parameters
| Index | Parameter | Type | Option | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | targetChainId | string | mandatory | 조회하려는 계좌의 체인 id. (ex: A-assetChain, D-dataChain) |

### Returns
| Index | Parameter | Type | Description |
| -------- | -------- | -------- | -------- | -------- |
| 1 | validatorAccountAddr | string | 밸리데이터 계정주소 |
| 2 | totalBondingBalance | string | Validator에게 위임된 총 지분양 |
| 3 | totalBondingBalanceOfValidator | string | 밸리데이터로 참여하기 위해 걸었던 본딩액 |
| 4 | delegatorMap | | 계정별 Validator에게 위임한 지분양 및 보상양 |
| 5 | companyName | string | 밸리데이터 노드를 운영하는 법인명 |
| 6 | companyDesc | string | 법인 소개 |
| 7 | companyUrl | string | 회사 홈페이지 url |
| 8 | companyLogoUrl | string | 회사 CI의 이미지 링크 |
| 9 | companyLat | string | 회사 위치 GPS latitude |
| 10 | companyLon | string | 회사 위치 GPS longitude |
| 11 | isFreezing | bool | 합의군 (validator set)에 빠져있는지 여부 |
| 12 | freezingReason | long | 합의군에 빠지게 된 이유 |
| 13 | freezingBlockNo | long | |
| 14 | disconnectCnt | long | 합의 프로토콜 과정 중에서 네트원 문제 혹은 이미 과정중 동의비율이 2/3를 초과하여 의미 참여에 의미가 없어 빠진 경우의 회수 |
