## Simple Summary
X.Blockchain 이 다루는 Transaction 의 종류와 데이터 구조를 정의한다.

## Specification
### Block Data Structures
#### ``Block``
|필드명|타입|설명|비고|
|---|---|---|---|
| Header | Header | Block 헤더. |  |
| Data | byte[][] | 현재 블록에 포함되는 트랜잭션 데이터의 배열. | 트랜잭션 데이터는 byte[] 이고, 그 것의 배열을 의미함. |
| Evidence | EvidenceData |  |  |
| LastCommit | Commit |  |  |
| Extension <sup>:small_blue_diamond:</sup> | BlockExtension |  |  |

#### ``Header``
|필드명|타입|설명|비고|
|---|---|---|---|
| ChainID | string | 현재 블록이 속한 X.Chain 에 대한 식별자. |  | ``XBlockInfo``.``NewChainID`` 로 이동. |
| Height | int64 | 현재 블록 높이. (= 블록 번호) |  |
| Bindex <sup>:small_blue_diamond:</sup>| int64 | Parent Chain 과 전체 Assetless Child Chain 에서 블록 순번. | |
| Time | Time | 블록이 생성된 로컬 시간. | Tenermint 의 Time 구조체 참조. |
| NumTxs | int64 | 블록에 포함된 Tx. 개수 |  |
| LastBlockID | BlockID |  |  |
| TotalTxs | int64 |  |  |
| LastCommitHash | byte[] |  |  |
| DataHash | byte[] |  |  |
| ValidatorsHash | byte[] |  |  |
| ConsensusHash | byte[] |  |  |
| AppHash | byte[] |  |  |
| AppDataHash | byte[] |  |  |
| LastResultsHash | byte[] |  |  |
| EvidenceHash | byte[] |  |  |

#### ``BlockExtension`` <sup>:small_blue_diamond:</sup>
|필드명|타입|설명|비고|
|---|---|---|---|
| BlockType | int32 |  |  |
| XBlockHash | byte[] | 이전 X.Block 해쉬 |  |
| ~~Reward~~ | ~~BigNumber~~ | ~~현재 블록에 대한 보상량.~~ |  |
| ~~Fee~~ | ~~BigNumber~~ | ~~현재 블록에 포함된 트랜잭션 수수료 총합.~~ |  |
| GenesisInfo | XGenesisBlock | 현재 블록이 X.Block 인 경우, X.Block 에 대한 정보를 기술. |  |


#### ``XGenesisBlock`` <sup>:small_blue_diamond:</sup>
|필드명|타입|설명|비고|
|---|---|---|---|
| ~~GenesisTime~~ | ~~string~~ |  |  |
| ChainID | string |  | 현재 블록으로 부터 새로 확장될 X.Chain 의 식별자. |
| AirdropAssetHolders | AssetHolder[] |  |  |
| ~~_~~ | ~~TxMakeXChainBody~~ |  |  |

<br />

### Validator Data Structures
#### ``Validator``
|필드명|타입|설명|비고|
|---|---|---|---|
| validatorAccountAddr | Address | Validator 계정 주소. | |
| validatorPubKey | PublicKey | Validator 의 Public Key. |  |
| bondedBalance | BigNumber | 위임 받은 총 지분량. |  |
| rewardBalance | BigNumber | 총 보상량. |  |
| rewardBlockMap | Map\<int64, Reward\> | 블록 단위 보상 내역. |  |
| isFreezing | bool |  |  |
| freezingReason | int32 |  |  |
| disconnectedCnt | int32 | 연속적으로 블록 합의에 빠진 블록수 |  |
| delegatorMap | Map\<Address, Delegator\> |  |  |

#### ``SimpleValidator``
|필드명|타입|설명|비고|
|---|---|---|---|
| validatorAccountAddr | Address | Validator 계정 주소. | |
| validatorPubKey | PublicKey | Validator 의 Public Key. |  |
| bondedBalance | BigNumber | 위임 받은 총 지분량. |  |
| startBlockNo | int64 | Validator Set 에 포함되어 블록합의에 참여를 시작한 블록 번호. |  |
| endBlockNo | int64 | Validator Set 에 포함되어 블록합의에 참여한 마지막 블록 번호. |  |
| isFreezing | bool |  |  |
| freezingReason | int32 |  |  |
| governanceVersion | string | 현재 Validator 가 적용중인 Governance Rule 버전. |  |
| xnodeVersion | string | 현재 Validator 가 구동 중인 xnode 버전. |  |

