# 类型定义
## BFMeta.TransactionJSON

```typescript
interface TransactionJSON<AssetJSON extends object = object> { 
    /**事件版本号 */
    version: number;
    /**事件类型 */
    type: string;
    /**事件的发起账户地址，base58 编码的 16 进制字符串 */
    senderId: string;
    /**事件的发起账户公钥，128 个字节的 16 进制字符串 */
    senderPublicKey: string;
    /**事件的发起账户安全公钥，128 个字节的 16 进制字符串 */
    senderSecondPublicKey?: string;
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId?: string;
    /**事件的接收范围类型 */
    rangeType: BFMeta.RANGE_TYPE;
    /**事件的接收范围 */
    range: string[];
    /**事件的手续费 */
    fee: string;
    /**事件的时间戳 */
    timestamp: number;
    /**事件所属的 dappid */
    dappid?: string;
    /**事件所属的位名 */
    lns?: string;
    /**事件的来源IP，IPv4或者IPv6，不包含头尾(例如: 127.0.0.1)，默认为空 */
    sourceIP?: string;
    /**事件的来源链网络标识符 */
    fromMagic: string;
    /**事件的去往链网络标识符 */
    toMagic: string;
    /**事件的发起高度 */
    applyBlockHeight: number;
    /**事件的有效高度 */
    effectiveBlockHeight: number;
    /**事件的签名 */
    signature: string;
    /**事件的安全签名 */
    signSignature?: string;
    /**事件的备注信息 */
    remark: {
        [key: string]: string;
    };
    /**实际事件部分 */
    asset: AssetJSON;
    /**事件的索引对象 */
    storage?: TransactionStorageJSON;
    /**事件的索引键，提供额外查询使用的字段名 */
    storageKey?: TransactionStorageJSON["key"];
    /**事件的索引值，提供额外查询使用的字段值 */
    storageValue?: TransactionStorageJSON["value"];
    /**事件 pow 噪点 */
    nonce: number;
}
```
## BFMeta.RANGE_TYPE

```typescript
enum RANGE_TYPE {
  /**不限定范围 */
  EMPTY = 0,
  /**多地址 */
  MULTI_ADDRESS = 1,
  /**DAppid范围 */
  MULTI_DAPPID = 2,
  /**链域名范围 */
  MULTI_LOCATION_NAME = 4,
}
```
## BFMeta.TransactionStorageJSON

```typescript
interface TransactionStorageJSON { 
    /**事件的索引键，提供额外查询使用的字段名 */
    key: string;
    /**事件的索引值，提供额外查询使用的字段值 */
    value: string;
}
```
## BFMetaPC.MemInfoModel.AccountInfoAndAsset

```typescript
type AccountInfoAndAsset = {
    /**账户基础信息，JSON对象 */
    accountInfo: AccountInfoModel;
    /**账户权益信息，JSON对象 */
    accountAssets: AccountAssetsModel;
};
```
## BFMetaPC.MemInfoModel.AccountInfoModel

```typescript
type AccountInfoModel = {
    /**账户地址 */
    address: string;
    /**账户公钥 */
    publicKey?: string;
    /**账户安全公钥 */
    secondPublicKey?: string;
    /**账户别名 */
    username?: string;
    /**账户状态：0 为正常 1 为冻结状态 */
    accountStatus: number;
    /**是否为受托人：false 为普通账户 true 为受托人 */
    isDelegate: boolean;
    /**是否接收投票：false 为不接收 true 为接收投票 */
    isAcceptVote: boolean;
    /**已锻造区块数 */
    producedblocks: number;
    /**掉块数量，被选为受托人并且到达锻造区块的时间却因为各种原因没有锻造出相应的区块的次数 */
    missedblocks: number;
    /**累计事件量 */
    numberOfTransactions: number;
    /**获得的权益信息 */
    voteInfo: VoteInfoModel;
    /**参与治理投票的权益信息 */
    equityInfo: EquityInfoModel;
    /**上一轮轮末的权益量和事件量信息，倒数第一轮的轮次信息，若当前为 600 ，则当前为第11轮，这个值指的则是第10轮 */
    lastRoundInfo: RoundInfoModel;
    /**账户变动区块高度 */
    height: number;
    /**账户在线率 */
    productivity?: number;
};
```
## BFMetaPC.MemInfoModel.VoteInfoModel

```typescript
type VoteInfoModel = {
    /**权益所属轮次 */
    round: number;
    /**获得的权益数量 */
    vote: bigint;
};
```
## BFMetaPC.MemInfoModel.EquityInfoModel

```typescript
type EquityInfoModel = {
    /**权益所属轮次 */
    round: number;
    /**权益剩余值 */
    equity: bigint;
    /**权益初始值 */
    fixedEquity: bigint;
};
```
## BFMetaPC.MemInfoModel.RoundInfoModel

```typescript
type RoundInfoModel = {
    /**所属轮次 */
    round: number;
    /**轮末持有的权益量 */
    assetNumber: bigint;
    /**轮末的事件量 */
    txCount: number;
};
```
## BFMetaPC.MemInfoModel.AccountAssetsModel

```typescript
type AccountAssetsModel = {
    /**账户地址 */
    address: string;
    /**账户公钥 */
    publicKey?: string;
    /**账户权益 */
    assets: AssetsModel;
    /**累计花费的手续费 */
    paidFee: bigint;
    /**累计投票收益 */
    votingRewards: bigint;
    /**累计打块收益 */
    forgingRewards: bigint;
    /**账户变动区块高度 */
    height: number;
};
```
## BFMetaPC.MemInfoModel.AssetsModel

```typescript
type AssetsModel = {
    /**某条链的权益信息 */
    [sourceChainMagic: string]: {
        /**某种权益信息 */
        [assetType: string]: AccountAssetSubModel;
    };
};
```
## BFMetaPC.MemInfoModel.AccountAssetSubModel

