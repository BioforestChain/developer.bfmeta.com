# BFMeta API 接口文档

## 1.简介

欢迎使用 BFMeta API 官方文档。
BFMeta 区块链浏览器是 BFMeta 官方提供给用户用来搜索，分析链上数据，调用 API 的平台。
作为对区块链数据访问的一种方式，我们开发了 BFMeta Develop API，使开发人员能够通过 GET/POST 请求直接访问 BFMeta 区块链浏览器的数据和服务。

## 2 API 接口说明

本章主要用来说明 BFMeta 浏览器提供的各个接口的主要用途、输入输出参数以及调用方式，成功示例以及错误示例。
目前 API 接口仅支持 http 调用。
### 2.1 endpoint


| 链  | 网络    | 网址                         |
|:----|:--------|------------------------------|
| BFM | mainnet | https://tracker.bfmeta.org   |
| BFM | testnet | https://qatracker.bfmeta.org |
### 2.2 接口传入参数说明

1.  必填/可选：
    必填表示该命令执行时必须输入参数，缺少该参数则无法正常调用。
    可选表示该命令执行时的可选参数，可以根据实际情况选择是否填写。

### 2.3 接口说明

#### 获取账户资产列表

接口路径: /browser/api/accountAssets
接口用途：用来获取地址账户所拥有的的可用权益数量和冻结权益数量
接口方法：GET
接口示例：https://tracker.bfmeta.org/browser/api/accountAssets?address=c6trocwCp3dtHduZmMGPZA7tib4eWL4Wva&page=1&pageSize=10

##### 请求参数

| 参数名称 | 参数含义              | 参数类型 | 是否必须 |
|----------|-----------------------|----------|----------|
| address  | 指定地址              | string   | 必须     |
| page     | 页码，默认 1          | number   | 非必须   |
| pageSize | 每页查询件数，默认 15 | number   | 非必须   |

##### 返回数据示例

###### 成功示例

```json
{
    // 是否成功；ture 成功， false失败
    "success": true,
    // 数据明细
    "data": {
        // 页码
        "page": 1,
        // 每页查询件数
        "pageSize": 15,
        // 数据列表，数组格式
        "dataList": [
            {
                // 权益名
                "assetType": "BFMTEST",
                // 资产未冻结数量
                "assetNumber": "10150087043",
                // 资产冻结数量
                "frozenAssetNumber": "173430030299",
                // 资产总数量(冻结数量+未冻结数量)
                "totalNumber": "183580117342"
            }
        ],
        // 查询总件数
        "total": 1,
        // 是否还有查询数据，true：不是最后一页数据，还能继续查询 false：已经查询到最后一页
        "hasMore": false
    }
}
```

###### 失败示例

```json
{
    "success": false,
    "error": {
        // 错误码
        "code": 1001,
        // 错误信息
        "message": "input address is wrong"
    }
}
```

#### 获取地址账户资产详情

接口路径: /browser/api/accountsAssetDetail
接口用途：获取地址账户资产详情
接口方法：GET
接口示例：https://tracker.bfmeta.org/browser/api/accountsAssetDetail?address=cCVQvZBVbaeCfLKR4FQzzVhnmmQa5Ndrsm

##### 请求参数

| 参数名称 | 参数含义 | 参数类型 | 是否必须 |
|----------|----------|----------|----------|
| address  | 指定地址 | string   | 必须     |

##### 返回数据示例

###### 成功示例