> **Note**:exclamation:
> Validator Set 결정시 전체 Validator 리스트를 로드해야 하는데 Validator를 사용하는 경우 deletator 등등의 데이터까지 모두 가져오게 됨.  
> 로드되는 데이터 사이즈를 줄이기 위하여 사용

<br/>

#### ``Delegator``
|필드명|타입|설명|비고|
|---|---|---|---|
| bondingBalance | BigNumber | Delegator 가 위임한 지분량. |  |
| rewardBalance | BigNumber | Delegator 가 보상량. |  |
| rewardBlockMap | Map\<int64, Reward\> | 블록 단위 보상 이력. |  |

#### ``Reward``
|필드명|타입|설명|비고|
|---|---|---|---|
| rewardBalance | BigNumber | 특정 블록에 대한 보상량. |  |
| bondedBalance | BigNumber | 특정 블록 시점에서 동결한 지분량. |  |

<br/>

### Account Data Structures
#### ``Account`` (``AssetAccount``)
|필드명|타입|설명|비고|
|---|---|---|---|
| address | Address | 데이터 계정 주소. | IAVL+ Tree 구조에서 Key. |
| txNo | int64 | 현재 계정이 생성한 트랜잭션 개수. ||
| bondingBalance | BigNumber | Validator 자격 획득의 위해 동결한 지분수량. | 사용 불가 |
| balance | BigNumber | 동결되지 않은 자산 총액. | 사용 가능. |
| bondingMap | Map\<Address, Bond\> | Validator 에게 위임한 지분 정보. ||
| unbondingList | Unbond[]] | Validator 에게 위임한 지분 위임 취소 정보. ||

#### ``Bond``
|필드명|타입|설명|비고|
|---|---|---|---|
| bondingBalance | BigNumber | 위임 또는 동결한 자산액. ||  

#### ``Unbond``
|필드명|타입|설명|비고|
|---|---|---|---|
| validatorAccountAddr | Address | 취소할 위임 지분을 위임 받은 Validator의 Address. ||
| unbondingBalance | BigNumber | 회수한 지분의 양. ||
| rewardBalance | BigNumber | unbonding 또는 undelegating 시 보상 받게될 자산. ||
| breakBlockNo | int64 | 회수한 지분이 사용 가능해 지는 시점의 BlockNo. ||

#### ``DataAccount``
|필드명|타입|설명|비고|
|---|---|---|---|
| address | Address | 데이터 계정 주소. | IAVL+ Tree 구조에서 Key. |
| opCnt | int64 | 현재 계정에 대하여 발생한 오퍼레이션 개수. | 생성 Tx. 을 포함하여 최초 값은 1.  |
| creator | byte[] | 데이터 생성 계정의 주소. | 업데이트 변경 불가. <br /> *Update 시 ``Transaction``.``sender`` 로 설정.(?)* |
| info | byte[] | 데이터에 대한 사용자 임의 정보. | 업데이트 변경 가능. |
| authors | Address[] | 데이터 갱신 권한을 갖는 계정의 주소 목록.<br/>이 목록에 ``0x0`` 하나의 주소만 설정된다면 이후 어느 계정으로도 데이터 변경이 불가능해 진다.<br/>이 목록에 ``0xffff...ffff`` 가 포함되어 있다면 모든 계정이 데이터 변경 권한을 갖는다.| 업데이트 변경 가능. |
| dataHash | byte[] | 데이터의 해쉬값 | 업데이트 변경 가능. |
| reserved | byte[] |  |  |

#### ``AssetHolder``
|필드명|타입|설명|비고|
|---|---|---|---|
| accountAddr | Address |  |  |
| amount | BigNumber |  |  |

<br />

