# ontology-kotlin-sdk NeoVm Testing

<!-- TOC -->

- [ontology-kotlin-sdk NeoVm Testing](#ontology-kotlin-sdk-neovm-testing)
    - [Overview](#overview)
    - [sendRawTransaction](#sendrawtransaction)
        - [Test Code](#test-code)
        - [Test Result](#test-result)
    - [makeDeployCodeTransaction](#makedeploycodetransaction)
        - [Test Code](#test-code)
        - [Test Result](#test-result)
    - [MakeInvokeTransaction](#makeinvoketransaction)
        - [Test Code](#test-code)
        - [Test Smart Contract](#test-smart-contract)
        - [Test Result](#test-result)
    - [sendTransaction](#sendtransaction)
        - [Test Code](#test-code)
        - [Test Smart Contract](#test-smart-contract)
        - [Test Result](#test-result)

<!-- /TOC -->

## Overview

- :egg: 表示在`ontology-kotlin-sdk`中未找到对应的接口。

- :hatching_chick:表示在`ontology-kotlin-sdk`中有对应接口，但可能存在不完善的地方。

- :hatched_chick: 表示在`ontology-kotlin-sdk`中存在完全对应接口。

| Result          |                             |
| :-------------: | :-------------------------: |
| :hatched_chick: | sendRawTransaction()        |
| :hatched_chick: | makeDeployCodeTransaction() |
| :hatched_chick: | MakeInvokeTransaction()     |
| :hatched_chick: | sendTransaction             |

## sendRawTransaction

### Test Code

```Kotlin
    fun testSendTransaction() {
        OntSdk.setConnectTestNet()
        val codeHex = "54c56b6c766b00527ac46c766b51527ac4616c766b00c36c766b52527ac46c766b52c30548656c6c6f87630600621a006c766b51c300c36165230061516c766b53527ac4620e00006c766b53527ac46203006c766b53c3616c756651c56b6c766b00527ac46151c576006c766b00c3c461681553797374656d2e52756e74696d652e4e6f7469667961616c7566"
        val abi = "{\"hash\":\"0x362cb5608b3eca61d4846591ebb49688900fedd0\",\"entrypoint\":\"Main\",\"functions\":[{\"name\":\"Main\",\"parameters\":[{\"name\":\"operation\",\"type\":\"String\"},{\"name\":\"args\",\"type\":\"Array\"}],\"returntype\":\"Any\"},{\"name\":\"Hello\",\"parameters\":[{\"name\":\"msg\",\"type\":\"String\"}],\"returntype\":\"Void\"}],\"events\":[]}"
        val abiInfo = JSON.parseObject(abi, AbiInfo::class.java)
        val func = abiInfo.getFunction("Hello")
        func!!.setParamsValue("value")
        val codeAddress = Address.AddressFromVmCode(codeHex).toHexString()
        print("codeAddress")
        println(codeAddress)
        val result = NeoVm.sendTransaction(Helper.reverse(codeAddress), null, null, 0, 0, func, true)
        println(result)
    }
```

### Test Result

```bash
POST url=http://polaris1.ont.io:20336,{"jsonrpc":"2.0","method":"sendrawtransaction","params":["00d14576097e000000000000000000000000000000000000000000000000000000000000000000000000230576616c756551c10548656c6c6f67d0ed0f908896b4eb916584d461ca3e8b60b52c360000",1],"id":1}
{"State":1,"Gas":20000,"Result":"01"}
```

## makeDeployCodeTransaction

### Test Code

```Kotlin
fun testMakeDeployTransaction() {
    OntSdk.setConnectTestNet()
    val privateKey = "523c5fcf74823831756f0bcb3634234f10b3beb1c05595058534577752ad2d9f"
    val payer = Account(Helper.hexToBytes(privateKey), SignatureScheme.SHA256WITHECDSA)
    val b58Payer = payer.addressU160.toBase58()
    val codeHex = "54c56b6c766b00527ac46c766b51527ac4616c766b00c36c766b52527ac46c766b52c30548656c6c6f87630600621a006c766b51c300c36165230061516c766b53527ac4620e00006c766b53527ac46203006c766b53c3616c756651c56b6c766b00527ac46151c576006c766b00c3c461681553797374656d2e52756e74696d652e4e6f7469667961616c7566"
    val codeAddress = Address.AddressFromVmCode(codeHex).toHexString()
    val gasLimit: Long = 20000000
    val gasPrice: Long = 500
    val tx = Vm.makeDeployCodeTransaction(codeHex, true, "DX", "v1.0", "DX_TEST", "email", "deploy for test", b58Payer, gasLimit, gasPrice)
    OntSdk.signTx(tx, arrayOf(arrayOf(payer)))
    val result = OntSdk.rpc.sendRawTransaction(tx.toHexString())
    print("DeployTransaction: ")
    println(result)
    Thread.sleep(6000)
    val contract = OntSdk.rpc.getContract(codeAddress)
    print("Contract: ")
    println(contract)
    }
```

### Test Result

```bash
POST url=http://polaris1.ont.io:20336,{"jsonrpc":"2.0","method":"sendrawtransaction","params":["00d0fdadfe56f401000000000000002d3101000000004756c9dd829b2142883adbe1ae4f8689a1f673e98d54c56b6c766b00527ac46c766b51527ac4616c766b00c36c766b52527ac46c766b52c30548656c6c6f87630600621a006c766b51c300c36165230061516c766b53527ac4620e00006c766b53527ac46203006c766b53c3616c756651c56b6c766b00527ac46151c576006c766b00c3c461681553797374656d2e52756e74696d652e4e6f7469667961616c7566010244580476312e300744585f5445535405656d61696c0f6465706c6f7920666f72207465737400014241012c1405d8fe3d6353388541a7e4ad32ee6c8d91306afd383cd23ed64a6ebcc7f378f973a494aa1a83debade1655bd69851032513b9ce1650dc0ae0ddb9d90ba94232103036c12be3726eb283d078dff481175e96224f0b0c632c7a37e10eb40fe6be889ac"],"id":1}
DeployTransaction: true
POST url=http://polaris1.ont.io:20336,{"jsonrpc":"2.0","method":"getcontractstate","params":["362cb5608b3eca61d4846591ebb49688900fedd0",1],"id":1}
Contract: {"NeedStorage":true,"Email":"XXX@XXXX.com","Description":"Hello World","CodeVersion":"1.0","Author":"Tester","Code":"54c56b6c766b00527ac46c766b51527ac4616c766b00c36c766b52527ac46c766b52c30548656c6c6f87630600621a006c766b51c300c36165230061516c766b53527ac4620e00006c766b53527ac46203006c766b53c3616c756651c56b6c766b00527ac46151c576006c766b00c3c461681553797374656d2e52756e74696d652e4e6f7469667961616c7566","Name":"MyHello"}
```

## MakeInvokeTransaction

### Test Code

```Kotlin
fun testMakeInvokeTransaction() {
    OntSdk.setConnectTestNet()
    val codeHex = "54c56b6c766b00527ac46c766b51527ac4616c766b00c36c766b52527ac46c766b52c30548656c6c6f87630600621a006c766b51c300c36165230061516c766b53527ac4620e00006c766b53527ac46203006c766b53c3616c756651c56b6c766b00527ac46151c576006c766b00c3c461681553797374656d2e52756e74696d652e4e6f7469667961616c7566"
    val abi = "{\"hash\":\"0x362cb5608b3eca61d4846591ebb49688900fedd0\",\"entrypoint\":\"Main\",\"functions\":[{\"name\":\"Main\",\"parameters\":[{\"name\":\"operation\",\"type\":\"String\"},{\"name\":\"args\",\"type\":\"Array\"}],\"returntype\":\"Any\"},{\"name\":\"Hello\",\"parameters\":[{\"name\":\"msg\",\"type\":\"String\"}],\"returntype\":\"Void\"}],\"events\":[]}"
    val abiInfo = JSON.parseObject(abi, AbiInfo::class.java)
    val func = abiInfo.getFunction("Hello")
    func!!.setParamsValue("value")
    val params = BuildParams.serializeAbiFunction(func)
    val codeAddress = Address.AddressFromVmCode(codeHex).toHexString()
    val gasLimit: Long = 0
    val gasPrice: Long = 0
    val tx = Vm.makeInvokeCodeTransaction(Helper.reverse(codeAddress), null, params, null, gasLimit, gasPrice)
    val result = OntSdk.rpc.sendRawTransactionPreExec(tx.toHexString())
    print("testMakeInvokeTransaction: ")
    println(result)
}
```

### Test Smart Contract

```c#
using Ont.SmartContract.Framework.Services.Ont;
using Ont.SmartContract.Framework;
using System;
using System.ComponentModel;

namespace Ont.SmartContract
{
    public class HelloWorld : Framework.SmartContract
    {
        public static object Main(string operation, params object[] args)
        {
            switch (operation)
            {
                case "Hello":
                    Hello((string)args[0]);
                    return true;
                default:
                    return false;
            }
        }
        public static void Hello(string msg)
        {
            Runtime.Notify(msg);
        }
    }
}
```

### Test Result

```bash
testMakeInvokeTransaction: {"State":1,"Gas":20000,"Result":"01"}
```

## sendTransaction

### Test Code

```Kotlin
fun testSendTransaction() {
    OntSdk.setConnectTestNet()
    val codeHex = "54c56b6c766b00527ac46c766b51527ac4616c766b00c36c766b52527ac46c766b52c30548656c6c6f87630600621a006c766b51c300c36165230061516c766b53527ac4620e00006c766b53527ac46203006c766b53c3616c756651c56b6c766b00527ac46151c576006c766b00c3c461681553797374656d2e52756e74696d652e4e6f7469667961616c7566"
    val abi = "{\"hash\":\"0x362cb5608b3eca61d4846591ebb49688900fedd0\",\"entrypoint\":\"Main\",\"functions\":[{\"name\":\"Main\",\"parameters\":[{\"name\":\"operation\",\"type\":\"String\"},{\"name\":\"args\",\"type\":\"Array\"}],\"returntype\":\"Any\"},{\"name\":\"Hello\",\"parameters\":[{\"name\":\"msg\",\"type\":\"String\"}],\"returntype\":\"Void\"}],\"events\":[]}"
    val abiInfo = JSON.parseObject(abi, AbiInfo::class.java)
    val func = abiInfo.getFunction("Hello")
    func!!.setParamsValue("value")
    val codeAddress = Address.AddressFromVmCode(codeHex).toHexString()
    print("codeAddress: ")
    println(codeAddress)
    val result = NeoVm.sendTransaction(Helper.reverse(codeAddress), null, null, 0, 0, func, true)
    println(result)
}
```

### Test Smart Contract

```c#
using Ont.SmartContract.Framework.Services.Ont;
using Ont.SmartContract.Framework;
using System;
using System.ComponentModel;

namespace Ont.SmartContract
{
    public class HelloWorld : Framework.SmartContract
    {
        public static object Main(string operation, params object[] args)
        {
            switch (operation)
            {
                case "Hello":
                    Hello((string)args[0]);
                    return true;
                default:
                    return false;
            }
        }
        public static void Hello(string msg)
        {
            Runtime.Notify(msg);
        }
    }
}
```

### Test Result

```bash
POST url=http://polaris1.ont.io:20336,{"jsonrpc":"2.0","method":"sendrawtransaction","params":["00d1536c6b6a000000000000000000000000000000000000000000000000000000000000000000000000230576616c756551c10548656c6c6f67d0ed0f908896b4eb916584d461ca3e8b60b52c360000",1],"id":1}
{"State":1,"Gas":20000,"Result":"01"}
```