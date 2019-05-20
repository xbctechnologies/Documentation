## Simple Summary
X.Blockchain 은 전자 문서를 포함한 다양한 전자적 형태로 이루어진 데이터의 무결성 검증 목적의 기능을 지원한다. X.Blockchain 에 등록되는 데이터는 등록시 고유의 주소(**Data Account**)를 부여 받는다. 이 주소는 일반적인 암호 화폐 자산 계정(**Asset Account**)과 동일한 주소 형태에 X.Chain 의 ID 값이 결합된 형태 (?) 로 구성된다. **Asset Account**의 잔고(Blanace) 정보 대신 **Data Account** 은 해당 데이터의 해쉬값을 갖는다.  
각 **Data Account**의 내용을 반영하는 해쉬 값은 **Asset Account** 과 마찬가지로, 그러나 독립적인 또 다른 IAVL+ Tree 구조를 구성하고 이 트리의 최상위 해쉬 값이 Block 에 포함됨으로서 결과적으로 블록체인에 의한 무결성을 보장 획득한다.
이 후 전자 문서는 주소로 식별되고 해당 문서의 최근 해쉬 값으로 해당 전자 문서의 무결성 여부를 체크할 수 있다.

## Abstract
기존의 블록체인에서 암호화폐의 거래 이외의 전자적 데이터의 무결성 검증을 위해서 블록체인 소프트웨어가 제공하는 '사용자 필드' 에 문서의 해쉬 값을 저장 하였다. 그러나 이런 방법은 최적화 된 방법이 아닐 뿐더러 특정 문서의 히스토리 관리가 사실상 불가능하였다.  
X.Blockchain 은 전자 문서의 관리를 기존 암호화폐의 계정관리와 동일한 절차 및 방법을 통해 블록체인의 Transaction Confirmation 레벨에서의 전자 문서 무결성 검증 및 변경 이력 검증이 가능한 방법을 제시하고자 한다.

## Specification
#### Data Account
X.Blockchain 에서 특정 문서를 구별하기 위한 식별자로 일반적인 계정 주소를 사용한다. 이는 ATX 의 잔고를 갖는 계정의 주소와 동일한 것이다. 다만 보통의 자산 계정은 '잔고' 를 갖지만 문서(데이터) 계정은 자산의 잔고가 아닌 해당 문서의 무결성 검증의 위한 해쉬 값이 기록되고 이러한 계정을 **Asset Account (자산 계정)** 와 구분하기 위하여 **Data Account (데이터 계정)** 이라 한다.  

**Data Account** 는 다음과 같은 필드로 구성된다.  

- ``address`` : Address - 데이터를 식별을 위한 데이터의 주소. **Data Account** 식별 하기 위한 Key 로 사용된다.
- ``opCnt`` : 현재 계정에 대하여 발생한 오퍼레이션 개수. 생성시 최초 값은 1.
- ``creator`` : Address - 현재 문서의 ~~최초~~ 등록/갱신 계정의 주소.
- ``info`` : byte[], Optional - 현재 문서에 대한 추가 정보.
- ``authors`` : Address[] - 해당 데이터의 업데이트 권한을 갖는 Address.
- ``dataHash`` : byte[] - 데이터 해쉬 값.
- ``reserved`` : byte[], Optional - 사용자 데이터.

> **Note**:exclamation:
>``authors`` 필드는 **Data Account** 변경 권한을 갖는 계정의 주소 목록이다. 이 때 주소값 ``0x0000...0000`` 와 ``0xFFFF...FFFF`` 를 특별한 목적으로 아래와 같이 사용한다.
>- ``0x0000...0000`` 가 설정 되면, 생성자를 포함하여 어느 계정으로도 현재 **Data Account** 를 변경할 수 없다.  
>- ``0xFFFF...FFFF`` 가 설정 되면, 모든 계정이 현재 **Data Account** 를 변경할 수 있다.
>- ``0x0000...0000`` 과 ``0xFFFF...FFFF`` 는 다른 주소와 함께 설정될 수 없다.

<br/>

**Data Account** 의 ``address`` 는 다음과 같이 생성된다.
```
Data Account's address = Keccak256(Transaction_Hash + Nano_Second_Timestamp) [12..31]
```  

<br />

#### Data Register
데이터의 최초 등록(Data Account 생성)시 ``TxFilePayloadBody`` 의 내용은 아래와 같다.  

- ``op`` : byte - 데이터 등록을 의미하는 코드. ``0x01`` 사용.
- ``data`` : Base64Encoded - 등록하고자 하는 데이터. 요청자로 부터의 요청에 포함되고, 이후 처리 과정에서 ``dataHash`` 로 대체된다.
- ``dataHash``: byte[] - ``data`` 의 해쉬 값. ``data`` 필드 대신 블록체인에 저장되는 값.
- ``reserved`` : Base64Encoded, Optional -  사용자 데이터. 어플리케이션에서 필요에 따라 이 필드에 대한 용도를 정의하여 사용할 수 있다.
- ``info`` : byte[], Optional - 등록되는 문서에 대한 추가 정보.
- ``authors`` : Address[] - 등록되는 문서의 업데이트 권한을 갖는 자산 계정의 주소 목록.

