# ontology-kotlin-sdk Restful Testing

<!-- TOC -->

- [ontology-kotlin-sdk Restful Testing](#ontology-kotlin-sdk-restful-testing)
    - [Overview](#overview)
    - [getVersion()](#getversion)
        - [Test Code](#test-code)
        - [Test Result](#test-result)
    - [getNodeCount()](#getnodecount)
        - [Test Code](#test-code)
        - [Test Result](#test-result)
    - [GetBlock()](#getblock)
        - [Test Code](#test-code)
        - [Test Result](#test-result)
    - [getBlockHeight()](#getblockheight)
        - [Test Code](#test-code)
        - [Test Result](#test-result)
    - [getBalance()](#getbalance)
        - [Test Code](#test-code)
        - [Test Result](#test-result)
    - [getAllowance](#getallowance)
        - [Test Code](#test-code)
        - [Test Result](#test-result)
    - [getStorage](#getstorage)
        - [Test Code](#test-code)
        - [Test Result](#test-result)
    - [getSmartCodeEvent()](#getsmartcodeevent)
        - [Test Code](#test-code)
        - [Test Result](#test-result)
    - [getTransaction()](#gettransaction)
        - [Test Code](#test-code)
        - [Test Result](#test-result)
    - [getMerkleProof()](#getmerkleproof)
        - [Test Code](#test-code)
        - [Test Result](#test-result)
    - [SendRawTransaction()](#sendrawtransaction)
        - [Test Code](#test-code)
        - [Test Result](#test-result)
    - [sendRawTransactionPreExec()](#sendrawtransactionpreexec)
        - [Test Code](#test-code)
        - [Test Result](#test-result)

<!-- /TOC -->

## Overview

- :egg: 表示在`ontology-kotlin-sdk`中未找到对应的接口。

- :hatching_chick:表示在`ontology-kotlin-sdk`中有对应接口，但可能存在不完善的地方。 

- :hatched_chick: 表示在`ontology-kotlin-sdk`中存在完全对应接口。

|      Result      |                             |
|:----------------:|:---------------------------:|
|  :hatched_chick: |         getVersion()        |
|       :egg:      |         getGasPrice         |
|       :egg:      |         getNetworkId        |
| :hatching_chick: |          GetBlock()         |
|  :hatched_chick: |       getBlockHeight()      |
|  :hatched_chick: |         getBalance()        |
|  :hatched_chick: |         getAllowance        |
|  :hatched_chick: |          getStorage         |
|  :hatched_chick: |     getSmartCodeEvent()     |
| :hatching_chick: |       getTransaction()      |
|  :hatched_chick: |       getMerkleProof()      |
|  :hatched_chick: |     SendRawTransaction()    |
|  :hatched_chick: | sendRawTransactionPreExec() |

## getVersion()

### Test Code

```Kotlin
fun testGetVersion() {
    val restUrl = "http://polaris3.ont.io:20334"
    OntSdk.setRestful(restUrl)
    val version = OntSdk.restful.getVersion()
    print("Version: ")
    println(version)
}
```

### Test Result

```bash
Version: v1.0.2
```

## getNodeCount()

### Test Code

```Kotlin
fun testGetNodeCount() {
    val restUrl = "http://polaris3.ont.io:20334"
    OntSdk.setRestful(restUrl)
    val count = OntSdk.restful.getNodeCount()
    print("NodeCount: ")
    println(count)
}
```

### Test Result

```bash
NodeCount: 12
```

## GetBlock()

### Test Code

```Kotlin
fun testGetBlockByHash() {
    val restUrl = "http://polaris3.ont.io:20334"
    OntSdk.setRestful(restUrl)
    val hash = "44425ae42a394ec0c5f3e41d757ffafa790b53f7301147a291ab9b60a956394c"
    val block = OntSdk.restful.getBlock(hash)
    print("Block: ")
    println(block)
}
```

```Kotlin
fun testGetBlockByHeight() {
    val restUrl = "http://polaris3.ont.io:20334"
    OntSdk.setRestful(restUrl)
    val height = 0
    val block = OntSdk.restful.getBlock(height)
    print("Block: ")
    println(block)
}
```

### Test Result

```bash
Block: com.github.ontio.core.block.Block@a956394c
```

![Alt text](../img/restfulGetBlock_1.png)

```bash
Block: com.github.ontio.core.block.Block@a956394c
```

![Alt text](../img/restfulGetBlock_2.png)

![Alt text](../img/rpcGetBlock_5.png)

## getBlockHeight()

### Test Code

```Kotlin
fun testGetBlockHeight() {
    val restUrl = "http://polaris1.ont.io:20334"
    OntSdk.setRestful(restUrl)
    val blockHeight = OntSdk.restful.getBlockHeight()
    print("BlockHeight: ")
    println(blockHeight)
}
```

### Test Result

```bash
BlockHeight: 179851
```

## getBalance()

### Test Code

```Kotlin
fun testGetBalance() {
    val restUrl = "http://polaris1.ont.io:20334"
    OntSdk.setRestful(restUrl)
    val b58Address = "AazEvfQPcQ2GEFFPLF1ZLwQ7K5jDn81hve"
    val balance = OntSdk.restful.getBalance(b58Address)
    print("Balance: ")
    println(balance)
}
```

### Test Result

```bash
Balance: {"ont":"74308794","ong":"1753934689961983"}
```

## getAllowance

### Test Code

```Kotlin
fun testSendRawTransaction() {
    val restUrl = "http://polaris1.ont.io:20334"
    OntSdk.setRestful(restUrl)
    val privateKey = "523c5fcf74823831756f0bcb3634234f10b3beb1c05595058534577752ad2d9f"
    val payer = Account(Helper.hexToBytes(privateKey), SignatureScheme.SHA256WITHECDSA)
    val b58Payer = payer.addressU160.toBase58()
    val b58Recv = "AazEvfQPcQ2GEFFPLF1ZLwQ7K5jDn81hve"
    println("balance: " + OntSdk.restful.getBalance(payer.addressU160.toBase58()))
    val transaction = Ont.makeTransfer(b58Payer, b58Recv, 1, b58Payer, 20000, 500)
    OntSdk.signTx(transaction, arrayOf(arrayOf(payer)))
    val result = OntSdk.restful.sendRawTransaction(transaction)
    print("sendRawTransaction: ")
    println(result)
}
```

### Test Result

```bash
Allowance: 1
Allowance: 0
```

## getStorage

### Test Code

```Kotlin
fun testGetStorage() {
    val restUrl = "http://polaris1.ont.io:20334"
    OntSdk.setRestful(restUrl)
    val contractAddress = "0100000000000000000000000000000000000000"
    val key = "746f74616c537570706c79"
    val value = OntSdk.restful.getStorage(contractAddress, key)
    print("Storage: ")
    println(value)
}
```

### Test Result

```bash
Storage: 00ca9a3b00000000
```

## getSmartCodeEvent()

### Test Code

```Kotlin
fun testGetSmartCodeEvent() {
    val restUrl = "http://polaris1.ont.io:20334"
    OntSdk.setRestful(restUrl)
    val txHash = "65d3b2d3237743f21795e344563190ccbe50e9930520b8525142b075433fdd74"
    val eventByHash = OntSdk.restful.getSmartCodeEvent(txHash)
    print("Event: ")
    println(eventByHash)
    val height = 0
    val eventByHeight = OntSdk.restful.getSmartCodeEvent(height)
    print("Event: ")
    println(eventByHeight)
}
```

### Test Result

```bash
Event: {"GasConsumed":0,"Notify":[],"TxHash":"65d3b2d3237743f21795e344563190ccbe50e9930520b8525142b075433fdd74","State":1}
Event: [{"GasConsumed":0,"Notify":[],"TxHash":"65d3b2d3237743f21795e344563190ccbe50e9930520b8525142b075433fdd74","State":1},{"GasConsumed":0,"Notify":[],"TxHash":"e67e5c934dd165bec2156835e5af06696bc42cc14daa04a5846ebb7e60cc4701","State":1},{"GasConsumed":0,"Notify":[],"TxHash":"5d09b2b9ba302e9da8b9472ef10c824caf998e940cc5a73d7da16971d64c0290","State":1},{"GasConsumed":0,"Notify":[],"TxHash":"e492d21464c459f310656d66b1388622f81d5b1ebdb06ccb364f68145b2f1c26","State":1},{"GasConsumed":0,"Notify":[],"TxHash":"e6592dda267eec1867d61a5dd1f6ccbf23565a526da7a8230f5c5a27591d27de","State":1},{"GasConsumed":0,"Notify":[],"TxHash":"7842ed25e4f028529e666bcecda2795ec49d570120f82309e3d5b94f72d30ebb","State":1},{"GasConsumed":0,"Notify":[{"States":["transfer","AFmseVrdL9f9oyCzZefL9tG6UbvhPbdYzM","AZW8eBkXh5qgRjmeZjqY2KFGLXhKcX4i2Y",1000000000],"ContractAddress":"0100000000000000000000000000000000000000"}],"TxHash":"1ebde66ec3f309dad20a63f8929a779162a067c36ce7b00ffbe8f4cfc8050d79","State":1},{"GasConsumed":0,"Notify":[{"States":["transfer","AFmseVrdL9f9oyCzZefL9tG6UbvhPbdYzM","AFmseVrdL9f9oyCzZefL9tG6UbvhUMqNMV",1000000000000000000],"ContractAddress":"0200000000000000000000000000000000000000"}],"TxHash":"7e8c19fdd4f9ba67f95659833e336eac37116f74ea8bf7be4541ada05b13503e","State":1},{"GasConsumed":0,"Notify":[],"TxHash":"bf74e9208c0a20ec417de458ab6c9d29c12c614e77fb943be4566c95fab61454","State":1},{"GasConsumed":0,"Notify":[{"States":["initContractAdmin","0700000000000000000000000000000000000000","did:ont:AMAx993nE6NEqZjwBssUfopxnnvTdob9ij"],"ContractAddress":"0600000000000000000000000000000000000000"}],"TxHash":"12943957b10643f04d89938925306fa342cec9d32925f5bd8e9ea7ce912d16d3","State":1}]
```

## getTransaction()

### Test Code

```Kotlin
fun testGetTransaction() {
    val restUrl = "http://polaris1.ont.io:20334"
    OntSdk.setRestful(restUrl)
    val txHash = "65d3b2d3237743f21795e344563190ccbe50e9930520b8525142b075433fdd74"
    val transaction = OntSdk.restful.getTransaction(txHash)
    print("Transaction: ")
    println(transaction)
}
```

### Test Result

```bash
Transaction: com.github.ontio.core.payload.DeployCode@433fdd74
```

![Alt text](../img/getTransaction_1.png)

![Alt text](../img/getTransaction_2.png)

## getMerkleProof()

### Test Code

```Kotlin
fun testGetMerkleProof() {
    val restUrl = "http://polaris1.ont.io:20334"
    OntSdk.setRestful(restUrl)
    val txHash = "65d3b2d3237743f21795e344563190ccbe50e9930520b8525142b075433fdd74"
    val proof = OntSdk.restful.getMerkleProof(txHash)
    print("MerkleProof: ")
    println(proof)
}
```

### Test Result

```bash
POST url=http://polaris1.ont.io:20334,{"jsonrpc":"2.0","method":"getmerkleproof","params":["65d3b2d3237743f21795e344563190ccbe50e9930520b8525142b075433fdd74"],"id":1}
MerkleProof: {"TransactionsRoot":"39c225c72f9bd76cf2030ec96057531a403887b4ea2801d5137ac000e847cb4d","Type":"MerkleProof","CurBlockRoot":"a2bba1bddc8f2be35661e8e3855640f2f6a0b71006555b80df691c6751ea4196","CurBlockHeight":179944,"BlockHeight":0,"TargetHashes":["fb2530764e458ef5b94147a687e85cbcf46ab575e1869730f792e57176d1a99b","57f1a5a6d76804b8b78800101271f962e072e86c817b798012a087c149feb2dd","c67a1112f389a59c2d96c0d140c54947124741d8043b8283af89f38a9c127146","e7bc6dc7e96b579cebc3f50a36912ca579b7be19174de20b6fa1392a5b968b97","b29f69ef311ab8de9d7521d8342e625ca7e6a83b0ab744dd37d487eb04015f55","158549153fb2e72890c18744ad71260c30815bb8be2018f6a67fc1bec5b9350e","1e28f633ff0dd320feaa4e57a8c0b7da3bce3bff2d4ff6144df51b696a323160","e0138809da29afa741a03101dd783432b1e19063f618caae835f6be95b785803","cfb6a4d77bca71af28d693785e254941196ddf2d6076cdc9b0fa33b6f04cc981","539e219f4c8c5a8380cade1d71c2aa793f3266bea2ffb81da4f540aa02a9e535","ec831558466cecde520b456a7c981d5e0eb9ab9bb4d6918a282449c6467515cd","5f50b59f27ebeac7144e39a0ca3a90784d3194f488ee23c28af17cfe69ac07a6","efafc2f48bd0adbb9d1912ef5f09844c81c9bdd0ce8bfc1ad2e85f667c224395","2470009a1c62bad043933e8e770100378dd361b38ee52d5b7b37104bb9b0d26b","2f3a6afaa3b9b9df53b2fc24e3683362e5195e3610f2502cbae56aa35ae5b3a0","1dfea4e74b4ba7651d73231f2f57cfba5450014448f55380fbb1d48b91d987db","052716bc0151b2e707242d156ae2abd123a4bedab5c4e29706d6c5fb3a76f5c2","e688164d9b2309f0de52c988bb07d9e4b251092cd7178fccedcd1c996fbff2fe"]}
```

## SendRawTransaction()

### Test Code

```Kotlin
fun testSendRawTransaction() {
    val restUrl = "http://polaris1.ont.io:20334"
    OntSdk.setRestful(restUrl)
    val privateKey = "523c5fcf74823831756f0bcb3634234f10b3beb1c05595058534577752ad2d9f"
    val payer = Account(Helper.hexToBytes(privateKey), SignatureScheme.SHA256WITHECDSA)
    val b58Payer = payer.addressU160.toBase58()
    val b58Recv = "AazEvfQPcQ2GEFFPLF1ZLwQ7K5jDn81hve"
    println("balance: " + OntSdk.restful.getBalance(payer.addressU160.toBase58()))
    val transaction = Ont.makeTransfer(b58Payer, b58Recv, 1, b58Payer, 20000, 500)
    OntSdk.signTx(transaction, arrayOf(arrayOf(payer)))
    val result = OntSdk.restful.sendRawTransaction(transaction)
    print("sendRawTransaction: ")
    println(result)
}
```

### Test Result

```bash
balance: {"ont":"890350","ong":"2351456658805"}
POST url=http://polaris1.ont.io:20334/api/v1/transaction,{},{"Action":"sendrawtransaction","Version":"v1.0.0","Data":"00d1ee72a9f8f401000000000000204e0000000000004756c9dd829b2142883adbe1ae4f8689a1f673e97100c66b144756c9dd829b2142883adbe1ae4f8689a1f673e96a7cc814d2c124dd088190f709b684e0bc676d70c41b37766a7cc8516a7cc86c51c1087472616e736665721400000000000000000000000000000000000000010068164f6e746f6c6f67792e4e61746976652e496e766f6b65000142410110926c5f3c94c81cc21e31ab5d4b8a47102df4e4d39ae891cc10396b8da257bdce98677e1f3a9d36321285f13f219cf683f007d80051795dba48ebf370f21ac1232103036c12be3726eb283d078dff481175e96224f0b0c632c7a37e10eb40fe6be889ac"}
sendRawTransaction: true
```

## sendRawTransactionPreExec()

### Test Code

```Kotlin
fun testSendRawTransactionPreExec() {
    val restUrl = "http://polaris1.ont.io:20334"
    OntSdk.setRestful(restUrl)
    val privateKey = "523c5fcf74823831756f0bcb3634234f10b3beb1c05595058534577752ad2d9f"
    val payer = Account(Helper.hexToBytes(privateKey), SignatureScheme.SHA256WITHECDSA)
    val b58Payer = payer.addressU160.toBase58()
    val b58Recv = "AazEvfQPcQ2GEFFPLF1ZLwQ7K5jDn81hve"
    println("balance: " + OntSdk.restful.getBalance(payer.addressU160.toBase58()))
    val transaction = Ont.makeTransfer(b58Payer, b58Recv, 1, b58Payer, 20000, 500)
    OntSdk.signTx(transaction, arrayOf(arrayOf(payer)))
    val result = OntSdk.restful.sendRawTransactionPreExec(transaction.toHexString())
    print("sendRawTransactionPreExec: ")
    println(result)
}
```

### Test Result

```bash
balance: {"ont":"890350","ong":"2351456658805"}
POST url=http://polaris1.ont.io:20334/api/v1/transaction,{"preExec":"1"},{"Action":"sendrawtransaction","Version":"v1.0.0","Data":"00d17fb3095bf401000000000000204e0000000000004756c9dd829b2142883adbe1ae4f8689a1f673e97100c66b144756c9dd829b2142883adbe1ae4f8689a1f673e96a7cc814d2c124dd088190f709b684e0bc676d70c41b37766a7cc8516a7cc86c51c1087472616e736665721400000000000000000000000000000000000000010068164f6e746f6c6f67792e4e61746976652e496e766f6b650001424101e2e776d007f418c5f46f2fa6e33a0a5ada73cc56473c2bb22ce32cd41f448e52a63732b21fa5b87e6eae4940acdc28e9a5e957530539340d926abcfb56a32989232103036c12be3726eb283d078dff481175e96224f0b0c632c7a37e10eb40fe6be889ac"}
sendRawTransactionPreExec: {"State":1,"Gas":20000,"Result":"01"}
```