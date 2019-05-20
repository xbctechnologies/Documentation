## Simple Summary
X.Blockchain 에서 X.Chain 을 생성하고 이를 반영하는 절차를 기술한다.  
X.Chain 생성을 위한 X.Block 의 ``BlockExtension`` 은 생성될 Target X.Chain 의 초기 상태를 결정하기 위한 정보들이 포함되어 있다. X.Block 이 제안, 합의 되어 네트워크에 전파되어지면 각 노드는 X.Block 의 내용을 각자의 상태 정보에 반영한다. 이 때 Target X.Chain 이 Assetless 일 경우 Target X.Chain 의 생성 작업이 추가로 수행 되어야 한다.
또한 특정 X.Chain 을 동기화 할 경우, 해당 X.Chain 의 초기 상태는 최상위 X.Chain 의 그것과는 달리 X.Block 으로 부터 생성되기 위한 절차가 필요하다.

## Specification

### X.Block Build and Proposal
Proposer 는 X.Transaction 을 포함하는 X.Block 을 구성하여 validators 에게 제안한다. Proposer 가 X.Block 을 구성할 때 새로 생성되는 Target X.Chain 이 Assetful 일 경우, 현재 X.Chain 의 자산 보유 계정으로 일정량의 Target X.Chain 자산이 자동 airdrop 된다.  
이를 위해서 Proposer 는 현재 X.Chain 자산 보유 계정 목록과 수량 정보를 ``BlockExtension``.``GenesisInfo`` 에 추가하여 X.Block 을 구성한다. 자동 airdrop 되는 자산은 최초 Target X.Chain 전체 자산량 대비 일정 비율 만큼 추가 발행되는 방식으로 확보 하는데, X.Transaction 의  ``TxMakeXChainPayloadBody``.``airdropRate`` 의 값이 추가 발행 되어야 할 비율을 나타내며 각 계정별 airdrop 수량은 현재 X.Chain 자산 보유량에 비례하여 결정된다.  
Airdrop 정보 외 기타 Target X.Chain 의 최초 상태를 결정하기 위한 정보가 X.Block 의 ``BlockExtension``.``GenesisInfo`` 에 포함된다.

### X.Block Extension
Validator 들에 의하여 합의된 X.Block 을 수신한 노드는 그 내용을 블록 체인의 상태 정보에 반영한다. X.Block 를 수신한 X.Consensus 는 다음과 같이 XCI 를 호출 함으로서 X.State 의 상태 정보에 X.Block의 내용이 반영되도록 한다.

```
CurrXci.BeginBlock(...)
CurrXci.DeliveryTx(...)
CurrXci.EndBlock(...)
CurrXci.Commit(...)
```

Target X.Chain 이 Assetless 일 경우, 추가적으로 아래와 같은 절차가 수행된다.

```
if (Target X.Chain is Assetless) {

  TargetXci <- New XCI()
  TargetXci.Genesis(...)

  TargetXci.BeginBlock(...)
  // TargetXci.DeliveryTx(...)
  TargetXci.EndBlock(...)
  TargetXci.Commit(...)

}
```


### X.Block Synchronization
특정 X.Chain 에 대하여 동기화를 시작하는 경우, 우선 현재 노드는 [XIP-N1:Get SEED NODEs for a X.Chain](./XIP-N1.md) 에서 기술한 절차에 따라 대상이 되는 X.Chain 의 Seed 노드를 찾고 이 Seed 노드로 부터 현재 X.Chain 의 노드 정보를 획득 할 수 있다.  
이 노드들과 네트워크 연결을 생성한 다음 최초의 X.Block 수신을 시작으로 동기화를 시작한다.
현재 X.Chain 의 최초의 X.Block 을 수신한 노드는 X.Block 의 ``BlockExtenstion`` 정보로 부터 최초의 상태 정보를 생성한다.

```
call currXci.Genesis(...)

call currXci.BeginBlock(...)
// call currXci.DeliveryTx(...)
call currXci.EndBlock(...)
call currXci.Commit(...)

```

이로서 현재 X.Chain 의 genesis block 이 생성됨과 동시에 최초의 상태 정보가 구성된다. 이 상태 정보에는 airdrop 정보도 포함한다.