### Transaction Data Structures
#### ``Transaction``
|필드명|타입|설명|비고|
|---|---|---|---|
|hash|byte[]|data 부분을 해시한 값.|  |
|size|uint32|트랜잭션 전체 크기.|  |
|data|TxData|  |  |

#### ``TxData``
|필드명|타입|설명|비고|
|---|---|---|---|
|sender|byte[20]|발신 계정 주소.||
|receiver|byte[20]|수신 계정 주소.||
|amount|BigNumber|전송 자산 수량.| xto 단위. |
|fee|BigNumber|트랜잭션 수수료.|트랜잭션 타입별 최소 수수료 이상.|
|targetChainId|string|트랜잭션이 기록될 X.Chain ID.||
|~~v~~|~~BigNumber~~|~~ECDSA 전자서명의 v 값.~~||
|r|BigNumber|ECDSA 전자서명의 r 값.||
|s|BigNumber|ECDSA 전자서명의 s 값.||
|time|BigNumber|트랜잭션 발생 시간.||
|payload|TxPayload|트랜잭션 타입별 데이터 구조.||

#### ``TxPayload``
|필드명|타입|설명|비고|
|---|---|---|---|
|type|TxPayloadType|트랜잭션 타입.||
|body|TxCommonBody|type 에 따른 가변 데이터 필드.|type = CommonType|
||TxFileBody||type = FileType|
||TxBondingBody||type = BondingType|
||TxUnBondingBody||type = UnBondingType|
||TxDelegatingBody||type = DelegatingType|
||TxUndelegatingBody||type = UnDelegatingType|
||TxGRProposalBody||type = ProposeGovernanceRuleType|
||TxGRVoteGRBody||type = VoteGovernanceRuleType|
||TxRecoverValidatorBody||type = RecoverValidatorType|
||TxMakeXChainBody||type = MakeXChainType|

#### ``TxPayloadType``
|이름|값|설명|비고|
|---|---|---|---|
|CommonType|int32(1)|||
|FileType|int32(2)|||
|BondingType|int32(3)|||
|UnBondingType|int32(4)|||
|DelegatingType|int32(5)|||
|UnDelegatingType|int32(6)|||
|ProposeGovernanceRuleType|int32(7)|||
|VoteGovernanceRuleType|int32(8)|||
|RecoverValidatorType|int32(9)|||
|MakeXChainType|int32(10)|||


#### ``TxCommonBody``
|필드명|타입|설명|비고|
|---|---|---|---|
|input|byte[]|임의의 데이터를 저장.||


#### ``TxFileBody``
|필드명|타입|설명|비고|
|---|---|---|---|
| op | int32 | 데이터 계정에 적용할 작업 종류.<br/>데이터 생성:0x1,<br/>데이터 갱신:0x2,<br/>데이터 검증:0x3,<br/>데이터 조회:0x4 ||
| ~~address~~ | ~~Address~~ | ~~``DataAccount`` 의 주소.~~ | |
| *data* | *Base64Encoded* | *데이터 원본. Base64 인코딩. RPC Interfaces 를 호출하는 시점에서만 사용되고, 이후 Mempool 저장 또는 네트워크 전파시에 이 필드는 dataHash 필드로 대체 된다.* | ``TxFileBody`` 를 포함 하고 있는 트랜잭션의 ``Transaction.hash`` 및 ``Transaction``의 전자서명 계산시 이 필드는 ``dataHash`` 값으로 대체 되어야 계산되어야 한다. |
| dataHash | byte[] | 데이터 원본 ``data`` 의 해쉬(SHA256) 값. ||
| reserved | byte[] |  |  |
| info | string | 데이터에 대한 메타 정보 ||
| authors | Address[] | 데이터 갱신 권한을 갖는 계정의 주소 목록. ||
> **Note**:exclamation:
> ``Transaction.receiver`` 필드가 Target ``DataAccount`` 의 주소를 의미한다.  

<br/>