```typescript
type AccountAssetSubModel = {
    /**权益所属链网络标识符 */
    sourceChainMagic: string;
    /**权益名 */
    assetType: string;
    /**权益所属主链名称 */
    sourceChainName?: string;
    /**权益数量 */
    assetNumber: bigint;
    /**上上轮轮末的权益量和事件量 */
    penultimateRoundInfo?: RoundInfoModel;
    /**上一轮轮末的权益量和事件量 */
    lastRoundInfo?: RoundInfoModel;
};
```
## BFMetaNodeSDK.Transaction.TRANSACTION.TransactionCommonParams

```typescript
interface TransactionCommonParams { 
    /**发起账户的公钥 */
    publicKey: string;
    /**发起账户的安全公钥 */
    secondPublicKey?: string;
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId?: string;
    /**事件的接收范围类型，只能是 0，1，2，4 中的某一个，0 表示不限定操作范围，1 表示只有指定的账户地址才能对这笔事件进行操作，2 表示只有指定的 dappid 才能对这笔事件进行操作，4 表示只有指定的位名才能对这笔事件进行操作，默认为 0 */
    rangeType?: number;
    /**事件的接收范围，当 rangeType 为 0 时，不能填写任何数据，当 rangeType 为 1 时，只能填写账户地址，当 rangeType 为 2 时，只能填写 dappid，当 rangeType 为 4 时，只能填写位名，默认为空 */
    range?: string[];
    /**事件的手续费 */
    fee: string;
    /**事件的发起高度，0-9 组成并且不包含小点，可选，默认使用当前区块链的最新高度 */
    applyBlockHeight: number;
    /**事件备注信息, 为json对象 */
    remark?: {
        [key: string]: string;
    };
    /**事件所属的 dappid，大写字母或数字，8 个字符，默认为空 */
    dappid?: string;
    /**事件所属的位名，2-1024 个字符，每级位名最大长度为 128 个字符，一级位名只能时小写字母组成，二级及以上开头及结尾只能由小写字母或数字组成，中间可以包含下划线，根位名必须时本链链名，可选，默认为空 */
    lns?: string;
    /**事件的来源IP，IPv4或者IPv6，不包含头尾(例如: 127.0.0.1)，默认为空 */
    sourceIP?: string;
    /**事件的来源链网络标识符，大写字母或数字组成，5 个字符，默认使用创世块的 magic */
    fromMagic?: string;
    /**事件的去往链网络标识符，大写字母或数字组成，5 个字符，默认使用创世块的 magic */
    toMagic?: string;
    /**事件的过期区块间隔，默认使用创世块最大过期时间参数，0-9 组成并且不包含小数点 */
    numberOfEffectiveBlocks?: number;
}
```

## BFMeta.TransferAssetAssetJSON

```typescript
interface TransferAssetAssetJSON { 
    /**权益转移事件附带信息 */
    transferAsset: TransferAssetJSON;
}
```
## BFMeta.TransferAssetJSON

```typescript
interface TransferAssetJSON { 
    /**转移的权益所属链名，小写字母组成，3-8 位 */
    sourceChainName: string;
    /**转移的权益所属链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic: string;
    /**转移的权益名称，大写字母组成，3-5 个字符 */
    assetType: string;
    /**转移的权益数量，0-9 组成并且不包含小数点，必须大于0 */
    amount: string;
}
```
## BFMeta.SignatureAssetJSON

```typescript
interface SignatureAssetJSON { 
    /**设置安全密码事件附带信息 */
    signature: SignatureJSON;
}
```
## BFMeta.SignatureJSON

```typescript
interface SignatureJSON { 
    /**安全密钥生成的公钥，128 个字节的 16 进制字符串 */
    publicKey: string;
}
```
## BFMeta.UsernameAssetJSON

```typescript
interface UsernameAssetJSON { 
    /**地址名命事件附带信息 */
    username: UsernameJSON;
}
```
## BFMeta.UsernameJSON

```typescript
interface UsernameJSON { 
    /**用户名字符串，大小写字母、数字、下划线组成，1-20 个字符，不能包含本链名 */
    alias: string;
}
```
## BFMeta.DelegateAssetJSON

```typescript
interface DelegateAssetJSON { 
```
## BFMeta.AcceptVoteAssetJSON

```typescript
interface AcceptVoteAssetJSON { 
```
## BFMeta.RejectVoteAssetJSON

```typescript
interface RejectVoteAssetJSON { 
```
## BFMeta.VoteAssetJSON

```typescript
interface VoteAssetJSON { 
    /**投票事件附带信息 */
    vote: VoteJSON;
}
```
## BFMeta.VoteJSON

```typescript
interface VoteJSON { 
    /**投出的权益数，0-9 组成并且不包含小数点，允许为 0 */
    equity: string;
}
```
## BFMeta.DAppAssetJSON

```typescript
interface DAppAssetJSON { 
    /**发行 dapp 事件附带信息 */
    dapp: DAppJSON;
}
```
## BFMeta.DAppJSON

```typescript
interface DAppJSON { 
    /**dappid 所属的链名，小写字母组成，3-8 位 */
    sourceChainName: string;
    /**dappid 所属的链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic: string;
    /**dappid，大写字母或数字组成，8 个字符，最后一位是校验位 */
    dappid: string;
    /**dappid 的类型，只能为 0 或 1；0 表示这个 dappid 是付费使用的，1 表示这个 dappid 是免费使用的 */
    type: BFMeta.DAPP_TYPE;
    /**购买 dappid 使用的权益 */
    purchaseAsset?: DAppPurchaseAssetJSON;
}
```
## BFMeta.DAPP_TYPE

```typescript
enum DAPP_TYPE {
  /**付费应用 */
  PAID_APP = 0, // "PAID_APP",
  /**免费应用 */
  FREE_APP = 1, // "FREE_APP",
}
```
## BFMeta.DAppPurchaseAssetJSON

