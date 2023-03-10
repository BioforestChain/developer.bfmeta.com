# Basic interface
## 1. Get the BFMeta version number
- The full name of the interface: `nodeVersion`
- Interface abbreviation: `v`
- Callable methods: `Http, Websocket, command line`
- Calling method: `get`
- Interface `url` address: `/api/basic/nodeVersion`
- Request parameters: none
- Return parameters:
```typescript
interface NodeVersion {
    /**whether succeed */
    success: boolean;
    result: {
        /**The version number of the current node */
        version: string;
    };
}

```
## 2.Get event type
- The full name of the interface：`getTransactionType`
- Interface abbreviation：`none`
- Callable methods：`Http,Websocket`
- Calling method：`post`
- Interface `url` address：`/api/basic/getTransactionType`
- Request parameters：
```typescript
interface GetTransactionType {
    /**event base type */
    baseType: {
        //Secondary Password
        SIGNATURE = "BSE-01",
        //Registered Miner
        DELEGATE = "BSE-02",
        //Governance Vote
        VOTE = "BSE-03",
        //Set Username
        USERNAME = "BSE-04",
        //Start Collecting Vote
        ACCEPT_VOTE = "BSE-05",
        //Stop Receiving Vote
        REJECT_VOTE = "BSE-06",
        //Create DAPPID
        DAPP = "WOD-00",
        //DAPPID Payment 
        DAPP_PURCHASING = "WOD-01",
        //Register New Blockchain
        REGISTER_CHAIN = "WOD-02",
        //Data storage certificate
        MARK = "EXT-00",
        //Create Equity
        ISSUE_ASSET = "AST-00",
        //Equity transfer
        TRANSFER_ASSET = "AST-01",
        //Burn Equity
        DESTORY_ASSET = "AST-02",
        //Initiate a gift of Equity
        GIFT_ASSET = "AST-03",
        //Accept gift
        GRAB_ASSET = "AST-04",
        //Initiate Equity entrustment
        TRUST_ASSET = "AST-05",
        //Sign for asset entrustment
        SIGN_FOR_ASSET = "AST-06",
        //Move out of Equity
        EMIGRATE_ASSET = "AST-07",
        //Move in of Equity
        IMMIGRATE_ASSET = "AST-08",
        //Initiate Equity exchange
        TO_EXCHANGE_ASSET = "AST-09",
        //Accept the exchange of Equity
        BE_EXCHANGE_ASSET = "AST-10",
        //Initiate asset exchange
        TO_EXCHANGE_SPECIAL_ASSET = "AST-11",
        //accept asset exchange
        BE_EXCHANGE_SPECIAL_ASSET = "AST-12",
        //Register/deregister blockchain domain name
        LOCATION_NAME = "LNS-00",
        //Set blockchain domain name resolution value
        SET_LNS_RECORD_VALUE = "LNS-01",
        //Set blockchain domain administrator
        SET_LNS_MANAGER = "LNS-02",
        //Personality event
        CUSTOM = "CUS-00",
    };
}

```
- Return parameters：
```typescript
interface GetTransactionType {
    /**whether succeed */
    success: boolean;
    result: {
        /**Full name of event type  */
        type: string;
    };
}

```