```json
{
    "success": true,
    "data": {
        // 地址
        "address": "cCVQvZBVbaeCfLKR4FQzzVhnmmQa5Ndrsm",
        // 资产
        "assets": {
            // 资产来源网络标识符(magic)
            "PPPPW": {
                // 拥有的权益名
                "BFMTEST": {
                    // 资产来源网络标识符
                    "sourceChainMagic": "PPPPW",
                    // 权益名
                    "assetType": "BFMTEST",
                    // 资产来源链名
                    "sourceChainName": "bfmetatest",
                    // 权益数量
                    "assetNumber": "10151728947",
                    // 上一轮的余额信息
                    "lastRoundInfo": {
                        // 上一轮轮次
                        "round": 130,
                        // 上一轮轮末账户持有的主权益数量
                        "assetNumber": "10150820565",
                        // 上一轮账户的事件量
                        "txCount": 1
                    },
                    // 上上轮轮末的权益量和事件量
                    "penultimateRoundInfo": {
                        // 上上一轮轮次
                        "round": 129,
                        // 上上一轮轮末账户持有的主权益数量
                        "assetNumber": "10150823912",
                        // 上上一轮账户的事件量
                        "txCount": 1
                    }
                }
            }
        },
        // 累计打块收益
        "forgingRewards": "0",
        // 区块高度
        "height": 7411,
        // 累计花费手续费
        "paidFee": "697753",
        // 账户公钥
        "publicKey": "539681abb4336995cd868feefa711029116c7287e165d5d1f0b9a874658e2bd6",
        // 累计投票收益
        "votingRewards": "152426700"
    }
}
```

###### 失败示例

```json
{
    "success": false,
    "error": {
        "code": 9002,
        "message": "parameter error",
        "details": [
            {
                "location": "params",
                "param": "address",
                "msg": "address must be input"
            }
        ]
    }
}
```

#### 获取账户资产余额

接口路径: /browser/api/accountBalance
接口用途：获取账户地址的资产余额
接口方法：GET
接口示例：https://tracker.bfmeta.org/browser/api/accountBalance?magic=PPPPW&assetType=BBBBR&address=c6trocwCp3dtHduZmMGPZA7tib4eWL4Wva

##### 请求参数

| 参数名称  | 参数含义   | 参数类型 | 是否必须 |
|-----------|------------|----------|----------|
| magic     | 网络标识符 | string   | 必须     |
| assetType | 权益名     | string   | 必须     |
| address   | 指定地址   | string   | 必须     |

##### 返回数据示例

###### 成功示例

```json
{
    "success": true,
    // 资产余额
    "data": "10150823912"
}
```

###### 失败示例

```json
{
    "success": false,
    "error": {
        // 错误码
        "code": 1001,
        // 错误信息
        "message": "input address is wrong"
    }
}
```

#### 获取上一轮区块所有交易平均手续费

接口路径: /browser/api/blockAverageFee
接口用途：获取上一轮区块所有交易平均手续费
接口方法：GET
接口示例：https://tracker.bfmeta.org/browser/api/blockAverageFee

##### 请求参数

| 参数名称 | 参数含义 | 参数类型 | 是否必须 |
|----------|----------|----------|----------|
| 无       |          |          |          |

##### 返回数据示例

###### 成功示例

```json
{
    "success": true,
    // 上一轮区块中所有交易的平均手续费
    // 单位：本；1 BFM = 100000000本
    // 例如上一轮总共有57个区块，假设每个区块都是10笔交易，每笔交易的手续费分别为c0,c1, ..... c9
    // 计算方式如下： (c0 + c1 + ... + c9) * 57 / (57 * 10)
    "data": "704846"
}
```

#### 获取资产列表

接口路径: /browser/api/assets
接口用途：获取资产列表
接口方法：GET
接口示例：https://tracker.bfmeta.org/browser/api/assets?assetType=BBB&page=1&pageSize=10

##### 请求参数

| 参数名称  | 参数含义              | 参数类型 | 是否必须 |
|-----------|-----------------------|----------|----------|
| assetType | 权益名（可模糊查询）  | string   | 非必须   |
| page      | 页码，默认 1          | number   | 非必须   |
| pageSize  | 每页查询件数，默认 10 | number   | 非必须   |

##### 返回数据示例

###### 成功示例