#### ``TxBondingBody``
|필드명|타입|설명|비고|
|---|---|---|---|
| ~~amount~~ | ~~BigNumber~~ | ~~Validator 되기 위하여 Bonding 할 지분량.~~ | |
| CompanyName | string | Validator 의 이름. | |
| CompanyDesc | string | Validator 에 대한 소개. | |
| CompanyUrl | string | Validator의 Site URL. | |
| CompanyLogoUrl | string | 로고 이미지 URL. | |
| CompanyLat | string | Validator 위치의 위도 정보. | |
| CompanyLon | string | Validator 위치의 경도 정보. | |
| input | byte[] | 임의의 데이터를 저장.||
> **Note**:exclamation:
> ``Transaction.data.amount`` 필드가 Bonding 할 지분량 값을 의미한다.  

<br/>

#### ``TxUnBondingBody``
|필드명|타입|설명|비고|
|---|---|---|---|
|~~amount~~ | ~~BigNumber~~ | ~~회수할 지분량.~~ |
|input|byte[]|임의의 데이터를 저장.||
> **Note**:exclamation:
> ``Transaction.data.amount`` 필드가 회수할 지분량 값을 의미한다.  

<br/>

#### ``TxDelegatingBody``
|필드명|타입|설명|비고|
|---|---|---|---|
|~~validatorAccountAddr~~ | ~~Address~~ | ~~위임을 받을 Validator 계정의 주소.~~ |
|~~amount~~ | ~~BigNumber~~ | ~~위임할 지분량.~~ | |
|input|byte[]|임의의 데이터를 저장.||
> **Note**:exclamation:
> ``Transaction.data.receiver`` 필드가 위임을 받을 Validator 계정의 주소 값을 의미한다.  
> ``Transaction.data.amount`` 필드가 Bonding 할 지분량 값을 의미한다.  

<br/>

#### ``TxUndelegatingBody``
|필드명|타입|설명|비고|
|---|---|---|---|
|~~validatorAccountAddr~~ | ~~Address~~ | ~~위임을 취소 Validator 계정의 주소.~~ |
|~~amount~~ | ~~BigNumber~~ | ~~위임 취소 지분량.~~ | |
|input|byte[]|임의의 데이터를 저장.||
> **Note**:exclamation:
> ``Transaction.data.receiver`` 필드가 위임을 취소할 Validator 계정의 주소 값을 의미한다.  
> ``Transaction.data.amount`` 필드가 위임 취소할 지분량 값을 의미한다.  

<br/>

#### ``TxGRProposalBody``
|필드명|타입|설명|비고|
|---|---|---|---|
| RewardXtoPerCoin | BigNumber | 블록합의에 참여한 Validator 및 그의 Delegator 들에게 주어지는 Coin 당 보상량(xto 단위)으로 . | ex. 이 값이 1Gxto 라면 1 ATX 당 1Gxto 를 지급함을 의미.   |
| ~~ValidatorRewardRate~~ | ~~int64~~ | ~~RewardAmount 중 Validator 의 지분률.~~ | ~~30%~~ |
| ~~DelegatorRewardRate~~ | ~~int64~~ | ~~RewardAmount 중 Delegator 의 지분률. 여기에는 Validator 자신도 포함됨.~~ | ~~70%~~ |
| MinCommonTxFee | BigNumber | 트랜잭션 최소 수수료. | |
| MinBondingTxFee | BigNumber | TxBondingBody 를 포함하는 트랜잭션 최소 수수료. |  |
| MinGRProposalTxFee | BigNumber | TxGRProposalBody 를 포함하는 트랜잭션 최소 수수료. |  |
| MinGRVoteTxFee | BigNumber | TxGRVoteBody 를 포함하는 트랜잭션 최소 수수료. |  |
| MinXTxFee | BigNumber | TxMakeXChainBody 를 포함하는 트랜잭션 최소 수수료. |  |
| MinBlockNumsToGRProposal | int64 | Governance Rule 변경 제안을 하기 위해 Validator가 연속적으로 합의에 참여해야 하는 최근 블록의 수. 이 값 보다 많은 블록의 합의에 연속적으로 참여한 Validator 만이  Governance Rule 변경을 제안할 수 있다. |  |
| MaxBlockNumsForVoting | int64 | Governance Rule 변경 제안에 대한 최대 투표기간. |  |
| MinBlockNumsUtilReflection | int64 | 가결된 제안이 적용되기 위해 대기 해야 할 최소 블록 수. 가결된 제안의 적용은 적어도 이 값에 명시된 블록수 이후에 적용된다.| 201,600 (blocks) ~= 7일 |
| MaxBlockNumsUtilReflection | int64 | 가결된 제안이 적용되기 위해 대기 해야 할 최대 블록 수. 가결된 제안의 적용은 적어도 이 값에 명시된 블록수 이전에 적용된다. | 864,000 (blocks) - 30일 |
| BlockNumsFreezingValidator | int64 | Validator가 이 값에 지정된 블록수 동안 블록 합의에 연속적으로 참여하지 않을 경우, Validator Set에서 제외| 28,800 (blocks) - 1일 |
| BlockNumsUtilUnbonded | int64 | Unbonding 또는 Undelegating후 자산을 일정기간 동결하기 위한 블록수 | 201,600 (blocks) - 7일 |
| MaxDelegatableValidatorNums | int64 | 하나의 계정이 자신의 지분을 위임할 수 있는 최대 Validator 수 | 50 |
| ValidatorNums | int64 | Validator 의 수. | 50 |
| FirstCompatibleVersion | string | XBlockchain 시스템에서 적용해야할 Xnode 버젼.<br/>VersionMajor : 해당 버젼증가시 안건발의 해야 함.<br/>VersionMinor : 해당 버젼증가시 안건발의 해야 함.<br/>VersionPatch : 해당 버젼증가시 안건발의 불필요. | ex: 1.0.0-stable |
| CurrentReflection | CurrentReflection | 현재 제안된 제안 변경 건에 대한 속성. |  |