```typescript
interface DAppPurchaseAssetJSON { 
    /**购买dappid使用权的权益所属链名，小写字母组成，3-8 位 */
    sourceChainName: string;
    /**购买 dappid 使用权的DAPPID付费所属链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic: string;
    /**购买 dappid 使用权的权益名称，大写字母组成，3-5 个字符 */
    assetType: string;
    /**购买 dappid 使用权需要的权益数量(如果 dappid 是付费应用则必须携带，如果是免费应用则无需携带)，0-9 组成并且不包含小数点，必须大于 0 */
    amount: string;
}
```
## BFMeta.DAppPurchasingAssetJSON

```typescript
interface DAppPurchasingAssetJSON { 
    /**购买 dapp 事件附带信息 */
    dappPurchasing: DAppPurchasingJSON;
}
```
## BFMeta.DAppPurchasingJSON

```typescript
interface DAppPurchasingJSON { 
    /**购买的 dapp 信息 */
    dappAsset: DAppJSON;
}
```
## BFMeta.MarkAssetJSON

```typescript
interface MarkAssetJSON { 
    /**存证事件附带信息 */
    mark: MarkJSON;
}
```
## BFMeta.MarkJSON

```typescript
interface MarkJSON { 
    /**存证内容，为任意字符串 */
    content: string;
    /**存证类型，为任意字符串，用于区别存证 */
    action: string;
    /**存证事件使用的 dapp 信息 */
    dapp: DAppJSON;
}
```
## BFMeta.IssueAssetAssetJSON

```typescript
interface IssueAssetAssetJSON { 
    /**发行权益事件附带信息 */
    issueAsset: IssueAssetJSON;
}
```
## BFMeta.IssueAssetJSON

```typescript
interface IssueAssetJSON { 
    /**权益所属链名，小写字母组成，3-8 位 */
    sourceChainName: string;
    /**权益所属链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic: string;
    /**权益名称，大写字母组成，3-5 个字符 */
    assetType: string;
    /**发行的新权益总数，权益数量由0-9共十个数字组成，权益数量不包含小数点且必须大于0 */
    expectedIssuedAssets: string;
}
```
## BFMeta.DestoryAssetAssetJSON

```typescript
interface DestoryAssetAssetJSON { 
    /**权益销毁事件附带信息 */
    destoryAsset: DestoryAssetJSON;
}
```
## BFMeta.DestoryAssetJSON

```typescript
interface DestoryAssetJSON { 
    /**销毁的权益所属链名，小写字母组成，3-8 位 */
    sourceChainName: string;
    /**销毁的的权益所属链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic: string;
    /**销毁的权益名称，大写字母组成，3-5 个字符 */
    assetType: string;
    /**销毁的权益数量，0-9 组成并且不包含小数点，必须大于0 */
    amount: string;
}
```
## BFMeta.ToExchangeAssetAssetJSON

```typescript
interface ToExchangeAssetAssetJSON { 
    /**发起权益交换事件附带信息 */
    toExchangeAsset: ToExchangeAssetJSON;
}
```
## BFMeta.ToExchangeAssetJSON

```typescript
interface ToExchangeAssetJSON { 
    /**加密密钥生成的公钥数组 */
    cipherPublicKeys: string[];
    /**用于交换的权益来源链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    toExchangeSource: string;
    /**被交换的权益来源链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    beExchangeSource: string;
    /**用于交换的权益来源链名，小写字母组成，3-8 位 */
    toExchangeChainName: string;
    /**被交换的权益来源链名，小写字母组成，3-8 位 */
    beExchangeChainName: string;
    /**用于交换的权益名，大写字母组成，3-5 个字符 */
    toExchangeAsset: string;
    /**被交换的权益名，大写字母组成，3-5 个字符 */
    beExchangeAsset: string;
    /**用于交换的权益数量，0-9 组成并且不包含小数点，必须大于 0 */
    toExchangeNumber: string;
    /**权益的交换比例 */
    exchangeRate: BFMeta.RateJSON<string>;
}
```
## BFMeta.RateJSON

```typescript
interface RateJSON<T extends number | bigint | string = number> { 
    /**前部权重 */
    prevWeight: T;
    /**后部权重 */
    nextWeight: T;
}
```
## BFMeta.BeExchangeAssetAssetJSON

```typescript
interface BeExchangeAssetAssetJSON { 
    /**接收权益交换事件附带信息 */
    beExchangeAsset: BeExchangeAssetJSON;
}
```
## BFMeta.BeExchangeAssetJSON

```typescript
interface BeExchangeAssetJSON { 
    /**发起权益交换的事件签名，128 个字节的 16 进制字符串 */
    transactionSignature: string;
    /**加密密钥生成的签名数组 */
    ciphertextSignature?: AccountSignatureJSON;
    /**用于交换的权益数量，权益数量由0-9共十个数字组成，权益数量不包含小数点且必须大于0 */
    toExchangeNumber: string;
    /**交换得到的权益数量，权益数量由0-9共十个数字组成，权益数量不包含小数点且必须大于0 */
    beExchangeNumber: string;
    /**权益交换信息 */
    exchangeAsset: ToExchangeAssetJSON;
}
```
## BFMeta.AccountSignatureJSON

```typescript
interface AccountSignatureJSON { 
    /**账户密钥生成的公钥 */
    publicKey: string;
    /**账户公钥生成的签名 */
    signature: string;
    /**账户安全密钥生成的公钥 */
    secondPublicKey?: string;
    /**账户安全公钥生成的签名 */
    signSignature?: string;
}
```
## BFMeta.ToExchangeSpecialAssetAssetJSON

```typescript
interface ToExchangeSpecialAssetAssetJSON { 
    /**发起资产交换事件附带信息 */
    toExchangeSpecialAsset: ToExchangeSpecialAssetJSON;
}
```
## BFMeta.ToExchangeSpecialAssetJSON