```json
{
    "success": true,
    "data": {
        // 页码
        "page": 1,
        // 每页查询件数
        "pageSize": 10,
        // 数据列表，数组格式
        "dataList": [
            {
                // 权益名
                "assetType": "BBBBR",
                // 资产来源网络标识符
                "sourceChainMagic": "PPPPW",
                // 申请发行资产账户的地址
                "applyAddress": "cNyqY5uFERYnTLPJRkrYg3MHka3NoLQ1fy",
                // 冻结的主资产数量
                "frozenMainAssetPrealnum": "10000000000",
                // 资产的创世账户地址
                "genesisAddress": "cH2ZM4rgJHYoRvoqaz8YJJCrFHU8oavRVw",
                // 区块高度
                "height": 7346,
                // 发行的资产数量
                "issuedAssetPrealnum": "10000000000000",
                // 销毁后剩余的资产数量
                "remainAssetPrealnum": "9999999850000",
                // 资产来源链名
                "sourceChainName": "bfmetatest"
            },
            {
                "assetType": "BBBCQ",
                "sourceChainMagic": "PPPPW",
                "applyAddress": "cGEG52jKstuiXn3UdNAiN7yYebChTNoymL",
                "frozenMainAssetPrealnum": "10000000000",
                "genesisAddress": "cDBZriP4ie5CdssGGiXwnMoUjuWeMeduQE",
                "height": 7409,
                "issuedAssetPrealnum": "10000000000000",
                "remainAssetPrealnum": "9999999820000",
                "sourceChainName": "bfmetatest"
            }
            //  ..... 后面数据省略
        ],
        // 查询总件数
        "total": 14,
        // 是否还有查询数据，true：不是最后一页数据，还能继续查询 false：已经查询到最后一页
        "hasMore": true
    }
}
```

###### 失败示例

```json
{
    "success": false,
    "error": {
        "code": 9002,
        "message": "parameter error",
        "details": [
            {
                "location": "query",
                "param": "page",
                "msg": "page must be positive integer",
                "value": "test"
            }
        ]
    }
}
```

#### 获取资产明细

接口路径: /browser/api/assetDetails
接口用途：获取资产明细
接口方法：GET
接口示例：https://tracker.bfmeta.org/browser/api/assetDetails?assetType=BBBBR

##### 请求参数

| 参数名称  | 参数含义 | 参数类型 | 是否必须 |
|-----------|----------|----------|----------|
| assetType | 权益名   | string   | 必须     |

##### 返回数据示例

###### 成功示例

```json
{
    "success": true,
    "data": {
        // 权益名
        "assetType": "BBBBR",
        // 资产来源网络标识符
        "sourceChainMagic": "PPPPW",
        // 申请发行资产账户的地址
        "applyAddress": "cNyqY5uFERYnTLPJRkrYg3MHka3NoLQ1fy",
        // 冻结的主资产数量
        "frozenMainAssetPrealnum": "10000000000",
        // 资产的创世账户地址
        "genesisAddress": "cH2ZM4rgJHYoRvoqaz8YJJCrFHU8oavRVw",
        // 区块高度
        "height": 7346,
        // 发行的资产数量
        "issuedAssetPrealnum": "10000000000000",
        // 销毁后剩余的资产数量
        "remainAssetPrealnum": "9999999850000",
        // 资产来源链名
        "sourceChainName": "bfmetatest"
    }
}
```

###### 失败示例

```json
{
    "success": false,
    "error": {
        "code": 9002,
        "message": "parameter error",
        "details": [
            {
                "location": "params",
                "param": "assetType",
                "msg": "assetType must be input"
            }
        ]
    }
}
```

#### 获取账户信息

接口路径: /browser/api/accountsInfo
接口用途：获取账户信息
接口方法：GET
接口示例：https://tracker.bfmeta.org/browser/api/accountsInfo?address=cCVQvZBVbaeCfLKR4FQzzVhnmmQa5Ndrsm

##### 请求参数

| 参数名称 | 参数含义 | 参数类型 | 是否必须 |
|----------|----------|----------|----------|
| address  | 指定地址 | string   | 必须     |

##### 返回数据示例

###### 成功示例