#### ``CurrentReflection``
|필드명|타입|설명|비고|
|---|---|---|---|
|BlockNumsForVoting | int64 | 현재 Governance Rule 변경 제안에 대한 투표 기간 지정. ``MaxBlockNumsForVoting`` 의 값보다 작거나 같아야 한다. | 1000e18 xto |
|BlockNumsUtilReflection | int64 | 현재 Governance Rule 변경 제안이 가결될 경우, 가결된 내용이 반영되어야 할 시점을 블록 수로 지정. 이 값은 ``MinBlockNumsUtilReflection`` 보다 크거나 같고, ``MaxBlockNumsUtilReflection`` 보다 작거나 같아야 한다. | 30% |

#### ``TxGRVoteBody``
|필드명|타입|설명|비고|
|---|---|---|---|
|YesOrNo | bool | 현재 진행중인 제안에 대한 찬성/반대. |  |

#### ``TxRecoverValidatorBody``
Not Yet.


#### ``TxMakeXChainBody``
|필드명|타입|설명|비고|
|---|---|---|---|
|depth | uint8 | 생성되는 X.Chain 이 가질 수 있는 ChildChain 의 깊이. | |
|hasAsset | bool | 생성되는 X.Chain 의 독자적인 자산 생성 여부. | Parent XChain의 enableSubAsset = false 이면 무조건 FALSE<br />Parent XChain의 enableSubAsset = true 이면 true/false |
|enableSubAsset | bool | 생성되는 X.Chain 의 ChildChain 의 독자적인 자산 생성 가능 여부. | hasAsset == false || ParentChain's enableSubAsset == false 인 경우 자동으로 false 값을 가짐. |
|nonExchangeChain | bool | 타 X.Chain 의 자산과 교환 가능 여부. | hasAsset = false이면 무조건 true |
|customDesc | string | 생성자에 의하여 기술되는 X.Chain 의 설명. | |
|Validators | Validator[] | 초기 validator 리스트. | |
|airdropRate | uint8 | Parent XChain 의 자산 보유자 에게 발행되는 자산의 비율. | 현재 발행된 자산 대비 요율. 이 자산량 만큼 전체 발행 자산은 증가. |
| ~~airdropAssetHolders~~ | ~~Map\<Address, *Holder\>~~ | | |
|assetHolders | AssetHolder[] | 최초 자산 보유자 주소 및 보유량 | |
|seedNodes | string | 생성되는 X.Chain 의 Seed Node 목록. tcp://{ip address}:{port} 형식의 문자열. | |
