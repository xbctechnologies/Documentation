트랜잭션을 사용하기 위해서는 2가지의 객체를 이해해야 하며 이 둘을 조합하여 트랜잭션을 생성할 수 있습니다.

# TxRequest
```java
public class TxRequest {
    private Long reqId;             // Set if you want the application to set the ID for the request
    private boolean isSync;         // If the value is true, it responds after block creation.
    private String targetChainId;   // Set the unique xchain ID on xblockchain
    private String sender;          // Set the address of the account that generates the transaction
    private String receiver;        // Set the address of the account receiving the transaction
    private Long txNonce;           // Set the tx nonce of address
    private BigInteger fee;         // Set the commission for creating the transaction
    private BigInteger amount;      // Set the amount of coins to send to the receiver
    private BigInteger time;        // Input time at signing when transaction is signed by client
 
    private int v;
    private BigInteger r;
    private BigInteger s;
 
    private ApiEnum.PayloadType payloadType;    // Set the type of tx
    private TxBody payloadBody;
}
```

# TxBody
TxBody는 트랜잭션의 종류에 따라 템플릿 클래스로 작성되어 있습니다. 따라서 종류에 따라 파라미터가 다릅니다.

## Common
- Type : ApiEnum.PayloadType.CommonType
- Body : TxCommonBody
```java
TxCommonBody body = TxCommonBody.builder()
        .withInput("Custom field")  //Optional
        .build();
```
