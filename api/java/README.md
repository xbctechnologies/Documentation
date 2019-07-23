# Java API

- [tx](./java-tx.md)
- [api](./java-api.md)

# Quick Start

## Maven 설정
[Maven repository](https://mvnrepository.com/)에서 [xcube-core를 검색](https://mvnrepository.com/artifact/com.xbctechnologies/xcube-core)하여 최신 버전을 가져옵니다.

```xml
<dependency>
    <groupId>com.xbctechnologies</groupId>
    <artifactId>xcube-core</artifactId>
    <version>0.1.3</version>
</dependency>
```

## Gradle 설정
```gradle
compile group: 'com.xbctechnologies', name: 'xcube-core', version: '0.1.3'
```

## Tutorial
Xnode에 접속하기 위해서는 Xcube객체를 생성해 주어야 합니다. Xcube객체에서는 필요한 RestHttp 통신을 수행하며 트랜잭션 혹은 그외 호출 기능을 위해 적절한 RPC 통신 프로토콜로 통신하는 기능을 담당합니다.
Xcube객체에 통신에 대한 파라미터를 주어 RPC 서버 기능을 할 수 있는 Xnode의 url과 port를 입력합니다.

### XCube 객체 생성
```java
xCube = new XCube(new RestHttpClient(
        RestHttpConfig.builder()
                .withXNodeUrl("http://${XNODE_URL}:${PORT}")
                .withMaxConnection(100)
                .withDefaultTimeout(60000)
                .build()
));
```

### 계좌 생성
자산을 보관할 계좌를 생성하기 위해서는 패스워드(pass phrase)와 json key파일이 필요한데 이는 아래와 같이 생성가능하다. 이때 생성된 key 파일은 잘 보관되어야 하며 private key는 이 둘의 조합으로 생성된다.
```java
String keystoreDir = System.getProperty("user.home") + "/keystores";
String passPhrase = "password";
 
String account = xCube.createAccount(passPhrase, keystoreDir);
```

```java
String [] result = xCube.createAccountAndPrivateKey();

String privateKey = result[0];
String account = result[1];
```

### Signing
계좌의 트랜잭션 처리를 하기 위해서는 신뢰할 수 있는 요청이 필요하며 이것은 자신의 패스워드와 키파일을 사용하여 서명(Signing)을 한 후에 트랜잭션을 요청한다. Signing 하기 위해  패스워드와 Key 파일의 디렉토리를 지정해줘야 합니다. 이 후 트랜잭션 요청시 이 정보를 사용하여 signing한 트랜잭션을 내부적으로 생성한 후 XNode에 전송합니다.
```java
// Signing 정보 입력
if (xCube.loadKeyStore(account, keystoreDir)) {
    xCube.setPassPhrase("유저의 패스워드");
} else {
    System.err.printf("There is not a account(%s)'s keystore in %s\n", account, keystoreDir);
}
```

```java
xCube.setPrivateKey(privateKey);
```

### 트랜잭션 요청
트랜잭션에 대한 객체를 정의해 주어야 합니다. 기본적인 송금 트랜잭션 같은 경우 보낼 계좌와 받을 계좌 그리고 송금할 양, 수수료 등의 내용을 담을 수 있으며 보다 자세한 것은 이후의 트랜잭션 종류에 언급되어 있습니다.

이렇게 생성한 트랜잭션 요청 객체를 Xcube의 `sendTransaction()`를 이용하여 Xnode 쪽에 보낼 수 있습니다.

```java
// 트랜잭션 전송
TxRequest txRequest = TxRequest.builder()
        .isSync(false)                                      // true : 블록에 포함된 이후 리턴, false : Tx 검증 후 리턴
        .targetChainId(targetChainId)                       // Tx를 전송할 Chain ID
        .sender(sender)
        .receiver(sender)
        .fee(CurrencyUtil.generateXTO(CoinType, 2))         // Tx의 수수료
        .amount(CurrencyUtil.generateXTO(CoinType, 10))     // Receiver에게 전송할 토큰양
        .payloadType(ApiEnum.PayloadType.CommonType)        // Tx 타입정의
        .payloadBody(new TxCommonBody("1st transaction"))   // Tx 내용정의 (하단에 정의된 트랜잭션 타입별 Body를 이용)
        .build();
TxSendResponse txSendResponse = xCube.sendTransaction(txRequest).send();
```

그외 API 사용
기본적인 트랜잭션 외에도 계좌액 조회, 피어 상태, Validator 정보 등등 정보조회용의 API들이 있습니다. 사용 형태는 아래와 같은 형태로 사용하며 response속에 정보를 `getXXXX()` 하는 형태로 얻을 수 있습니다.

```java
// 계좌 조회
AccountBalanceResponse balance = xCube.getBalance(null, "1A", account, CoinType).send();
 
if (balance.getBalance() == null) {
    // error occurred
    System.out.println(balance.getError().getCode());
    System.out.println(balance.getError().getMessage());
} else {
    System.out.println(balance.getBalance());
}
```