```json
{
    "success": true,
    "data": {
        // 地址
        "address": "cCVQvZBVbaeCfLKR4FQzzVhnmmQa5Ndrsm",
        // 账户状态 0 为正常 1 为冻结状态
        "accountStatus": 0,
        // 权益信息
        "equityInfo": {
            // 轮次
            "round": 129,
            // 权益剩余值
            "equity": "50750418305329",
            // 权益初始值
            "fixedEquity": "50750426680001"
        },
        // 区块高度
        "height": 7408,
        // 是否接收投票 false 为不接收投票 true 为接收投票
        "isAcceptVote": false,
        // 是否为受托人 false 为普通账户 true 为受托人
        "isDelegate": false,
        // 上一轮的余额信息
        "lastRoundInfo": {
            // 上一轮轮次
            "round": 129,
            // 上一轮轮末账户持有的主权益数量
            "assetNumber": "10150820565",
            // 上一轮账户的事件量
            "txCount": 1
        },
        // 掉块数量，被选为受托人并且到达锻造区块的时间却因为各种原因没有锻造出相应的区块的次数
        "missedblocks": 0,
        // 已锻造区块数
        "producedblocks": 0,
        // 账户公钥
        "publicKey": "539681abb4336995cd868feefa711029116c7287e165d5d1f0b9a874658e2bd6",
        // 账户二次公钥
        "secondPublicKey": null,
        // 用户名
        "username": "testUser",
        // 获得投票权益信息
        "voteInfo": {
            // 获得投票权益所属轮次
            "round": 1,
            // 获得投票权益数量
            "vote": "1000"
        }
    }
}
```

###### 失败示例

```json
{
    "success": false,
    "error": {
        // 错误码
        "code": 1001,
        // 错误信息
        "message": "input address is wrong"
    }
}
```

#### 根据签名查询事件详情

接口路径: /browser/api/transaction/signature
接口用途：根据签名查询事件详情
接口方法：GET
接口示例：https://tracker.bfmeta.org/browser/api/transaction/signature?signature=b5883dc9922c27af85faa3251fb76df8ea307381975e5f75559337a4c64789ef04ab9dc714e70f2f3fa479b6081cd937b65225e5446efcf07b8855afa3ca5900&height=100

##### 请求参数

| 参数名称  | 参数含义                                             | 参数类型 | 是否必须 |
|-----------|------------------------------------------------------|----------|----------|
| signature | 签名                                                 | string   | 必须     |
| height    | 区块高度（这个区块高度是用来查询交易的最低区块高度） | number   | 非必须   |

##### 返回数据示例

###### 成功示例

```json
{
    "success": true,
    "data": {
        // 事件的块中索引
        "tIndex": 6687685,
        // 区块高度
        "height": 7441,
        // 事件涉及的账户权益变动信息
        "transactionAssetChanges": [
          {
            // 账户状态 0 发起账户 1 为接收账户
            "accountType": 0,
            // 资产来源网络标识符
            "sourceChainMagic": "PPPPW",
            // 权益名称
            "assetType": "BFMTEST",
            // 权益数量
            "assetPrealnum": "169734806611"
          }
        ],
        // 区块签名
        "signature": "ad3c2a0fa323020f6ba8c8d1fbf5900928bb380f98dbc1bee242bf96ca47dd77097a45ad9446eb55459980085de5f5bfbcb37f919e02a4289165f58456268603",
        // 交易体
        "transaction": {
          // 版本号
          "version": 1,
          // 交易类型
          "type": "BFMTEST-BFMETATEST-AST-03",
          // 交易发送者地址
          "senderId": "c6trocwCp3dtHduZmMGPZA7tib4eWL4Wva",
          // 交易发送者公钥
          "senderPublicKey": "ff30aef7c4c3fa1c0390f00cfdcae536a2e1583e66bf1b4a8b9f897e6e99dd04",
          // 交易手续费
          "fee": "4000400",
          // 交易时间戳
          "timestamp": 10523197,
          // 交易发起高度
          "applyBlockHeight": 7440,
          // 有效区块高度
          "effectiveBlockHeight": 7497,
          // 事件签名
          "signature": "b5883dc9922c27af85faa3251fb76df8ea307381975e5f75559337a4c64789ef04ab9dc714e70f2f3fa479b6081cd937b65225e5446efcf07b8855afa3ca5900",
          // 交易的业务处理属性（根据交易类型不同，数据格式也不一样，具体可以参考开发者社区）
          "asset": {
            "giftAsset": {
              // 加密密钥生成的公钥数组
              "cipherPublicKeys": [],
              // 资产来源网络标识符
              "sourceChainMagic": "PPPPW",
              // 资产来源链名
              "sourceChainName": "bfmetatest",
              // 权益名
              "assetType": "BFMTEST",
              // 数量
              "amount": "10000000",
              // 从资产赠送交易发起到开始被签收的区块间隔
              "totalGrabableTimes": 1,
              // 接收规则
              "giftDistributionRule": 0
            }
          },
          // 交易 POW 的随机数
          "nonce": 0,
          // 交易的接收范围类型
          "rangeType": 0,
          // 交易的接收范围
          "range": [],
          // 交易的来源链网络标识符
          "fromMagic": "",
          // 交易的来源链网络标识符
          "toMagic": "",
          // 交易的备注信息
          "remark": {},
          // 交易的索引键
          "storageKey": "assetType",
          // 交易的索引值
          "storageValue": "BFMTEST"
        }
      },
    }
}
```

