# 基础接口
## 1.获得节点版本号
- 接口全称：`nodeVersion`
- 接口简写：`v`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`get`
- 接口`url`地址：`/api/basic/nodeVersion`
- 请求参数：无
- 返回参数：
```typescript
interface NodeVersion {
    /**是否成功 */
    success: boolean;
    result: {
        /**当前节点的版本号 */
        version: string;
        hash?: string;
    };
}

```
## 2.获取事件类型
- 接口全称：`getTransactionType`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/basic/getTransactionType`
- 请求参数：
```typescript
interface GetTransactionType {
    /**事件基础类型 */
    baseType: {
        //二次密码
        SIGNATURE = "BSE-01",
        //注册锻造者
        DELEGATE = "BSE-02",
        //治理投票
        VOTE = "BSE-03",
        //设置用户名
        USERNAME = "BSE-04",
        //开始收票
        ACCEPT_VOTE = "BSE-05",
        //停止收票
        REJECT_VOTE = "BSE-06",
        //创建DAPPID
        DAPP = "WOD-00",
        //DAPPID付费
        DAPP_PURCHASING = "WOD-01",
        //注册新链
        REGISTER_CHAIN = "WOD-02",
        //数据存证
        MARK = "EXT-00",
        //创建权益
        ISSUE_ASSET = "AST-00",
        //权益转移
        TRANSFER_ASSET = "AST-01",
        //销毁权益
        DESTORY_ASSET = "AST-02",
        //发起权益赠送
        GIFT_ASSET = "AST-03",
        //接受权益赠送
        GRAB_ASSET = "AST-04",
        //发起权益委托
        TRUST_ASSET = "AST-05",
        //签收资产委托
        SIGN_FOR_ASSET = "AST-06",
        //权益迁出
        EMIGRATE_ASSET = "AST-07",
        //权益迁入
        IMMIGRATE_ASSET = "AST-08",
        //发起权益交换
        TO_EXCHANGE_ASSET = "AST-09",
        //接受权益交换
        BE_EXCHANGE_ASSET = "AST-10",
        //发起资产交换
        TO_EXCHANGE_SPECIAL_ASSET = "AST-11",
        //接受资产交换
        BE_EXCHANGE_SPECIAL_ASSET = "AST-12",
        //注册/注销链域名
        LOCATION_NAME = "LNS-00",
        //设置链域名解析值
        SET_LNS_RECORD_VALUE = "LNS-01",
        //设置链域名管理员
        SET_LNS_MANAGER = "LNS-02",
        //个性事件
        CUSTOM = "CUS-00",
    };
}

```
- 返回参数：
```typescript
interface GetTransactionType {
    /**是否成功 */
    success: boolean;
    result: {
        /**事件类型全称 */
        type: string;
    };
}

```
## 3.获取本地节点当前最新区块
- 接口全称：`getLastBlock`
- 接口简写：`glb`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`get`
- 接口`url`地址：`/api/basic/getLastBlock`
- 请求参数：无
- 返回参数：
```typescript
interface GetLastBlock {
    /**是否成功 */
    success: boolean;
    result: {
        /**区块版本号 */
        version: number;
        /**区块高度 */
        height: number;
        /**区块时间戳 */
        timestamp: number;
        /**区块大小 */
        blockSize: number;
        /**锻造者公钥 */
        generatorPublicKey: string;
        /**锻造者的安全公钥 */
        generatorSecondPublicKey?: string;
        /**锻造者权益 */
        generatorEquity: string;
        /**前块签名 */
        previousBlockSignature: string;
        /**区块奖励值 */
        reward: string;
        /**区块的链标识符 */
        magic: string;
        /**区块参与度 */
        blockParticipation: string;
        /**区块签名 */
        signature: string;
        /**区块安全签名 */
        signSignature?: string;
        /**区块备注信息 */
        remark: {
            [key: string]: string;
        };
        /**区块事件信息 */
        transactionInfo: {
            startTindex: number;
            numberOfTransactions: number;
            payloadHash: string;
            payloadLength: number;
            totalAmount: string;
            totalFee: string;
            transactionInBlocks: [];
            statisticInfo: {
                /**区块打包的事件的总手续费 */
                totalFee: string;
                /**区块打包的事件的总权益值（不区分权益类型） */
                totalAsset: string;
                /**区块打包的事件的总主权益值 */
                totalChainAsset: string;
                /**区块打包的事件涉及的总账户数 */
                totalAccount: number;
                /**区块打包的权益统计明细，JSON 对象 */
                magicAssetTypeTypeStatisticHashMap: {
                    [magic: string]: {
                        /**区块打包的权益统计明细，JSON 对象 */
                        assetTypeTypeStatisticHashMap: {
                            [assetType: string]: {
                                /**区块打包的事件类型统计明细，JSON 对象 */
                                typeStatisticHashMap: {
                                    [baseType: string]: {
                                        /**变动总值，某个账户的增加或者减少都会有影响 */
                                        changeAmount: string;
                                        /**变动总次数，某个账户的增加或者减少都会有影响 */
                                        changeCount: number;
                                        /**资产迁移总量 */
                                        moveAmount: string;
                                        /**交易量 */
                                        transactionCount: number;
                                    };
                                };
                                /**区块打包的事件类型统计，JSON 对象 */
                                total: {
                                    /**变动总值，某个账户的增加或者减少都会有影响 */
                                    changeAmount: string;
                                    /**变动总次数，某个账户的增加或者减少都会有影响 */
                                    changeCount: number;
                                    /**资产迁移总量 */
                                    moveAmount: string;
                                    /**交易量 */
                                    transactionCount: number;
                                };
                            };
                        };
                    };
                };
            };
        };
        /**区块附加信息 */
        asset: any;
    };
}