```typescript
interface ToExchangeSpecialAssetJSON { 
    /**加密密钥生成的公钥数组 */
    cipherPublicKeys: string[];
    /**用于交换的权益/资产来源链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    toExchangeSource: string;
    /**被交换的资产/权益来源链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    beExchangeSource: string;
    /**用于交换的权益/资产来源链名，小写字母组成，3-8 位 */
    toExchangeChainName: string;
    /**被交换的资产/权益来源链名，小写字母组成，3-8 位 */
    beExchangeChainName: string;
    /**用于交换的权益/资产名，大写字母组成，3-5 个字符 */
    toExchangeAsset: string;
    /**被交换的资产/权益名，大写字母组成，3-5 个字符 */
    beExchangeAsset: string;
    /**用于交换或交换得到的权益数量，权益数量由0-9共十个数字组成，权益数量不包含小数点且必须大于0 */
    exchangeNumber: string;
    /**资产的类型，只能为 0 或 1，0 为 dappid，1 为位名 */
    exchangeAssetType: BFMeta.SPECIAL_ASSET_TYPE;
    /**资产的来源，只能为 0 或 1，0 为出售，1 为求购 */
    exchangeDirection: BFMeta.EXCHANGE_DIRECTION;
}
```
## BFMeta.SPECIAL_ASSET_TYPE

```typescript
enum SPECIAL_ASSET_TYPE {
  /**特殊资产类型：dapp */
  DAPP_ID,
  /**特殊资产类型：链域名 */
  LOCATION_NAME,
}
```
## BFMeta.EXCHANGE_DIRECTION

```typescript
enum EXCHANGE_DIRECTION {
  /**特殊资产来自 to 交易的发起账户，即出售 */
  ASSET_FROM_SENDER = 0,
  /**特殊资产来自 be 交易的发起账户，即求购 */
  ASSET_FROM_RECIPIENT = 1,
}
```
## BFMeta.BeExchangeSpecialAssetAssetJSON

```typescript
interface BeExchangeSpecialAssetAssetJSON { 
    /**接收资产交换事件附带信息 */
    beExchangeSpecialAsset: BeExchangeSpecialAssetJSON;
}
```
## BFMeta.BeExchangeSpecialAssetJSON

```typescript
interface BeExchangeSpecialAssetJSON { 
    /**发起资产交换的事件签名，128 个字节的 16 进制字符串 */
    transactionSignature: string;
    /**加密密钥生成的签名数组 */
    ciphertextSignature?: AccountSignatureJSON;
    /**资产交换信息 */
    exchangeSpecialAsset: ToExchangeSpecialAssetJSON;
}
```
## BFMeta.GiftAssetAssetJSON

```typescript
interface GiftAssetAssetJSON { 
    /**权益赠送事件附带信息 */
    giftAsset: GiftAssetJSON;
}
```
## BFMeta.GiftAssetJSON

```typescript
interface GiftAssetJSON { 
    /**加密密钥生成的公钥数组 */
    cipherPublicKeys: string[];
    /**赠送的权益所属链名，小写字母组成，3-8 位 */
    sourceChainName: string;
    /**赠送的权益所属链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic: string;
    /**赠送的权益名称，大写字母组成，3-5 个字符 */
    assetType: string;
    /**赠送的权益数量，0-9 组成并且不包含小数点，必须大于0 */
    amount: string;
    /**可被接收的次数，0-9 组成并且不包含小数点，必须大于 0 */
    totalGrabableTimes: number;
    /**开始被签收的区块高度，0-9 组成并且不包含小数点 */
    beginUnfrozenBlockHeight?: number;
    /**接收规则，只能为 0，1 或 2，0 表示平均分配，1 表示根据任意账户的地址的随机分配，2 表示根据接收者列表中账户地址的随机分配 */
    giftDistributionRule: BFMeta.GIFT_DISTRIBUTION_RULE;
}
```
## BFMeta.GIFT_DISTRIBUTION_RULE

```typescript
enum GIFT_DISTRIBUTION_RULE {
  /**平均分配 */
  AVERAGE,
  /**根据任意账户的地址的随机分配法 */
  RANDOM,
  /**根据接收者列表中账户地址的随机分配法
   * 这种规则会确保赠送的资产尽可能的被分配完，并且确保每一个接收账户都有能得到的金额
   */
  RECIPIENT_RANDOM,
};
```
## BFMeta.GrabAssetAssetJSON

```typescript
interface GrabAssetAssetJSON { 
    /**接收权益赠送事件附带信息 */
    grabAsset: GrabAssetJSON;
}
```
## BFMeta.GrabAssetJSON

```typescript
interface GrabAssetJSON { 
    /**赠送事件所在的区块签名，128 个字节的 16 进制字符串 */
    blockSignature: string;
    /**赠送事件的签名，128 个字节的 16 进制字符串 */
    transactionSignature: string;
    /**根据共识规则计算出来的：抢到的金额 */
    amount: string;
    /**用于校验身份的密文签名，如果需要的话 */
    ciphertextSignature?: AccountSignatureJSON;
    /**以下是冗余的字段
     * 都是能从`transactionSignature`中查询出来的，但这个仍然做了存储，是为了确保能够在独立的情况下仍然能够将之渲染出来
     */
    /**礼物配置 */
    giftAsset: GiftAssetJSON;
}
```
## BFMeta.TrustAssetAssetJSON

```typescript
interface TrustAssetAssetJSON { 
    /**见证事件附带信息 */
    trustAsset: TrustAssetJSON;
}
```
## BFMeta.TrustAssetJSON

```typescript
interface TrustAssetJSON { 
    /**见证账户地址数组，base58 编码的 16 进制字符串数组 */
    trustees: string[];
    /**签收时需要的见证人签名数量，0-9 组成，必须大于 0，最大值为指定的受托人数量+2 */
    numberOfSignFor: number;
    /**见证的权益所属链名，小写字母组成，3-8 位 */
    sourceChainName: string;
    /**见证的权益所属链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic: string;
    /**见证的权益名称，大写字母组成，3-5 个字符 */
    assetType: string;
    /**见证的权益数量，0-9 组成并且不包含小数点，必须大于0 */
    amount: string;
}
```
## BFMeta.SignForAssetAssetJSON

