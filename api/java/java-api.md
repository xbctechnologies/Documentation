# Tx

## sendTransaction
트랜잭션 전송

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| TxRequest | txRequest | 트랜잭션 요청 구조체 |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, TxSendResponse> | txHash |
|  | txSize |
|  | dataAccountAddr |

</br>

## getTransaction
전송한 트랜잭션 조회

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | nullable |
| String | targetChainId | 체인 아이디 |
| String | txHash | 조회할 tx hash |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, TxResponse> |  |

</br>

# Block

## getBlock
블록해시를 이용하여 블록정보 조회

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | nullable |
| String | targetChainId | 체인 아이디 |
| String | blockHash | 조회할 tx hash |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, BlockResponse> | chainID |
|  | blockNo |
|  | blockHash |
|  | blockSize |
|  | transactionRoot |
|  | transactions |
|  | numTxs |
|  | bloctotal_txskNo |
|  | time |

</br>

## getBlockByNumber
블록No를 이용하여 블록정보 조회

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | nullable |
| String | targetChainId | 체인 아이디 |
| String | blockNo | 블록 넘버 |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, BlockResponse> | chainID |
|  | blockNo |
|  | blockHash |
|  | blockSize |
|  | transactionRoot |
|  | transactions |
|  | numTxs |
|  | bloctotal_txskNo |
|  | time |

</br>

## getBlockTxCount
블록해시 또는 블록No를 이용하여 블록에 포함된 Tx수 조회

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | nullable |
| String | targetChainId | 체인 아이디 |
| Object | block | |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, BlockTxCntResponse> |  |

</br>

## getBlockLatestBlock
지정된 체인에서의 가장 최신 블록정보 조회

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | nullable |
| String | targetChainId | 체인 아이디 |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, BlockResponse> | chainID |
|  | blockNo |
|  | blockHash |
|  | blockSize |
|  | transactionRoot |
|  | transactions |
|  | numTxs |
|  | bloctotal_txskNo |
|  | time |

# Account

## createAccount
로컬 계좌 생성

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| String | pass phrase | 패스워드 |
| String | keystore directory | 키파일 저장 디렉토리 |

### Returns
| Type | Result |
| -------- | -------- |
| String | account address |

</br>

## getBalance
계정의 잔고정보 조회

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | 트랜잭션 요청 id. 기본값 null |
| String | targetChainId | 체인 아이디 |
| String | account address | 계좌 주소 |
| CurrencyType | currenct type | 화폐 단위 |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, AccountBalanceResponse> |  |

</br>

## getAccount
계정의 txNo, 잔고, 본딩양, 본딩리스트, 언본딩 리스트 등의 데이터 조회

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | 트랜잭션 요청 id. 기본값 null |
| String | targetChainId | 체인 아이디 |
| String | account address | 계좌 주소 |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, AccountResponse> | txNo |
|  | balance |
|  | bondedBalance |
|  | bondingList |
|  | unBondingList |

</br>

## getLastTxNonce
계좌에 대한 마지막 nonce 값을 구해 온다.

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | 트랜잭션 요청 id. 기본값 null |
| String | targetChainId | 체인 아이디 |
| String | account address | 계좌 주소 |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, LongResponse> | last nonce |

</br>

# Validator

## getValidatorList
등록된 전체 Validator 리스트 조회

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | 트랜잭션 요청 id. 기본값 null |
| String | targetChainId | 체인 아이디 |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, ValidatorListResponse> | validatorAccountAddr |
|  | totalBondingBalance |
|  | totalBondingBalanceOfValidator |
|  | rewardAmountFee |
|  | rewardBlocks |
|  | delegatorMap |
|  | companyName |
|  | companyDesc |
|  | companyUrl |
|  | companyLogoUrl |
|  | companyLat |
|  | companyLon |
|  | isFreezing |
|  | freezingReason |
|  | freezingBlockNo |
|  | disconnectCnt |

</br>

# Network

## getPeerCnt
XNode에 연결된 Peer수 조회

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | 트랜잭션 요청 id. 기본값 null |
| String | targetChainId | 체인 아이디 |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, LongResponse> | peer count |

</br>

## getPeers
XNode에 연결된 Peer 리스트 정보 조회

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | 트랜잭션 요청 id. 기본값 null |
| String | targetChainId | 체인 아이디 |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, NetworkPeersResponse> | id |
|  | listenAddr |
|  | network |

</br>

## addPeer
요청한 노드에게 파라미터로 준 노드와의 피어연결을 하고 address book목록에도 추가

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | 트랜잭션 요청 id. 기본값 null |
| String | targetChainId | 체인 아이디 |
| String[] | peers | XNode network에 포함시킬 피어들 (Peer_ID@ip:port) |
| boolean | persistence | persistent 모드 여부 |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, BoolResponse> | add 여부 |

</br>

## removePeer
요청한 노드에게 파라미터로 준 노드와의 네트웍 연결을 끊고 address book목록에도 제외

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | 트랜잭션 요청 id. 기본값 null |
| String | targetChainId | 체인 아이디 |
| String[] | peers | XNode network에 포함시킬 피어들 (Peer_ID@ip:port) |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, BoolResponse> | remove 여부 |

</br>

## getNetMode
XNode의 네트웍 이름을 조회

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | 트랜잭션 요청 id. 기본값 null |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, Response> |  |

</br>

# Node

## getXNodeStatus
XNode의 상태 조회

### Parameters
| Type | Parameter | Description |
| -------- | -------- | -------- |
| Long | Request ID | 트랜잭션 요청 id. 기본값 null |
| String | targetChainId | 체인 아이디 |

### Returns
| Type | Result |
| -------- | -------- |
| Request<?, XChainStatusResponse> | xchainID |
|  | nodeInfo |
|  | syncInfo |
|  | validatorInfo |


