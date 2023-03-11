# 应用示例

    本文通过一个简单的交易所提现功能的开发示例，包括开发环境的准备和BFMeta的部署，使开发者能够轻松掌握如何在BFMeta网络上开发自己的DApp

# 开发准备
    本文的开发示例使用TypeScript进行开发，为了帮助开发者迅速上手，开发之前需要准备一些必备工具。

#### 1.Nodejs v16+
- 从Nodejs官网(https://nodejs.org/)选择版本进行安装
- 查看Nodejs版本
node -v
v16.13.2

#### 2.TypeScript 4.6+
- 安装Typescript
npm install -g typescript

- 查看Typescript版本
tsc -v

#### 3.Vscode
- 官网(https://code.visualstudio.com/)下载最新版本的Vscode

# 节点的安装和部署
    开发人员可以通过BFChain官网(http://bfmeta.org)下载节点的安装程序，进行安装完成之后启动本地节点。

# 具体示例
## 需求说明

在主流的交易所中，用户充值提现是一个很常见的功能。
它包括以下几个功能：

- 导入SDK
- 查询账户余额
- 用户提现（转账）

功能展示如图：
![交易所提现](./media/withdraw.png)

## 账户的DB结构

###Step1:初始化SDK

- 详见上一章sdk导入

```ts

import { bfmetaSDK } from "../bfmetaSDKInit";

export class BFMetaUtil {
    public bfmetaSDK: BFMetaSDK = bfmetaSDK;
    constructor() { 

    /**
    *   获取本地节点当前最新区块
    */
    async getLastBlock() {
        return this.bfmetaSDK.api.basic.getLastBlock();
    }

    /**
    *   获得账户余额
    *   @param:address:账户地址
    *   @param:assetType:资产类型
    */
    async getAddressBalance(address: string, assetType?: string) {
        if (!assetType) assetType = "BFM";
        const accountAsset = await this.bfmetaSDK.api.basic.getAccountLastTransaction({
            address: address,
            assetType: assetType
        })
        if (accountAsset && accountAsset.success === true) {
            return this.getAssetBalance(address, accountAsset.result.transactionInBlock)
        } else {
            return "0";
        }
    }

    async getAssetBalance(address: string, tr) {
        const assetChange = tr.transactionAssetChanges;
        if (assetChange && assetChange.length > 0) {
            const isSender = address === tr.transaction.senderId;
            const isRecipient = address === tr.transaction.recipientId;
            let assetType = "";
            if(tr.transaction && tr.transaction.storageKey === "assetType" && tr.transaction.storageKey){
                assetType = tr.transaction.storageKey
            };
    
            for(const change of assetChange){
                if((isSender && change.accountType ===0) || (isRecipient && change.accountType === 1)){
                    if(!assetType){
                        return change.assetBalance;
                    }
                    if(assetType !== "BFM" && change.assetTypes === 0){
                        return change.assetBalance;
                    }
                }
            }
        }
        return "0"
    }

    get bfmetaSDK() {
        return this.bfmetaSDK;
    }
}

```

###Step2: 初始化数据结构
为了简化数据模型，在这个实例中，我们只使用Account和Transaction这2个数据模型。以MongoDB为例。

- 账户数据模型(Account)

```ts
import { Schema } from "mongoose"

const accountSchema = new Schema({
    accountId: { type: String, require: true },     // 账户ID
    address: { type: String, require: true },       // 账户地址
    coin: { type: String, require: true },          // 币种
    balance: { type: String, require: true }        // 余额
});

export const account = mongoose.model("account", accountSchema)
```
- 交易数据模型(Transaction)

```ts
const transactionSchema = new Schema({
    tranId: { type: String, require: true },         //  交易ID
    amount: { type: Number, require: true },         //  交易金额
    tranType: { type: String, require: true },       //  交易类型
    senderId: { type: String, require: true },       //  发送者ID
    recipientId: { type: String, require: true },    //  接受者ID
    signature: { type: String, require: true }       //  交易签名
});

export const transaction = mongoose.model("transaction", transactionSchema)
```

###Step3: 生成交易
```ts
// 项目方本身需要具有一定的业务代码保证自身项目能够正常运行
// 在本sdk中，仅仅提供交易的生成及广播，区块、交易查询的接口。
// 项目方需要使用这些交易去拓展自身业务的使用场景。
// 例如提现功能在链上相当于权益转移交易。

```