```typescript
interface SignForAssetAssetJSON { 
    /**签收见证事件事件附带信息 */
    signForAsset: SignForAssetJSON;
}
```
## BFMeta.SignForAssetJSON

```typescript
interface SignForAssetJSON { 
    /**见证交易的签名，*/
    transactionSignature: string;
    /**见证交易的发起账户地址，base58 编码的 16 进制字符串 */
    trustSenderId: string;
    /**见证交易的接收账户地址，base58 编码的 16 进制字符串 */
    trustRecipientId: string;
    /**见证信息 */
    trustAsset: TrustAssetJSON;
}
```
## BFMeta.EmigrateAssetAssetJSON

```typescript
interface EmigrateAssetAssetJSON { 
    /**权益迁出事件附带信息 */
    emigrateAsset: EmigrateAssetJSON;
}
```
## BFMeta.EmigrateAssetJSON

```typescript
interface EmigrateAssetJSON { 
    /**创世受托人签名 */
    genesisDelegateSignature: AccountSignatureJSON;
    /**迁出的权益所属链名，小写字母组成，3-8 位 */
    sourceChainName: string;
    /**迁出的权益所属链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic: string;
    /**迁出的权益名称，大写字母组成，3-5 个字符 */
    assetType: string;
    /**迁出的权益数量，0-9 组成并且不包含小数点，必须大于0 */
    amount: string;
}
```
## BFMeta.ImmigrateAssetAssetJSON

```typescript
interface ImmigrateAssetAssetJSON { 
    /**权益迁入事件附带信息 */
    immigrateAsset: ImmigrateAssetJSON;
}
```
## BFMeta.ImmigrateAssetJSON

```typescript
interface ImmigrateAssetJSON { 
    /**创世受托人签名 */
    genesisDelegateSignature: AccountSignatureJSON;
    /**完整的主权益迁出事件 */
    emigrateAssetTransaction: EmigrateAssetTransactionJSON;
}
```
## BFMeta.EmigrateAssetTransactionJSON

```typescript
type EmigrateAssetTransactionJSON = TransactionMixJSON<EmigrateAssetAssetJSON, {
    hasRecipientId: false;
}>;
```
## BFMeta.LocationNameAssetJSON

```typescript
interface LocationNameAssetJSON { 
    /**注册/注销的位名事件附带信息 */
    locationName: LocationNameJSON;
}
```
## BFMeta.LocationNameJSON

```typescript
interface LocationNameJSON { 
    /**注册/注销的位名，2-1024 个字符，每级域名最大长度为 128 个字符，一级域名只能是小写字母组成，二级及以上开头及结尾只能由小写字母或数字组成，中间可以包含下划线，根域名必须是本链链名 */
    name: string;
    /**注册/注销的位名来源链名，小写字母组成，3-8 位 */
    sourceChainName: string;
    /**注册/注销的位名来源链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic: string;
    /**操作类型，只能是 0 或 1，0 表示注册位名，1 表示注销位名 */
    operationType: BFMeta.LOCATION_NAME_OPERATION_TYPE;
}
```
## BFMeta.LOCATION_NAME_OPERATION_TYPE

```typescript
enum LOCATION_NAME_OPERATION_TYPE {
  /**注册 lns */
  REGISTRATION,
  /**注销 */
  CANCELLATION,
};
```
## BFMeta.SetLnsManagerAssetJSON

```typescript
interface SetLnsManagerAssetJSON { 
    /**设置位名管理员事件附带信息 */
    lnsManager: SetLnsManagerJSON;
}
```
## BFMeta.SetLnsManagerJSON

```typescript
interface SetLnsManagerJSON { 
    /**位名，2-1024 个字符，每级域名最大长度为 128 个字符，一级域名只能是小写字母组成，二级及以上开头及结尾只能由小写字母或数字组成，中间可以包含下划线，根域名必须是本链链名 */
    name: string;
    /**位名来源链名，小写字母组成，3-8 位 */
    sourceChainName: string;
    /**位名来源链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic: string;
}
```
## BFMeta.SetLnsRecordValueAssetJSON

```typescript
interface SetLnsRecordValueAssetJSON { 
    /**设置位名解析值事件附带信息 */
    lnsRecordValue: SetLnsRecordValueJSON;
}
```
## BFMeta.SetLnsRecordValueJSON

```typescript
interface SetLnsRecordValueJSON { 
    /**位名，2-1024 个字符，每级域名最大长度为 128 个字符，一级域名只能是小写字母组成，二级及以上开头及结尾只能由小写字母或数字组成，中间可以包含下划线，根域名必须是本链链名 */
    name: string;
    /**位名来源链名，小写字母组成，3-8 位 */
    sourceChainName: string;
    /**位名来源链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic: string;
    /**操作类型，只能为 0 或 1 或 2，0 表示添加，1 表示删除，2 表示更新 */
    operationType: BFMeta.RECORD_OPERATION_TYPE;
    /**增加的解析值，解析值的类型只能为 A 或 AAAA 或 LNG_LAT 或 BLOCK_CHAIN_ACCOUNT_ADDRESS，A 表示 ipV4，AAAA 表示 ipV6，LNG_LAT 表示经纬度，BLOCK_CHAIN_ACCOUNT_ADDRESS 表示链上账户地址，下划线前面为解析值类型，下划线后面为解析值，可选，操作类型为 0 或 2 时必填 */
    addRecord?: LocationNameRecordJSON;
    /**删除的解析值，解析值的类型只能为 A 或 AAAA 或 LNG_LAT 或 BLOCK_CHAIN_ACCOUNT_ADDRESS，A 表示 ipV4，AAAA 表示 ipV6，LNG_LAT 表示经纬度，BLOCK_CHAIN_ACCOUNT_ADDRESS 表示链上账户地址，下划线前面为解析值类型，下划线后面为解析值，可选，操作类型为 1 或 2 时必填 */
    deleteRecord?: LocationNameRecordJSON;
}
```
## BFMeta.RECORD_OPERATION_TYPE