###### 失败示例

```json
{
    "success": false,
    "error": {
        "code": 9002,
        "message": "parameter error",
        "details": [
            {
                "location": "params",
                "param": "signature",
                "msg": "signature must be input"
            }
        ]
    }
}
```

#### 查询事件

接口路径: /browser/api/transaction
接口用途：查询事件
接口方法：POST
接口示例：https://tracker.bfmeta.org/browser/api/transaction
Body 数据示例：
{
"address": "c6trocwCp3dtHduZmMGPZA7tib4eWL4Wva",
"sort": -1
}

##### 请求参数

| 参数名称     | 参数含义                          | 参数类型 | 是否必须 |
|--------------|-----------------------------------|----------|----------|
| address      | 地址                              | string   | 非必须   |
| height       | 区块高度                          | number   | 非必须   |
| minHeight    | 最小区块高度(查询起始高度)        | number   | 非必须   |
| maxHeight    | 最大区块高度(查询终止高度)        | number   | 非必须   |
| senderId     | 发送者地址                        | string   | 非必须   |
| recipientId  | 接收者地址                        | string   | 非必须   |
| type         | 事件类型                          | string[] | 非必须   |
| page         | 页码，默认 1                      | number   | 非必须   |
| pageSize     | 每页查询件数，默认 10             | number   | 非必须   |
| sort         | 排序， 1:正序 Asc， -1：逆序 Desc | number   | 非必须   |
| storageValue | 查询用的索引存储的键值            | string   | 非必须   |

<** 备注 **>
为了提高查询效率，请求参数中必须输入某个地址参数（可在 address，senderId，recipientId 中任选一个参数作为地址参数查询）

##### 返回数据示例

###### 成功示例