```
## 4.获取指定区块
- 接口全称：`getBlock`
- 接口简写：`gb`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/basic/getBlock`
- 请求参数：
```typescript
interface GetBlock {
    /**区块签名 */
    signature?: string;
    /**区块高度 */
    height?: number;
    /**查看第几页（一页20条记录） */
    page?: number;
}

```
- 返回参数：
```typescript
interface GetBlock {
    /**是否成功 */
    success: boolean;
    result: {
        blocks: {
            /**区块版本号 */
            version: number;
            /**区块高度 */
            height: number;
            /**区块大小 */
            blockSize: number;
            /**区块时间戳 */
            timestamp: number;
            /**区块签名 */
            signature: string;
            /**区块安全签名 */
            signSignature?: string;
            /**锻造者公钥 */
            generatorPublicKey: string;
            /**锻造者的安全公钥 */
            generatorSecondPublicKey?: string;
            /**锻造者权益 */
            generatorEquity: string;
            /**前块签名 */
            previousBlockSignature: string;
            /**区块奖励值 */
            reward: string;
            /**区块的链标识符 */
            magic: string;
            /**区块参与度 */
            blockParticipation: string;
            /**区块备注信息 */
            remark: {
                [key: string]: string;
            };
            /**区块附加信息 */
            asset: object;
            /**锻造者掉线列表 */
            roundOfflineGeneratersHashMap: {
                /**
                 * 使用逗号分隔的地址
                 * address,address */
                [roundOffset: string]: string;
            };
            /**事件 */
            transactionInfo: {
                startTindex: number;
                numberOfTransactions: number;
                payloadHash: string;
                payloadLength: number;
                totalAmount: string;
                totalFee: string;
                transactionInBlocks: [];
                statisticInfo: {
                    /**区块打包的事件的总手续费 */
                    totalFee: string;
                    /**区块打包的事件的总权益值（不区分权益类型） */
                    totalAsset: string;
                    /**区块打包的事件的总主权益值 */
                    totalChainAsset: string;
                    /**区块打包的事件涉及的总账户数 */
                    totalAccount: number;
                    /**区块打包的权益统计明细，JSON 对象 */
                    magicAssetTypeTypeStatisticHashMap: {
                        [magic: string]: {
                            /**区块打包的权益统计明细，JSON 对象 */
                            assetTypeTypeStatisticHashMap: {
                                [assetType: string]: {
                                    /**区块打包的事件类型统计明细，JSON 对象 */
                                    typeStatisticHashMap: {
                                        [baseType: string]: {
                                            /**变动总值，某个账户的增加或者减少都会有影响 */
                                            changeAmount: string;
                                            /**变动总次数，某个账户的增加或者减少都会有影响 */
                                            changeCount: number;
                                            /**资产迁移总量 */
                                            moveAmount: string;
                                            /**交易量 */
                                            transactionCount: number;
                                        };
                                    };
                                    /**区块打包的事件类型统计，JSON 对象 */
                                    total: {
                                        /**变动总值，某个账户的增加或者减少都会有影响 */
                                        changeAmount: string;
                                        /**变动总次数，某个账户的增加或者减少都会有影响 */
                                        changeCount: number;
                                        /**资产迁移总量 */
                                        moveAmount: string;
                                        /**交易量 */
                                        transactionCount: number;
                                    };
                                };
                            };
                        };
                    };
                };
            };
        }[];
        /**符合查询条件的总数 */
        count: number;
        /**分页查询最大值 */
        cmdLimitPerQuery: number;
    };
}

```
## 5.获取指定事件
- 接口全称：`getTransactions`
- 接口简写：`gt`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/basic/getTransactions`
- 请求参数：
```typescript
interface GetTransactions {
    /**事件id */
    signature?: string;
    /**事件所属区块高度 */
    height?: number;
    /**事件所属的最低高度 */
    minHeight?: number;
    /**事件所属的最高高度 */
    maxHeight?: number;
    /**事件发起方 */
    senderId?: string;
    /**事件接收方 */
    recipientId?: string;
    /**事件涉及的账户 */
    address?: string;
    /**事件类型，如果不传入则不筛选事件类型，事件类型请参考<事件类型> */
    type?: string[];
    /**索引值 */
    storageValue?: string;
    /**查看第几页（一页20条记录） */
    page?: number;
}