```typescript
enum RECORD_OPERATION_TYPE {
  /**添加解析值 */
  ADD,
  /**删除解析值 */
  DELETE,
  /**更新解析值 */
  UPDATE,
};
```
## BFMeta.LocationNameRecordJSON

```typescript
interface LocationNameRecordJSON { 
    /**解析值类型 */
    recordType: BFMeta.RECORD_TYPE;
    /**解析值 */
    recordValue: string;
}
```
## BFMeta.RECORD_TYPE

```typescript
enum RECORD_TYPE {
  /**IPV4解析 */
  IPV4 = "A",
  /**IPV6解析 */
  IPV6 = "AAAA",
  /**经纬度解析 */
  LNG_LAT = "LNG_LAT",
  /**账户地址解析 */
  ADDRESSV1 = "BLOCK_CHAIN_ACCOUNT_ADDRESS",
};
```
## BFMeta.RegisterChainAssetJSON

```typescript
interface RegisterChainAssetJSON { 
    registerChain: RegisterChainJSON;
}
```
## BFMeta.RegisterChainJSON

```typescript
interface RegisterChainJSON { 
    genesisBlock: BlockJSON<GenesisBlockAssetJSON>;
}
```
## BFMeta.BlockJSON

```typescript
interface BlockJSON<AssetJSON extends object = object> extends BlockWithoutTransactionJSON<AssetJSON> { 
    /**事件 */
    transactions: TransactionInBlockJSON[];
}
```
## BFMeta.BlockWithoutTransactionJSON

```typescript
interface BlockWithoutTransactionJSON<AssetJSON extends object = object> { 
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
    /**锻造者gong'yao */
    generatorPublicKey: string;
    /**锻造者的安全公钥 */
    generatorSecondPublicKey?: string;
    /**锻造者权益 */
    generatorEquity: string;
    /**区块事件数量 */
    numberOfTransactions: number;
    /**区块事件摘要 */
    payloadHash: string;
    /**区块事件摘要长度 */
    payloadLength: number;
    /**前块签名 */
    previousBlockSignature: string;
    /**总发生资产量 */
    totalAmount: string;
    /**总发生手续费 */
    totalFee: string;
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
    asset: AssetJSON;
    /**区块统计信息 */
    statisticInfo: StatisticInfoJSON;
    /**锻造者掉线列表 */
    roundOfflineGeneratersHashMap: RoundOfflineGeneratersHashMap;
}
```
## BFMeta.GenesisBlockAssetJSON

```typescript
interface GenesisBlockAssetJSON { 
    /**创世块附带信息 */
    genesisAsset: GenesisAssetJSON;
}
```
## BFMeta.GenesisAssetJSON

```typescript
interface GenesisAssetJSON extends RoundDelegateJSON { 
    /**链名 */
    chainName: string;
    /**链主权益名 */
    assetType: string;
    /**链网络标识符 */
    magic: string;
    /**链网络类型，只能是 'b' 或 'c'，b 为正式网络，c 为测试网络 */
    bnid: string;
    /**链创世时间 */
    beginEpochTime: number;
    /**链创世位名 */
    genesisLocationName: string;
    /**链创世账户初始持有的主权益量 */
    genesisAmount: string;
    /**链事件每字节需要支付的最小手续费 */
    minTransactionFeePerByte: FractionJSON;
    /**链上事件体最大字节数 */
    maxTransactionSize: number;
    /**链上区块体最大字节数 */
    maxBlockSize: number;
    /**链上区块体最大处理的事件 tps */
    maxTPSPerBlock: number;
    /**删除分叉时至少需要落后的高度 */
    consessusBeforeSyncBlockDiff: number;
    /**每轮能处理的注册受托人数量 */
    maxDelegateTxsPerRound: number;
    /**权益赠送事件最大可抢次数 */
    maxGrabTimesOfGiftAsset: number;
    /**发行权益的账户最小持有的链主权益数量 */
    issueAssetMinChainAsset: string;
    /**注册创世块的账户最小持有的主权益数量 */
    registerChainMinChainAsset: string;
    /**最大的过期区块间隔数量 */
    maxApplyAndConfirmedBlockHeightDiff: number;
    /**每轮的区块数量 */
    blockPerRound: number;
    /**创世受托人数量 */
    delegates: number;
    /**是否允许受托人连续参与竞选 */
    whetherToAllowDelegateContinusElections: boolean;
    /**区块间隔 */
    forgeInterval: number;
    /**奖励分配比例，JSON 对象 */
    rewardPercent: RewardPercentJSON;
    /**区块链端口号，JSON 对象 */
    ports: PortsJSON;
    /**区块奖励，JSON 对象 */
    rewardPerBlock: RewardPerBlockJSON;
    /**账户参与度权重比，JSON 对象 */
    accountParticipationWeightRatio: AccountParticipationWeightRatioJSON;
    /**区块参与度权重比，JSON 对象 */
    blockParticipationWeightRatio: BlockParticipationWeightRatioJSON;
    /**构建tpow的难度系数 */
    averageComputingPower: number;
    /**tpow豁免的区块高度 */
    tpowOfWorkExemptionBlocks: number;
    /**tpow配置，JSON对象 */
    transactionPowOfWorkConfig: TransactionPowOfWorkConfigJSON;
}
```
## BFMeta.RoundDelegateJSON