현재 Tx. 을 처리하는 과정에서 - 실제 저장 & 전파될 ``TxFilePayloadBody`` 생성 과정 - **Data Account** 의 ``address`` 가 생성 된다. 요청자 측에서는 Tx. 생성 요청의 응답으로 이를 확인할 수 있다.  
그러나 이 시점에서 생성된 주소가 블록체인에 기록됨을 의미하지는 않는다. 블록체인의 기록은 현재 발생된 Tx. 이 블록에 포함되고 해당 블록이 합의 과정을 통과 하여 Commit 되어야 비로소 기록된다. 이 시점에서는 생성된 **Data Account** 의 ``address`` 를 확인 할 수 있을 뿐이다.  
현재 Tx. 이 블록에 포함되어 확정되는 시점에서 **Data Account** 가 생성되고, 이후 데이터의 갱신 또는 검증시에는 **Data Account** 의 ``address`` 를 해당 Tx. 의 ``receiver`` 로 사용한다.  

``authors`` 에는 생성자의 주소가 포함되어야 한다. 그렇지 않으면 데이터의 생성자는 이후 해당 데이터의 업데이트 권한을 갖지 못하게 될 것이다. 그러나 이를 시스템에서 강제하지는 않을 것이며 시스템 사용자의 주의 사항으로 남겨 둘 것이다.  

>**Note**:exclamation:  
>``authors`` 는 적어도 하나 이상의 주소가 포함 되어야 한다.

<br />


데이터 등록을 위한 ``TxFilePayloadBody`` 에 대하여 다음과 같은 확인이 Transaction Confirmation 단계에서 추가적으로 이루어져야 한다. (``Transaction.data.sender`` 의 잔고 확인, TxFee 에 대한 확인 등은 생략)

1. ``TxFilePayloadBody`` 에 필수 필드 존재 확인.
2. ``authors`` 에 적어도 하나 이상의 주소 설정 확인.
3. ``Transaction.data.sender`` 의 PublickKey 로 ``signature`` 를 검증한다.

CheckTx 를 통과한 ``TxFilePayloadBody`` 은 ``data`` 가 ``data``의 해쉬값 인 ``dataHash`` 로 대체되어 X.Consensus 의 Mempool 에 추가되고 주변 노드로 전파 된다.

<br />

#### Data Update
이미 등록된 데이터(Data Account 의 내용)의 업데이트시 ``TxFilePayloadBody`` 는 다음과 같은 내용을 포함한다.

- ``op`` : byte - 데이터 변경을 의미하는 코드. ``0x02`` 사용.
- ``data`` : Base64Encoded, Optional - 변경된 데이터. 데이터의 변경이 아닌 ``authors`` 에 대한 변경 요청인 경우, 이 필드는 생략될 수 있다. 또한 이 필드는 요청자 측에서 Tx. 발생시만 필요하며, 노드 사이에 전파되는 과정에서 이 필드는 ``dataHash``로 대체된다.
- ``dataHash``: byte[], Optional - ``data`` 의 해쉬 값. ``data`` 가 존재하는 경우, ``dataHash`` 값이 ``data`` 필드값을 대체 하여 블록체인에 저장되고 전파 될 것이다.
- ``reserved`` : Base64Encoded, Optional -  어플리케이션에서 필요에 따라 이 필드에 대한 용도를 정의하여 사용할 수 있다.
- ``info`` : byte[], Optional - 등록되는 문서에 대한 추가 정보.
- ``authors`` : Address[], Optional - 등록되는 문서의 업데이트 권한을 갖는 자산 계정의 주소 목록. Data Account 의 author list 의 변경이 필요 없을 경우, 이 필드는 생략될 수 있다.

> **Note**:exclamation:
>- **Data Account** 의 필드중 변경이 필요한 필드만 새로운 값을 갖고, 변경이 필요 없는 필드는 생략하거나 값을 지정하지 않는다. 즉, 업데이트 요청시 값이 지정되지 않은 필드는 현재 **Data Account** 의 값이 유지 된다.
>- 변경할 **Data Account** 주소는 현재 ``TxFilePayloadBody`` 를 포함하고 있는 ``Transaction.data.receiver`` 필드를 사용한다.
>- ``authors`` 필드는 **Data Account** 변경 권한을 갖는 계정 주소 목록인데, 이 때 주소값 ``0x0000...0000`` 와 ``0xFFFF...FFFF`` 는 위에서 언급한 바와 같이 특별한 목적으로 사용되므로 다른 일반적인 주소와 함께 지정될 수 없다.