```
- 返回参数：
```typescript
interface GetTransactions {
    /**是否成功 */
    success: boolean;
    result: {
        /* Mongodb.TransactionInBlockInsertModel[] */
        trs: {
            transaction: TransactionJSON;
            /**事件在区块内的索引 */
            tIndex: number;
            /**区块高度 */
            height: number;
            /**事件涉及的账户权益变动信息 */
            transactionAssetChanges: {
                /**账户类型 */
                accountType: number;
                /**变动的权益所属网络标识符 */
                sourceChainMagic: string;
                /**变动的权益名 */
                assetType: string;
                /**变动后的权益数 */
                assetPrealnum: string;
            }[];
            /**权益销毁前权益的最新信息 */
            assetPrealnum?: {
                /**剩余的权益数量 */
                remainAssetPrealnum: string;
                /**冻结的主权益数量 */
                frozenMainAssetPrealnum: string;
            };
            /**区块锻造者的签名 */
            signature: string;
            /**区块锻造者的安全签名 */
            signSignature?: string;
        }[] ;
        /**符合查询条件的总数 */
        count: number;
        /**分页查询最大值 */
        cmdLimitPerQuery: number;
    };
}

```
## 6.生成私钥
- 接口全称：`generateSecret`
- 接口简写：`gs`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/basic/generateSecret`
- 请求参数：
```typescript
interface GenerateSecret {
    /**语言 */
    lang: "cn" | "en";
}

```
- 返回参数：
```typescript
interface GenerateSecret {
    /**是否成功 */
    success: boolean;
    result: {
        /**账户私钥 */
        secret: string;
    };
}

```
## 7.获取账户公钥
- 接口全称：`getAccountPublicKey`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/basic/getAccountPublicKey`
- 请求参数：
```typescript
interface GetAccountPublicKey {
    /**账户地址 */
    address: string;
}

```
- 返回参数：
```typescript
interface GetAccountPublicKey {
    /**是否成功 */
    success: boolean;
    result: {
        /**账户公钥 */
        publicKey?: string;
    };
}

```
## 8.获取账户的最后一笔交易
- 接口全称：`getAccountLastTransaction`
- 接口简写：`galt`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/basic/getAccountLastTransaction`
- 请求参数：
```typescript
interface GetAccountLastTransaction {
    /**账户地址 */
    address: string;
    /**权益类型 */
    assetType: string;
}

```
- 返回参数：
```typescript
interface GetAccountLastTransaction {
    /**是否成功 */
    success: boolean;
    result: {
        transactionInBlock?: {
            transaction: TransactionJSON;
            /**事件在区块内的索引 */
            tIndex: number;
            /**区块高度 */
            height: number;
            /**事件涉及的账户权益变动信息 */
            transactionAssetChanges: {
                /**账户类型 */
                accountType: number;
                /**变动的权益所属网络标识符 */
                sourceChainMagic: string;
                /**变动的权益名 */
                assetType: string;
                /**变动后的权益数 */
                assetPrealnum: string;
            }[];
            /**权益销毁前权益的最新信息 */
            assetPrealnum?: {
                /**剩余的权益数量 */
                remainAssetPrealnum: string;
                /**冻结的主权益数量 */
                frozenMainAssetPrealnum: string;
            };
            /**区块锻造者的签名 */
            signature: string;
            /**区块锻造者的安全签名 */
            signSignature?: string;
        };
        assetIndex?: {
            [assetType: string]: number;
        };
    };
}

```
## 9.根据交易类型获取账户的最后一笔交易
- 接口全称：`getAccountLastTypeTransaction`
- 接口简写：`galtt`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/basic/getAccountLastTypeTransaction`
- 请求参数：
```typescript
interface GetAccountLastTypeTransaction {
    /**账户地址 */
    address: string;
    /**事件类型，通过 GetTransactionType 接口获得 */
    transactionType: string;
}