```json
{
    "success": true,
    "data": {
        // 页码
        "page": 1,
        // 每页查询件数
        "pageSize": 10,
        // 数据列表，数组格式
        "dataList": [
            {
                // 事件的块中索引
                "tIndex": 6687685,
                // 区块高度
                "height": 7441,
                // 事件涉及的账户权益变动信息
                "transactionAssetChanges": [
                    {
                        // 账户状态 0 发起账户 1 为接收账户
                        "accountType": 0,
                        // 资产来源网络标识符
                        "sourceChainMagic": "PPPPW",
                        // 权益名
                        "assetType": "BFMTEST",
                        // 权益数量
                        "assetPrealnum": "169734806611"
                    }
                ],
                // 区块签名
                "signature": "ad3c2a0fa323020f6ba8c8d1fbf5900928bb380f98dbc1bee242bf96ca47dd77097a45ad9446eb55459980085de5f5bfbcb37f919e02a4289165f58456268603",
                // 交易体
                "transaction": {
                    // 版本号
                    "version": 1,
                    // 事件类型
                    "type": "BFMTEST-BFMETATEST-AST-03",
                    // 交易发送者地址
                    "senderId": "c6trocwCp3dtHduZmMGPZA7tib4eWL4Wva",
                    // 交易发送者公钥
                    "senderPublicKey": "ff30aef7c4c3fa1c0390f00cfdcae536a2e1583e66bf1b4a8b9f897e6e99dd04",
                    // 交易手续费
                    "fee": "4000400",
                    // 交易时间戳
                    "timestamp": 10523197,
                    // 交易发起高度
                    "applyBlockHeight": 7440,
                    // 有效区块高度
                    "effectiveBlockHeight": 7497,
                    // 事件签名
                    "signature": "b5883dc9922c27af85faa3251fb76df8ea307381975e5f75559337a4c64789ef04ab9dc714e70f2f3fa479b6081cd937b65225e5446efcf07b8855afa3ca5900",
                    // 交易的业务处理属性（根据交易类型不同，数据格式也不一样，具体可以参考开发者社区）
                    "asset": {
                        "giftAsset": {
                            // 加密密钥生成的公钥数组
                            "cipherPublicKeys": [],
                            // 资产来源网络标识符
                            "sourceChainMagic": "PPPPW",
                            // 资产来源链名
                            "sourceChainName": "bfmetatest",
                            // 权益名
                            "assetType": "BFMTEST",
                            // 数量
                            "amount": "10000000",
                            // 从资产赠送交易发起到开始被签收的区块间隔
                            "totalGrabableTimes": 1,
                            // 接收规则
                            "giftDistributionRule": 0
                        }
                    },
                    // 交易 POW 的随机数
                    "nonce": 0,
                    // 交易的接收范围类型
                    "rangeType": 0,
                    // 交易的接收范围
                    "range": [],
                    // 交易的来源链网络标识符
                    "fromMagic": "",
                    // 交易的来源链网络标识符
                    "toMagic": "",
                    // 交易的备注信息
                    "remark": {},
                    // 交易的索引键
                    "storageKey": "assetType",
                    // 交易的索引值
                    "storageValue": "BFMTEST"
                }
            }
            //  ..... 后面数据省略
        ],
        // 查询总件数
        "total": 102,
        // 是否还有查询数据，true：不是最后一页数据，还能继续查询 false：已经查询到最后一页
        "hasMore": true
    }
}
```

###### 失败示例

```json
{
    "success": false,
    "error": {
        "code": 1014,
        "message": "address or senderId or recipientId must be input"
    }
}
```

#### 获取区块详情

接口路径: /browser/api/block/height
接口用途：获取区块详情
接口方法：GET
接口示例：https://tracker.bfmeta.org/browser/api/block/height?height=10

##### 请求参数

| 参数名称 | 参数含义 | 参数类型 | 是否必须 |
|----------|----------|----------|----------|
| height   | 区块高度 | number   | 必须     |

##### 返回数据示例

###### 成功示例