<br/>

업데이트를 위한 ``TxFilePayloadBody`` 에 대하여 다음과 같은 확인이 Transaction Confirmation 단계에서 추가적으로 이루어져야 한다.

1. ``TxFilePayloadBody`` 에 필수 필드 존재 확인. Optional 필드중 적어도 하나 이상 존재 확인.
2. 현재 **Data Account** 의 ``authors`` 에 ``Transaction.data.sender`` 존재 여부 확인.  
4. ``Transaction.data.sender`` 의 PublickKey 로 ``signature`` 를 검증한다.

CheckTx 를 통과한 ``TxFilePayloadBody`` 은 ``data`` 가 ``data``의 해쉬값 인 ``dataHash`` 로 대체되어 X.Consensus 의 Mempool 에 추가되고 주변 노드로 전파 된다.

<br />

#### Data Verification
등록된 데이터에 대한 무결성 검증시 ``TxFilePayloadBody`` 는 다음과 같은 내용을 포함한다.

- ``op`` : byte - 데이터 검증 요청을 의미하는 코드. ``0x03`` 사용.
- ``address`` : Address - Data Account 의 주소.
- ``data`` : Base64Encoded - 변경된 데이터.  

~~데이터의 무결성을 검증하기 위한 Tx. 은 주변 노드로 전파되지 않고 요청자측과 연결된 노드에서 처리되어 진다.~~ 이 때 검증 요청을 처리하는 노드는 다음과 같은 절차를 거쳐 데이터의 무결성을 검증한다.

**Step 1. DataAccount 검증**
```js
// DataAccount 레벨 검증

result = CheckTxFields(TxFilePayloadBody) //TxFilePayloadBody 필수 필드 확인.
DATAHASH = Hash( TxFilePayloadBody.data ) // Hash 계산.
DATAACCOUNT = FindDataAccount( TxFilePayloadBody.address ) // DataAccount 조회.
result = Equal( DATAHASH, DATAACCOUNT.dataHash ) // dataHash 비교.
```

**Step 2. Transaction 검증**
```js
// Transaction 레벨 검증

TX = FindTx( ??? ) // yskwon: 트랜잭션 조회 가능 ???
result &= Equal( DATAHASH, TX.TxFilePayloadBody.dataHash )
resule &= VerifySig(TX.S, TX.R, TX.V, ... ) // 트랜잭션 서명 검증
```

**Step 3. Block 검증**
```js
// Block 레벨 검증

// n : DataAccount.dataHash 가 업데이트된 시점의 BlockHeight

// DATAHASH 를 포함시켜 계산한 'n 번째 Block' 의 AppHash 계산
// DataAccount 검증시 요청된 DATAHASH 가 DataAccount 의 dataHash 와 동일함을 확인한 상태이므로,
// 현재 IAVL Tree 의 root hash (APPHASH)를 '계산'하여 다음 Block 의 AppHash 와 비교.
APPHASH = ComputeAppHashAt(n, ...)
BLOCK = FindBlock(n+1);
result &= Equal( APPHASH, BLOCK.Header.AppHash )

for (i=n; i < Min(n+31, N); i++) {
  // Validator's Signature 확인'
  for(j=0; j < validators_length; j++) {
    result &= VerifySig(BLOCK.LastCommit.Precommits[j],...)
  }

  // Block Hash 연결 확인
  NEXTBLOCK = FindBlock(i)
  result &= Equal(BLOCK.Hash(), NEXTBLOCK.Header.LastBlockID.Hash)

  BLOCK = NEXTBLOCK
}

// yskwon: 분산 검증 추가 ???, DDOS 공격 ???

```

검증 결과는 다음과 같은 필드를 포함한다.
- ``address`` : Address - Data Account 의 주소.
- ``dataHash`` : byte[] - 데이터 해쉬 값.
- ``result`` : bool - 검증 결과.
- ``confirmations`` : int64 - Block 검증시 확인한 블록의 개수.
- ~~``nodes`` : int32 - 검증에 참여한 노드의 수~~  

<br />

#### Data Search
등록된 데이터 조회시 ``TxFilePayloadBody`` 는 다음과 같은 내용을 포함한다.  

- ``op`` : byte - 데이터 조회를 의미하는 코드. ``0x04`` 사용.
- ``address`` : Address - Data Account 의 주소.

이 조회 요청에 대한 응답은 다음과 같은 정보를 포함한다.
- ``address`` : Address - Data Account 의 주소.
- ``opCnt`` : Int - 현재 address 에 대하여 발생된 오퍼레이션의 개수.
- ``creator`` : Address - 현재 문서의 최초 등록 계정의 주소.
- ``info`` : String - 현재 문서에 대한 추가 정보.
- ``authors`` : Address[] - 해당 데이터의 업데이트 권한을 갖는 Address.
- ``dataHash`` : byte[] - 데이터 해쉬 값.