## 3.Get the latest block of the local node
- The full name of the interface：`getLastBlock`
- Interface abbreviation：`glb`
- Callable methods：`Http,Websocket,command line`
- Calling method：`get`
- Interface `url` address：`/api/basic/getLastBlock`
- Request parameters：none
- Return parameters：
```typescript
interface GetLastBlock {
    /**whether succeed */
    success: boolean;
    result: {
        /**Block version number */
        version: number;
        /**Block height */
        height: number;
        /**Block timestamp */
        timestamp: number;
        /**Block size */
        blockSize: number;
        /**Miner's public key */
        generatorPublicKey: string;
        /**Miner's security public key */
        generatorSecondPublicKey?: string;
        /**Miner's Equity */
        generatorEquity: string;
        /**previous Block Signature */
        previousBlockSignature: string;
        /**Block reward value */
        reward: string;
        /**The block's chain identifier */
        magic: string;
        /**Block Participation */
        blockParticipation: string;
        /**Block signature */
        signature: string;
        /**Block security signature */
        signSignature?: string;
        /**Block remarks information */
        remark: {
            [key: string]: string;
        };
        /**Block event information */
        transactionInfo: {
            startTindex: number;
            numberOfTransactions: number;
            payloadHash: string;
            payloadLength: number;
            totalAmount: string;
            totalFee: string;
            transactionInBlocks: [];
            statisticInfo: {
                /**The total transaction fee of the events packaged in the block */
                totalFee: string;
                /**The total asset of the event packaged in the block (without distinction of asset type) */
                totalAsset: string;
                /**The total asset of the event packaged in the block */
                totalChainAsset: string;
                /**The total number of accounts involved in block packaging events */
                totalAccount: number;
                /**The details of block packaging asset, JSON object */
                magicAssetTypeTypeStatisticHashMap: {
                    [magic: string]: {
                        /**The details of block packaging asset, JSON object */
                        assetTypeTypeStatisticHashMap: {
                            [assetType: string]: {
                                /**The details of block packaging event types, JSON object */
                                typeStatisticHashMap: {
                                    [baseType: string]: {
                                        /**Changes in the total value, the increase or decrease of an account will have an impact */
                                        changeAmount: string;
                                        /**The total number of changes, the increase or decrease of an account will have an impact */
                                        changeCount: number;
                                        /**Total asset migration */
                                        moveAmount: string;
                                        /**Transaction volume */
                                        transactionCount: number;
                                    };
                                };
                                /**The details of block packaging event types, JSON object */
                                total: {
                                    /**Changes in the total value, the increase or decrease of an account will have an impact */
                                    changeAmount: string;
                                    /**The total number of changes, the increase or decrease of an account will have an impact */
                                    changeCount: number;
                                    /**Total asset migration */
                                    moveAmount: string;
                                    /**Transaction volume */
                                    transactionCount: number;
                                };
                            };
                        };
                    };
                };
            };
        };
        /**Block additional information */
        asset: any;
    };
}

```
## 4.Get the specified block
- The full name of the interface：`getBlock`
- Interface abbreviation：`gb`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface `url` address：`/api/basic/getBlock`
- Request parameters：
```typescript
interface GetBlock {
    /**Block signature */
    signature?: string;
    /**Block height */
    height?: number;
    /**What page to view (20 records per page) */
    page?: number;
}

```
- Return parameters：
```typescript
interface GetBlock {
    /**whether succeed */
    success: boolean;
    result: {
        blocks: {
            /**The version of the block */
            version: number;
            /**Block height */
            height: number;
            /**block size */
            blockSize: number;
            /**Block timestamp */
            timestamp: number;
            /**Block signature */
            signature: string;
            /**Block Security Signature */
            signSignature?: string;
            /**Miner's public key */
            generatorPublicKey: string;
            /**Miner's secure public key */
            generatorSecondPublicKey?: string;
            /**Miner's Equity */
            generatorEquity: string;
            /**Previous block signature */
            previousBlockSignature: string;
            /**Block reward value */
            reward: string;
            /**The block's chain identifier */
            magic: string;
            /**Block participation */
            blockParticipation: string;
            /**Block Remark Information */
            remark: {
                [key: string]: string;
            };
            /**Block additional information */
            asset: object;
            /**Miner drop list */
            roundOfflineGeneratersHashMap: {
                /**
                 * Use comma separated addresses
                 * address,address */
                [roundOffset: string]: string;
            };
            /**Event */
            transactionInfo: {
                startTindex: number;
                numberOfTransactions: number;
                payloadHash: string;
                payloadLength: number;
                totalAmount: string;
                totalFee: string;
                transactionInBlocks: [];
                statisticInfo: {
                    /**The total transaction fee of the event packaged in the block*/
                    totalFee: string;
                    /**The total asset of the event packaged in the block (without distinction of asset type) */
                    totalAsset: string;
                    /**The total asset of the event packaged in the block */
                    totalChainAsset: string;
                    /**The total number of accounts involved in block packaging events */
                    totalAccount: number;
                    /**The details of block packaging asset, JSON object */
                    magicAssetTypeTypeStatisticHashMap: {
                        [magic: string]: {
                            /**The details of block packaging asset, JSON object */
                            assetTypeTypeStatisticHashMap: {
                                [assetType: string]: {
                                    /**The details of block packaging event types, JSON object */
                                    typeStatisticHashMap: {
                                        [baseType: string]: {
                                            /**Changes in the total value, the increase or decrease of an account will have an impact */
                                            changeAmount: string;
                                            /**The total number of changes, the increase or decrease of an account will have an impact */
                                            changeCount: number;
                                            /**Total asset migration */
                                            moveAmount: string;
                                            /**Transaction volume */
                                            transactionCount: number;
                                        };
                                    };
                                    /**Event type statistics of block packaging, JSON object */
                                    total: {
                                        /**Changes in the total value, the increase or decrease of an account will have an impact */
                                        changeAmount: string;
                                        /**The total number of changes, the increase or decrease of an account will have an impact */
                                        changeCount: number;
                                        /**Total asset migration */
                                        moveAmount: string;
                                        /**Transaction volume */
                                        transactionCount: number;
                                    };
                                };
                            };
                        };
                    };
                };
            };
        }[];
        /**The total number of matching query conditions */
        count: number;
        /**Paging query maximum */
        cmdLimitPerQuery: number;
    };
}

```
## 5.Get the specified event
- The full name of the interface：`getTransactions`
- Interface abbreviation：`gt`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface `url` address：`/api/basic/getTransactions`
- Request parameters：
```typescript
interface GetTransactions {
    /**Event id */
    signature?: string;
    /**The Block height to which the event belongs */
    height?: number;
    /**The lowest altitude the event belongs to */
    minHeight?: number;
    /**The highest altitude the event belongs to */
    maxHeight?: number;
    /**Event initiation address */
    senderId?: string;
    /**Event receiving address */
    recipientId?: string;
    /**Account involved in the event */
    address?: string;
    /**Event type, if not passed in, the event type will not be filtered, please refer to <event type> for event type */
    type?: string[];
    /**storage value */
    storageValue?: string;
    /**What page to view (20 records per page) */
    page?: number;
}

```
- Return parameters：
```typescript
interface GetTransactions {
    /**whether succeed */
    success: boolean;
    result: {
        /* Mongodb.TransactionInBlockInsertModel[] */
        trs: {
            transaction: TransactionJSON;
            /**The index of the event within the block */
            tIndex: number;
            /**Block height */
            height: number;
            /**Information about changes in account equity involved in the event */
            transactionAssetChanges: {
                /**account type */
                accountType: number;
                /**The network identifier to which the changed equity belongs */
                sourceChainMagic: string;
                /**Changed equity name */
                assetType: string;
                /**Equity after change */
                assetPrealnum: string;
            }[];
            /**The latest information of the equity before the equity is destroyed */
            assetPrealnum?: {
                /**Amount of remaining equity */
                remainAssetPrealnum: string;
                /**The number of frozen master stakes */
                frozenMainAssetPrealnum: string;
            };
            /**Block miner's signature */
            signature: string;
            /**Security signature of the block miner */
            signSignature?: string;
        }[] ;
        /**The total number of matching query conditions */
        count: number;
        /**Paging query maximum */
        cmdLimitPerQuery: number;
    };
}

```
## 6.Generate private key
- The full name of the interface：`generateSecret`
- Interface abbreviation：`gs`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface `url` address：`/api/basic/generateSecret`
- Request parameters：
```typescript
interface GenerateSecret {
    /**Language */
    lang: "cn" | "en";
}

```
- Return parameters：
```typescript
interface GenerateSecret {
    /**Whether succeed */
    success: boolean;
    result: {
        /**Account private key */
        secret: string;
    };
}

```
## 7.Get account public key
- The full name of the interface：`getAccountPublicKey`
- Interface abbreviation：`none`
- Callable methods：`Http,Websocket`
- Calling method：`post`
- Interface `url` address：`/api/basic/getAccountPublicKey`
- Request parameters：
```typescript
interface GetAccountPublicKey {
    /**Account address */
    address: string;
}

```
- Return parameters：
```typescript
interface GetAccountPublicKey {
    /**Whether succeed */
    success: boolean;
    result: {
        /**Account public key */
        publicKey?: string;
    };
}

```
## 8.Get the last transaction of an account
- The full name of the interface：`getAccountLastTransaction`
- Interface abbreviation：`galt`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface `url` address：`/api/basic/getAccountLastTransaction`
- Request parameters：
```typescript
interface GetAccountLastTransaction {
    /**Account address */
    address: string;
    /**Asset Type */
    assetType: string;
}

```
- Return parameters：
```typescript
interface GetAccountLastTransaction {
    /**whether succeed */
    success: boolean;
    result: {
        transactionInBlock?: {
            transaction: TransactionJSON;
            /**The index of the event within the block */
            tIndex: number;
            /**Block height */
            height: number;
            /**Information about changes in account equity involved in the event */
            transactionAssetChanges: {
                /**Account type */
                accountType: number;
                /**The network identifier to which the changed equity belongs */
                sourceChainMagic: string;
                /**Changed equity name */
                assetType: string;
                /**Equity after change */
                assetPrealnum: string;
            }[];
            /**The latest information of the equity before the equity is destroyed */
            assetPrealnum?: {
                /**Amount of remaining equity */
                remainAssetPrealnum: string;
                /**The number of frozen master stakes */
                frozenMainAssetPrealnum: string;
            };
            /**Block miner's signature */
            signature: string;
            /**Security signature of the block miner */
            signSignature?: string;
        };
        assetIndex?: {
            [assetType: string]: number;
        };
    };
}

```
## 9.Get the last transaction of the account according to the transaction type
- The full name of the interface：`getAccountLastTypeTransaction`
- Interface abbreviation：`galtt`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface `url` address：`/api/basic/getAccountLastTypeTransaction`
- Request parameters：
```typescript
interface GetAccountLastTypeTransaction {
    /**account address */
    address: string;
    /**Event type, obtained through the 'GetTransactionType' interface */
    transactionType: string;
}

```
- Return parameters：
```typescript
interface GetAccountLastTypeTransaction {
    /**whether succeed */
    success: boolean;
    result: {
        transactionInBlock?: {
            transaction: TransactionJSON;
            /**The index of the event within the block */
            tIndex: number;
            /**Block height */
            height: number;
            /**Information about changes in account equity involved in the event */
            transactionAssetChanges: {
                /**account type */
                accountType: number;
                /**The network identifier to which the changed equity belongs */
                sourceChainMagic: string;
                /**Changed equity name */
                assetType: string;
                /**Equity after change */
                assetPrealnum: string;
            }[];
            /**The latest information of the equity before the equity is destroyed */
            assetPrealnum?: {
                /**Amount of remaining equity */
                remainAssetPrealnum: string;
                /**The number of frozen main equity */
                frozenMainAssetPrealnum: string;
            };
            /**Block miner's signature */
            signature: string;
            /**Security signature of the block miner */
            signSignature?: string;
        };
    };
}

```
## 10.Create Account
- The full name of the interface：`createAccount`
- Interface abbreviation：`ca`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface `url` address：`/api/basic/createAccount`
- Request parameters：
```typescript
interface CreateAccount {
    /**Account key */
    secret: string;
}

```
- Return parameters：
```typescript
interface CreateAccount {
    /**whether succeed */
    success: boolean;
    result: {
        /**Address */
        address: string;
        /**Public key */
        publicKey: string;
        /**Private key */
        secretKey: string;
    };
}

```
## 11.Get node status
- The full name of the interface：`getBlockChainStatus`
- Interface abbreviation：`gbc`
- Callable methods：`Http,Websocket,command line`
- Calling method：`get`
- Interface `url` address：`/api/basic/getBlockChainStatus`
- Request parameters：none
- Return parameters：
```typescript
interface GetBlockChainStatus {
    /**whether succeed */
    success: boolean;
    result: {
        /**Node running status: 0 Offline, unavailable 1 Free state, idle resources available 2 Rebuild blockchain 3 Node consensus 4 Replay block 5 Forge block 6 Rollback block */
        status: any;
        /**The current number of node connections */
        peers: number;
        /**Whether the current node has completed initializing data */
        isReady: boolean;
        /**the node's current timestamp */
        serverTimestamp: number;
    };
}

```
## 12.Temporarily set the KV binary data
- The full name of the interface：`setKVStorageTemp`
- Interface abbreviation：`skv`
- Callable methods：`Http,Websocket`
- Calling method：`post`
- Interface `url` address：`/api/basic/setKVStorageTemp`
- Request parameters：
```typescript
interface SetKVStorageTemp {
    /**Binary data */
    datas: Uint8Array[];
}

```
- Return parameters：
```typescript
interface SetKVStorageTemp {
    /**whether succeed */
    success: boolean;
    result: {
        keys: string[];
    };
}

```
## 13.Get KV binary data
- The full name of the interface：`getKVStorage`
- Interface abbreviation：`gkv`
- Callable methods：`Http,Websocket`
- Calling method：`post`
- Interface `url` address：`/api/basic/getKVStorage`
- Request parameters：
```typescript
interface GetKVStorage {
    /**Binary storage key value */
    key: string;
}

```
- Return parameters：
```typescript
interface GetKVStorage {
    /**whether succeed */
    success: boolean;
    result: {
        data: Uint8Array;
    };
}

```
## 14.Get the specified account
- The full name of the interface：`getAccountInfoAndAssets`
- Interface abbreviation：`ga`
- Callable methods：`command line`
- Calling method：`post`
- Interface `url` address：`/api/basic/getAccountInfoAndAssets`
- Request parameters：
```typescript
interface GetAccountInfoAndAssets {
    /**account address */
    address: string;
}

```
- Return parameters：
```typescript
interface GetAccountInfoAndAssets {
    /**whether succeed */
    success: boolean;
    result: MemInfoModel.AccountInfoAndAsset;
}

```