```json
{
    "success": true,
    "data": {
        // 版本号
        "version": 1,
        // 区块高度
        "height": 7463,
        // 区块大小
        "blockSize": 168151,
        // 锻造者公钥
        "generatorPublicKey": "20141cef3e129cec822726a84b79f617370554a53636acf5ad1dbd15e22c15d6",
        // 锻造者的安全公钥
        "generatorSecondPublicKey": null,
        // 锻造者权益
        "generatorEquity": "69037762057352857",
        // 前块签名
        "previousBlockSignature": "017c06ddd6df1d347b4de3f484b8cf9b6c1c97461e53e4888f5223d8ff054f7e10bf24b2af021a8a19728b2f7418cec264cd4f03965107d63eb4884c376fbb01",
        // 区块时间戳
        "timestamp": 10524800,
        // 区块奖励值
        "reward": "4000000000",
        // 区块的链标识符
        "magic": "PPPPW",
        // 区块备注信息
        "remark": {
            "info": "the net version is testnet, only running for the test",
            "debug": "BFMTEST_linux_v3.7.1_P16_DP1_T2514_C113_A22.25 UNTRS_B107_E535_TIME47 LOST 78"
        },
        // 区块附加信息
        "asset": {},
        // 区块事件信息
        "transactionInfo": {
            // 区块事件起始索引
            "startTindex": 6696401,
            // 区块事件数量
            "numberOfTransactions": 310,
            // 区块处理的交易 hash
            "payloadHash": "5e51db6039c5529914bae1ff4c4ca73f563ac631ea0e8e8a01c7fcc55d1d666b",
            // 区块处理的交易总字节数
            "payloadLength": 160074,
            // blob大小
            "blobSize": 0,
            // 区块处理的总资产数量
            "totalAmount": "1828418739",
            // 区块处理的总手续费数量
            "totalFee": "306884578",
            // 区块事件
            "transactionInBlocks": [],
            // 统计信息
            "statisticInfo": {
                // 本块处理的总手续费数量
                "totalFee": "306884578",
                // 本块处理的总资产数量
                "totalAsset": "1828418739",
                // 本块处理的总链资产数量
                "totalChainAsset": "1816884739",
                // 本块处理的总涉及的账户数量
                "totalAccount": 369,
                // 本块涉及的某个链的资产详情
                "magicAssetTypeTypeStatisticHashMap": {
                    // 权益名
                    "PPPPW": {
                        // 本块涉及本链的资产详情
                        "assetTypeTypeStatisticHashMap": {
                            // 权益名
                            "BBJXS": {
                                // 资产详情
                                "typeStatisticHashMap": {
                                    // 交易类型
                                    "AST-02": {
                                        // 变动总值，某个账户的增加或者减少都会有影响
                                        "changeAmount": "20000",
                                        // 变动总次数，某个账户的增加或者减少都会有影响
                                        "changeCount": 2,
                                        // 资产迁移总量
                                        "moveAmount": "10000",
                                        // 交易次数
                                        "transactionCount": 0
                                    }
                                },
                                "total": {
                                    // 变动总值，某个账户的增加或者减少都会有影响
                                    "changeAmount": "20000",
                                    // 变动总次数，某个账户的增加或者减少都会有影响
                                    "changeCount": 2,
                                    // 资产迁移总量
                                    "moveAmount": "10000",
                                    // 交易次数
                                    "transactionCount": 0
                                }
                            }
                        }
                    }
                }
            }
        },
        // 锻造者掉线列表
        "roundOfflineGeneratersHashMap": {},
        // 区块参与度
        "blockParticipation": "9084423695310",
        // 区块签名
        "signature": "684ed5d2ba9a37a8666dfb612af9c3a432f82d386943ec0cb91a24ca1a5298fb4e51b1a323ad12624a9b5eb7ff31335c5175e84f442d6ba216b8ed7d7bc85901",
        // 区块安全签名
        "signSignature": null
    }
}
```

###### 失败示例

```json
{
    "success": false,
    "error": {
        "code": 9002,
        "message": "parameter error",
        "details": [
            {
                "location": "params",
                "param": "height",
                "msg": "height must be input"
            }
        ]
    }
}
```

#### 获取区块列表

接口路径: /browser/api/block
接口用途：获取区块列表
接口方法：GET
接口示例：https://tracker.bfmeta.org/browser/api/block?sort=-1&page=1&pageSize=10

##### 请求参数

| 参数名称 | 参数含义                          | 参数类型 | 是否必须 |
|----------|-----------------------------------|----------|----------|
| sort     | 排序， 1:正序 Asc， -1：逆序 Desc | number   | 非必须   |
| page     | 页码，默认 1                      | number   | 非必须   |
| pageSize | 每页查询件数，默认 10             | number   | 非必须   |

##### 返回数据示例

###### 成功示例