```typescript
interface RoundDelegateJSON { 
    /**下一轮的打块账户列表 */
    nextRoundDelegates: NextRoundDelegateJSON[];
    /**新注册的受托人 */
    newDelegates: string[];
    /**上一轮投票账户中的最大轮末主权益量 */
    maxBeginBalance: string;
    /**上一轮投票账户中最大的事件量 */
    maxTxCount: number;
    /**上一轮投票账户的最大轮末主权益量和上一轮投票账户的最大事件的比 */
    rate: string;
}
```
## BFMeta.NextRoundDelegateJSON

```typescript
interface NextRoundDelegateJSON { 
    /**受托人账户地址 */
    address: string;
    /**受托人上一轮获得的权益 */
    equity: string;
}
```
## BFMeta.FractionJSON

```typescript
interface FractionJSON<T extends number | bigint | string = number> { 
    /**分子 */
    numerator: T;
    /**分母 */
    denominator: T;
}
```
## BFMeta.RewardPercentJSON

```typescript
interface RewardPercentJSON { 
    /**投票占区块总奖励的百分比 */
    votePercent: FractionJSON;
    /**打块占区块总奖励的百分比 */
    forgePercent: FractionJSON;
}
```
## BFMeta.PortsJSON

```typescript
interface PortsJSON { 
    /**共识端口号 */
    port: number;
    /**节点扫描端口号 */
    scan_peer_port: number;
}
```
## BFMeta.RewardPerBlockJSON

```typescript
interface RewardPerBlockJSON { 
    /**奖励变动的区块里程 */
    readonly heights: number[];
    /**每个区块里程对应奖励的资产数量 */
    readonly rewards: string[];
}
```
## BFMeta.AccountParticipationWeightRatioJSON

```typescript
interface AccountParticipationWeightRatioJSON { 
    /**账户上一轮末持有的主权益量的权重 */
    balanceWeight: number;
    /**账户上一轮事件量的权重 */
    numberOfTransactionsWeight: number;
}
```
## BFMeta.BlockParticipationWeightRatioJSON

```typescript
interface BlockParticipationWeightRatioJSON { 
    /**区块打包的主权益量的权重 */
    balanceWeight: number;
    /**区块打包的事件量的权重 */
    numberOfTransactionsWeight: number;
}
```
## BFMeta.TransactionPowOfWorkConfigJSON

```typescript
interface TransactionPowOfWorkConfigJSON { 
    /**链的 pow 难度增长系数 */
    growthFactor: FractionJSON<string>;
    /**链的 pow 计算参与比率 */
    participationRatio: FractionJSON;
}
```
## BFMeta.StatisticInfoJSON

```typescript
interface StatisticInfoJSON { 
    /**区块打包的事件的总手续费 */
    totalFee: string;
    /**区块打包的事件的总权益值（不区分权益类型） */
    totalAsset: string;
    /**区块打包的事件的总主权益值 */
    totalChainAsset: string;
    /**区块打包的事件涉及的总账户数 */
    totalAccount: number;
    /**区块打包的事件权益类型统计明细，JSON 对象 */
    assetStatisticHashMap: {
        [index: number]: AssetStatisticJSON;
    };
}
```
## BFMeta.AssetStatisticJSON

```typescript
interface AssetStatisticJSON extends AssetInfoJSON { 
    /**权益在块内的索引 */
    index: number;
    /**区块打包的事件类型统计明细，JSON 对象 */
    typeStatisticHashMap: {
        [baseType: number]: CountAndAmountStatisticJSON;
    };
    /**区块打包的事件权益类型统计，JSON 对象 */
    total: CountAndAmountStatisticJSON;
}
```
## BFMeta.AssetInfoJSON

```typescript
type AssetInfoJSON = {
    /**权益所属的链网络标识，大写字母或数字组成，5 个字符，最后一位是校验位 */
    magic: string;
    /**权益名，大写字母组成，3-5 个字符 */
    assetType: string;
};
```
## BFMeta.CountAndAmountStatisticJSON

```typescript
interface CountAndAmountStatisticJSON { 
    /**变动总值，某个账户的增加或者减少都会有影响 */
    changeAmount: string;
    /**变动总次数，某个账户的增加或者减少都会有影响 */
    changeCount: number;
    /**资产迁移总量 */
    moveAmount: string;
    /**交易量 */
    transactionCount: number;
}
```
## BFMeta.RoundOfflineGeneratersHashMap

```typescript
interface RoundOfflineGeneratersHashMap { 
    /**
     * 使用逗号分隔的地址
     * address,address */
    [roundOffset: string]: string;
}
```
## BFMeta.TransactionInBlockJSON

```typescript
interface TransactionInBlockJSON<T extends TransactionJSON = TransactionJSON> extends SomeTransactionJSON<T> { 
    /**事件在区块内的索引 */
    index: number;
    /**区块高度 */
    height: number;
    /**事件发起账户的事件量 */
    numberOfSenderTransactions: number;
    /**事件涉及的账户权益变动信息 */
    transactionAssetChanges: TransactionAssetChangeJSON[];
    /**权益销毁前权益的最新信息 */
    assetPrealnum?: AssetPrealnumJSON;
    /**区块锻造者的签名 */
    signature: string;
    /**区块锻造者的安全签名 */
    signSignature?: string;
}
```
## BFMeta.SomeTransactionJSON

```typescript
interface SomeTransactionJSON<T extends TransactionJSON> { 
    transaction: T;
}
```
## BFMeta.TransactionAssetChangeJSON

```typescript
interface TransactionAssetChangeJSON { 
    /**账户类型 */
    accountType: number;
    /**权益在块内的索引 */
    assetTypes: number;
    /**账户最新的权益持有量 */
    assetBalance: string;
}
```
## BFMeta.AssetPrealnumJSON

```typescript
type AssetPrealnumJSON = {
    /**剩余的权益数量 */
    remainAssetPrealnum: string;
    /**冻结的主权益数量 */
    frozenMainAssetPrealnum: string;
};
```
## BFMetaPC.Config.ConfigRevisable

