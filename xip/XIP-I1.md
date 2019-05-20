## Simple Summary
X.Blockchain 을 이용하여 서비스 또는 어플리케이션을 개발하기 위해서 X.Node 가 HTTP 또는 IPC 형태로 제공하는 API 를 활용할 수 있다.

## Specification

#### Transaction APIs
**sendTransaction**  
트랜잭션을 생성하고 네트워크에 전파한다.
```shell
curl http://{rpcserver}:7979/sendTransaction -d "{parameter1:value1, ...}"
```
- `parameter1` - Not defined.

**getTransaction**  
트랜잭션을 조회한다.  

**getTransactionReceipt**  
트랜잭션 처리결과를 조회한다.  

**signTransaction**  
트랜잭션에 서명 한다.  

**checkOriginal**  
원본 파일(데이터) 의 무결성을 확인한다.

**delegating**  
자신의 지분을 특정 validator 에게 위임한다.  

**undelegating**  
지분의 위임을 회수 한다.  

**bonding**  
Validator 가 되기 위해 자신의 지분 동결 시킨다.  
동결된 지분과 위임 받은 지분의 합으로 Validators 가 결정된다.  

**unbonding**  
동결한 지분을 다시 회수 한다.  
Unbonding 이 처리된 이후 일정 기간 후에 해당 자산의 동결은 완전히 해제 된다.  

#### Block APIs
**getBlock**  
블록 해쉬 값으로 블록 데이터를 조회 한다.  

**getBlockByNumber** ~~getBlockNumber~~  
블록 번호로 값으로 블록 데이터를 조회 한다.  

**getBlockTxCount** ~~getBlockTransactionCnt~~  
블록에 포함된 트랜잭션의 개수를 조회 한다.

#### Account APIs
**new** ~~newAccount~~  
**import**  
**export**  
**lock**  
**unlock**  
**getList** ~~getAccountList~~  
**getBalance**  
**getBondingAmount** ~~getBonding~~

#### Validator APIs
**isValidator**  
현재 노드 Validator Set 에 포함되었는지 여부를 리턴한다.  

**getValidatorsOf**  
특정 계정이 지분을 위임한 Validator 의 목록을 리턴한다.  

**getValidatorSet**  
현재 Validator Set 을 리턴한다.  (blockno 기준 validator set)

#### Network APIs
**getPeerCnt**  
**getPeers**  
**addPeer**  

#### Node APIs
**sync**  
**isSync**  
**getChainTreeInfo**  
**getVersion**  






| Category | API | Protocol | Description |
|---|---|---|---|
| Transaction | sendTransaction | HTTP, IPC | 트랜잭션을 생성하고 네트워크에 전파한다. |
|  | getTransaction | HTTP, IPC |  |
|  | signTransaction | HTTP, IPC |  |
|  | checkOriginal | HTTP, IPC |  |
|  | delegating | HTTP, IPC |  |
|  | undelegating | HTTP, IPC |  |
|  | bonding | HTTP, IPC |  |
|  | unbonding | HTTP, IPC |  |
| Block | getBlock | HTTP, IPC |  |
|  | ~~getBlockNumber~~ getBlockByNumber | HTTP, IPC |  |
|  | ~~getBlockTransactionCnt~~ getBlockTxCount | HTTP, IPC |  |
|  | sendTransaction | HTTP, IPC |  |
| Account | ~~newAccount~~ new | HTTP, IPC |  |
|  | import | HTTP, IPC |  |
|  | export | HTTP, IPC |  |
|  | lock | HTTP, IPC |  |
|  | unlock | HTTP, IPC |  |
|  | ~~getAccountList~~ getList | HTTP, IPC |  |
|  | getBalance | HTTP, IPC |  |
|  | ~~getBonding~~ getBondingAmount | HTTP, IPC |  |
| Validator | isValidator | HTTP, IPC | 특정 계정의 Validator Set 포함 여부를 리턴 한다. |
|  | getValidatorsOf | HTTP, IPC | 특정 계정이 지분을 위임한 Validator 의 목록을 리턴한다. |
|  | getValidatorSet | HTTP, IPC | 현재 Validator Set 을 리턴한다. |
| Network | getPeerCnt | HTTP, IPC | 연결된 Peer 개수 |
|  | getPeers | HTTP, IPC | 연결된 Peer의 Json 정보 목록. |
|  | addPeer | HTTP, IPC | 새로운 Peer 로 연결을 생성한다. |
| Node | sync | HTTP, IPC | xnode 기동시 flag 값으로 정의된 X.Chain 의 블록 동기화 수행 |
|  | isSync | HTTP, IPC | 동기화 완료 여부. |
|  | ~~getHierarchicalChainInfo~~ getXChainInfo | HTTP, IPC | 현재 노드에서 검색 가능한 X.Chain 의 계층적인 구조 조회. |
|  | getVersion | HTTP, IPC | xnode 버전 정보 |