```json
{
    "success": true,
    "data": {
        "page": 1,
        "pageSize": 10,
        "dataList": [
            {
                // 版本号
                "version": 1,
                // 区块高度
                "height": 7463,
                // 区块大小
                "blockSize": 168151,
                // 锻造者公钥
                "generatorPublicKey": "20141cef3e129cec822726a84b79f617370554a53636acf5ad1dbd15e22c15d6",
                // 锻造者的安全公钥
                "generatorSecondPublicKey": null,
                // 锻造者权益
                "generatorEquity": "69037762057352857",
                // 前块签名
                "previousBlockSignature": "017c06ddd6df1d347b4de3f484b8cf9b6c1c97461e53e4888f5223d8ff054f7e10bf24b2af021a8a19728b2f7418cec264cd4f03965107d63eb4884c376fbb01",
                // 区块时间戳
                "timestamp": 10524800,
                // 区块奖励值
                "reward": "4000000000",
                // 区块的链标识符
                "magic": "PPPPW",
                // 区块备注信息
                "remark": {
                    "info": "the net version is testnet, only running for the test",
                    "debug": "BFMTEST_linux_v3.7.1_P16_DP1_T2514_C113_A22.25 UNTRS_B107_E535_TIME47 LOST 78"
                },
                // 区块附加信息
                "asset": {},
                // 区块事件信息
                "transactionInfo": {
                    // 区块事件起始索引
                    "startTindex": 6696401,
                    // 区块事件数量
                    "numberOfTransactions": 310,
                    // 区块处理的交易 hash
                    "payloadHash": "5e51db6039c5529914bae1ff4c4ca73f563ac631ea0e8e8a01c7fcc55d1d666b",
                    // 区块处理的交易总字节数
                    "payloadLength": 160074,
                    // blob大小
                    "blobSize": 0,
                    // 区块处理的总资产数量
                    "totalAmount": "1828418739",
                    // 区块处理的总手续费数量
                    "totalFee": "306884578",
                    // 区块事件
                    "transactionInBlocks": [],
                    // 统计信息
                    "statisticInfo": {
                        // 本块处理的总手续费数量
                        "totalFee": "306884578",
                        // 本块处理的总资产数量
                        "totalAsset": "1828418739",
                        // 本块处理的总链资产数量
                        "totalChainAsset": "1816884739",
                        // 本块处理的总涉及的账户数量
                        "totalAccount": 369,
                        // 本块涉及的某个链的资产详情
                        "magicAssetTypeTypeStatisticHashMap": {
                            // 权益名
                            "PPPPW": {
                                // 本块涉及本链的资产详情
                                "assetTypeTypeStatisticHashMap": {
                                    // 权益名
                                    "BBJXS": {
                                        // 资产详情
                                        "typeStatisticHashMap": {
                                            // 交易类型
                                            "AST-02": {
                                                // 变动总值，某个账户的增加或者减少都会有影响
                                                "changeAmount": "20000",
                                                // 变动总次数，某个账户的增加或者减少都会有影响
                                                "changeCount": 2,
                                                // 资产迁移总量
                                                "moveAmount": "10000",
                                                // 交易次数
                                                "transactionCount": 0
                                            }
                                        },
                                        "total": {
                                            // 变动总值，某个账户的增加或者减少都会有影响
                                            "changeAmount": "20000",
                                            // 变动总次数，某个账户的增加或者减少都会有影响
                                            "changeCount": 2,
                                            // 资产迁移总量
                                            "moveAmount": "10000",
                                            // 交易次数
                                            "transactionCount": 0
                                        }
                                    }
                                }
                            }
                        }
                    }
                },
                // 锻造者掉线列表
                "roundOfflineGeneratersHashMap": {},
                // 区块参与度
                "blockParticipation": "9084423695310",
                // 区块签名
                "signature": "684ed5d2ba9a37a8666dfb612af9c3a432f82d386943ec0cb91a24ca1a5298fb4e51b1a323ad12624a9b5eb7ff31335c5175e84f442d6ba216b8ed7d7bc85901",
                // 区块安全签名
                "signSignature": null
            }
            // ......后面数据省略
        ],
        // 查询总件数
        "total": 5005,
        // 是否还有查询数据，true：不是最后一页数据，还能继续查询 false：已经查询到最后一页
        "hasMore": true
    }
}
```

###### 失败示例

```json
{
    "success": false,
    "error": {
        "code": 1007,
        "message": "sort must be 1 or -1"
    }
}
```