```typescript
interface ConfigRevisable { 
    /**进程参数配置  */
    coreForProcess: {
        /**是否使用配置的核心数 */
        forceUseConfig: boolean;
        /**处理交易的进程数 */
        coreNumForDealTransaction: number;
        /**处理账户的进程数 */
        coreNumForMemInfo: number;
        /**处理未处理交易的进程数 */
        coreNumForUntreatedTrs: number;
    };
    /**事件参数 */
    transactionConfig: {
        /**是否接收投票 */
        receiveVoteEnable: boolean;
        /**每轮可处理的最大投票交易数 */
        maxTransactionLimitForVote: number;
        /**每个区块的交易上限 */
        transactionsLimitPerBlock: number;
        /**矿机最小手续费 */
        minFeePerByte: {
            /**分子 */
            numerator: number;
            /**分母 */
            denominator: number;
        };
        /**事件的最大手续费，单位: B */
        maxFee: string;
        /**自动投票配置 */
        autoVote: AutoVoteModel;
    };
    /**文件日志配置 */
    logConfig: {
        /**控制台输出等级: "error" | "warn" | "info"  */
        consoleLogLevel: string;
        /**日志等级: "error" | "warn" | "info"  */
        fileLogLevel: string;
        /**日志文件大小限制(mb) */
        fileLogLimit: number;
        /**备份日志时分割的份数 */
        fileLogBackup: number;
        /**是否使用日期进行分割 */
        fileLogDateExpire: boolean;
        /**日志保存天数 */
        fileLogDaysToKeep: number;
    };
    /**启动参数 */
    startConfig: {
        /**开启是否打块，true：开始打块，false：停止打块 */
        generateBlockEnable: boolean;
        /**区块标记 */
        remark: string;
        /**使用关键检查点启动 */
        useCheckPoint: boolean;
        /**关键检查点保留轮次数 */
        numberOfReservedCheckPoint: number;
        /**节点初始连接ip */
        peers: string[];
        /**最大连接数 */
        maxChannelNumber: number;
    };
    /**网络配置 */
    networkConfig: {
        /**是否开启grpc */
        grpcEnable: boolean;
    };
    /**流控配置 */
    flowControlConfig: {
        /**流量控制 */
        requestLimit: {
            /**是否开启 */
            enable: boolean;
            /**访问次数总上限 */
            count: number;
            /**各api访问次数上限 */
            apiRequestInterface: {
                [apiName: string]: number;
            };
            /**重置间隔 */
            time: number;
        };
        /**总流量限制 */
        flowRequestLimit: {
            enable: boolean;
            /**总流量上限 */
            count: number;
            time: number;
        };
    };
    /**磁盘监控 */
    diskMonitorConfig: {
        enable: boolean;
        /**监控磁盘路径及配置 */
        clearPath: string[];
        /**当磁盘空间不足x时清理 */
        clearWhenFreeSpaceLowerThan: number;
        /**检查时间间隔 */
        checkInterval: number;
        /**清理后缀为xxx的文件 */
        clearWithSuffix?: string[];
        /**后缀为xxx的文件不清理 */
        noClearWithSuffix?: string[];
        /**最后修改时间大于n毫秒的文件不清理 */
        noClearWithLastModifyTimeGreaterThen?: number;
    };
    /**通知配置 */
    noticeConfig: {
        enable: boolean;
        /**邮箱发送条件 */
        sendCondition: {
            usageCPU: number;
            usageDisk: number;
            usageMemory: number;
            missedBlocks: number;
        };
        /**邮箱发送间隔时间 */
        sendIntervalTime: number;
    };
    /**服务市场配置 */
    serviceConfig: {
        /**是否开启服务市场 */
        serverMarketEnable: boolean;
        userMaxUploadNumber: number;
        uploadTokenExpireTime: number;
        remainTimeToClearUserSessoin: number;
    };
}
```
## BFMetaPC.Config.AutoVoteModel

```typescript
type AutoVoteModel = {
    /**是否开启自动投票 */
    enable: boolean;
    /**是否使用配置手续费 */
    useConfigFee: boolean;
    /**自动投票手续费 */
    fee: string;
    /**是否优先保证推荐人个数 */
    priorRecommendedNumber: boolean;
    /**选出的推荐人数量上限 */
    maxNumberOfRecommended: number;
    /**选取的区块范围, 最近的 100 轮 */
    numberOfRounds: number;
    /**在线率占比 */
    productivityPercent: number;
    /**打块数量占比 */
    forgedBlocksPercent: number;
    /**打包交易数量占比 */
    applyTxPercent: number;
    /**上一轮的得票率占比 */
    votePercent: number;
    /**新受托人占比 */
    newDelegatePercent: number;
    /**最小可被推荐得账户在线率 */
    minBeSelectProductivity: number;
};
```
## BFMetaPC.Config.ConfigReadable

```typescript
interface ConfigReadable extends ConfigRevisable { 
     /**链端口号 */
    chainPort: ConfigPort;
}
```
## BFMetaPC.Config.ConfigPort

```typescript
interface ConfigPort { 
    /**turnsever服务器使用端口 */
    turnserver: number;
    /**数据库使用的端口 */
    mongodb: number;
    /**节点管理使用端口 */
    system_websocket_port: number;
    /**进程锁使用端口 */
    clusterLock: number;
    /**grpc使用端口 */
    grpc: number;
    /**升级服务使用端口 */
    update_service_port: number;
    /**comlink使用端口 */
    comlink_port: number;
}
```
## BFMeta.TransactionMixJSON

```typescript
type TransactionMixJSON<AssetJSON extends object = object, Opts extends TransactionOptions = {}> = Opts["hasRecipientId"] extends true ? Omit<TransactionJSON<AssetJSON>, "recipientId"> & {
    recipientId: string;
} : Opts["hasRecipientId"] extends false ? Omit<TransactionJSON<AssetJSON>, "recipientId"> & {
    recipientId: undefined;
} : TransactionJSON<AssetJSON>;
```