```
- 返回参数：
```typescript
interface GetAccountLastTypeTransaction {
    /**是否成功 */
    success: boolean;
    result: {
        transactionInBlock?: {
            transaction: TransactionJSON;
            /**事件在区块内的索引 */
            tIndex: number;
            /**区块高度 */
            height: number;
            /**事件涉及的账户权益变动信息 */
            transactionAssetChanges: {
                /**账户类型 */
                accountType: number;
                /**变动的权益所属网络标识符 */
                sourceChainMagic: string;
                /**变动的权益名 */
                assetType: string;
                /**变动后的权益数 */
                assetPrealnum: string;
            }[];
            /**权益销毁前权益的最新信息 */
            assetPrealnum?: {
                /**剩余的权益数量 */
                remainAssetPrealnum: string;
                /**冻结的主权益数量 */
                frozenMainAssetPrealnum: string;
            };
            /**区块锻造者的签名 */
            signature: string;
            /**区块锻造者的安全签名 */
            signSignature?: string;
        };
    };
}

```
## 10.创建账户
- 接口全称：`createAccount`
- 接口简写：`ca`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/basic/createAccount`
- 请求参数：
```typescript
interface CreateAccount {
    /**账户密钥 */
    secret: string;
}

```
- 返回参数：
```typescript
interface CreateAccount {
    /**是否成功 */
    success: boolean;
    result: {
        /**地址 */
        address: string;
        /**公钥 */
        publicKey: string;
        /**私钥 */
        secretKey: string;
    };
}

```
## 11.获取节点状态
- 接口全称：`getBlockChainStatus`
- 接口简写：`gbc`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`get`
- 接口`url`地址：`/api/basic/getBlockChainStatus`
- 请求参数：无
- 返回参数：
```typescript
interface GetBlockChainStatus {
    /**是否成功 */
    success: boolean;
    result: {
        /**节点运行状态：0 离线，不可用 1 自由状态，有空闲资源可用 2 重建区块链 3 节点共识 4 重放区块 5 锻造区块 6 回滚区块 */
        status: any;
        /**当前的节点连接数 */
        peers: number;
        /**当前节点是否完成初始化数据 */
        isReady: boolean;
        /**节点的当前时间戳 */
        serverTimestamp: number;
    };
}

```
## 12.临时设置KV二进制数据
- 接口全称：`setKVStorageTemp`
- 接口简写：`skv`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/basic/setKVStorageTemp`
- 请求参数：
```typescript
interface SetKVStorageTemp {
    /**二进制的数据 */
    datas: Uint8Array[];
}

```
- 返回参数：
```typescript
interface SetKVStorageTemp {
    /**是否成功 */
    success: boolean;
    result: {
        keys: string[];
    };
}

```
## 13.获取KV二进制数据
- 接口全称：`getKVStorage`
- 接口简写：`gkv`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/basic/getKVStorage`
- 请求参数：
```typescript
interface GetKVStorage {
    /**二进制的存储key值 */
    key: string;
}

```
- 返回参数：
```typescript
interface GetKVStorage {
    /**是否成功 */
    success: boolean;
    result: {
        data: Uint8Array;
    };
}

```
## 14.获取指定账户
- 接口全称：`getAccountInfoAndAssets`
- 接口简写：`ga`
- 可调用方式：`命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/basic/getAccountInfoAndAssets`
- 请求参数：
```typescript
interface GetAccountInfoAndAssets {
    /**账户地址 */
    address: string;
}

```
- 返回参数：
```typescript
interface GetAccountInfoAndAssets {
    /**是否成功 */
    success: boolean;
    result: MemInfoModel.AccountInfoAndAsset;
}

```
