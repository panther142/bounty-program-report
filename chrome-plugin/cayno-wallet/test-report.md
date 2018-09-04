# Ontology Dapi Demo Review

<!-- TOC -->

- [Ontology Dapi Demo Review](#ontology-dapi-demo-review)
    - [Install](#install)
    - [Interface](#interface)
    - [Ontology Web Wallet](#ontology-web-wallet)
        - [Balance Interface](#balance-interface)
        - [Provider](#provider)
            - [onIsInstalled](#onisinstalled)
            - [onGetName](#ongetname)
            - [onGetVersion](#ongetversion)
        - [Network](#network)
            - [getBlockHeight](#getblockheight)
            - [getBlock](#getblock)
            - [getBalance](#getbalance)
        - [Asset](#asset)
            - [getOwnAccounts](#getownaccounts)
            - [getDefaultAccount](#getdefaultaccount)
            - [makeTransfer](#maketransfer)
        - [Smart Contract](#smart-contract)
            - [Smart Contract Call](#smart-contract-call)
            - [Smart Contract Deploy](#smart-contract-deploy)

<!-- /TOC -->


## Install

```bash
git clone https://github.com/OntologyCommunityDevelopers/ontology-dapi-demo.git
```

```bash
npm install -g opencollective
npm install
npm start
```

![Alt text](img/OEP/Dapi/DapiDemoCompiled.png)

![Alt text](img/OEP/Dapi/DapiDemo.png)

## Interface

![Alt text](img/OEP/Dapi/Asset.png)

## Ontology Web Wallet

> https://github.com/OntologyCommunityDevelopers/ontology-plugin-wallet

![Alt text](img/OEP/Dapi/OntologyWebWallet.png)

![Alt text](img/OEP/Dapi/OntologyWebWallet2.png)

![Alt text](img/OEP/Dapi/OntologyWebWallet3.png)

![Alt text](img/OEP/Dapi/OntologyWebWallet4.png)

![Alt text](img/OEP/Dapi/OntologyWebWallet5.png)

### Balance Interface

`Python SDK`获取到的`ONG`账户余额与插件中显示的账户余额小数点不一致。

![Alt text](img/OEP/Dapi/Balance.png)

### Provider

#### onIsInstalled

![Alt text](img/OEP/Dapi/onIsInstalled.png)

#### onGetName

![Alt text](img/OEP/Dapi/onGetName.png)

#### onGetVersion

![onGetVersion](../../img/OEP/Dapi/onGetVersion.png)

### Network

#### getBlockHeight

![Alt text](img/OEP/Dapi/GetBlockHeight.png)

#### getBlock

在OEP6中定义的接口如下：

```js
function getBlock(block: number | string): Promise<Block>
```

实际使用时没有传入参数的地方，返回结果在区块链浏览器中也未查到。

![Alt text](img/OEP/Dapi/GetBlockHeight.png)

#### getBalance

![Alt text](img/OEP/Dapi/getBalance.png)

返回值与插件中显示的数据不一致。

### Asset

#### getOwnAccounts

![Alt text](img/OEP/Dapi/getOwnAccounts.png)

#### getDefaultAccount

![Alt text](img/OEP/Dapi/getDefaultAccount.png)

#### makeTransfer

![Alt text](img/OEP/Dapi/makeTransfer_1.png)

![Alt text](img/OEP/Dapi/makeTransfer_2.png)

![Alt text](img/OEP/Dapi/makeTransfer_3.png)

![Alt text](img/OEP/Dapi/makeTransfer_4.png)

![Alt text](img/OEP/Dapi/makeTransfer_5.png)

转账失败。

### Smart Contract

#### Smart Contract Call

![Alt text](img/OEP/Dapi/CallSc_1.png)

![Alt text](img/OEP/Dapi/CallSc_2.png)

![Alt text](img/OEP/Dapi/CallSc_3.png)

![Alt text](img/OEP/Dapi/CallSc_4.png)

![Alt text](img/OEP/Dapi/CallSc_5.png)

![Alt text](img/OEP/Dapi/CallSc_6.png)

`TxHash`在区块链浏览器中未查询到。

#### Smart Contract Deploy

![Alt text](img/OEP/Dapi/DeploySC_1.png)

![Alt text](img/OEP/Dapi/DeploySC_2.png)

![Alt text](img/OEP/Dapi/DeploySC_3.png)

![Alt text](img/OEP/Dapi/DeploySC_4.png)

`TxHash`在区块链浏览器中未查询到。