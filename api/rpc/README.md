# 정의사항

* 필수사항 : M(Mandatory), 선택사항 : O(Optional)
* HTTP 통신을 통하여 질의 응답하며, `Post로 요청 해야한다.` (추후 HTTPS 고려.)
* `데이터 요청과 응답은 Json 형식`을 사용하며, 요청시 Request Body에 Json 형태로 입력한다.
* URI(Resource)는 따로 정의하지 않고, "/"로 요청한다.
  * 질의시 Json 데이터에 호출 서비스명과 함수를 명시한다. ex) tx_sendTransaction
  

## API Type
- [Account](./rpc-account.md)
- [Block](./rpc-block.md)
- [Network](./rpc-network.md)
- [Node](./rpc-node.md)
- [Tx](./rpc-tx.md)
- [Validator](./rpc-validator.md)
