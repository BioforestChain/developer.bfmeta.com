# Type definition
## BFMeta.TransactionJSON

```typescript
interface TransactionJSON <AssetJSON extends object = object> { 
    /**Event version number */
    version: number;
    /**Event type */
    type: string;
    /**The address of the account that initiated the event, base58-encoded hexadecimal string */
    senderId: string;
    /**The public key of the account that initiated the event, 128-byte hexadecimal string */
    senderPublicKey: string;
    /**The security public key of the account that initiated the event, 128-byte hexadecimal string */
    senderSecondPublicKey?: string;
    /**The receiving account address of the event, base58-encoded hexadecimal string */
    recipientId?: string;
    /**The receiving range type of the event */
    rangeType: BFMeta.RANGE_TYPE;
    /**The receiving range of the event */
    range: string[];
    /**Fee of the event */
    fee: string;
    /**Timestamp of the event */
    timestamp: number;
    /**dappid that belongs to the event*/
    dappid?: string;
    /**Bit name that blongs to the event */
    lns?: string;
    /**The source IP of the event, IPv4 or IPv6, does not include the header and the end (for example: 127.0.0.1), the default is empty */
    sourceIP?: string;
    /**The source chain network identifier of the event */
    fromMagic: string;
    /**The to-chain network identifier of the event */
    toMagic: string;
    /**Initiation height of the event */
    applyBlockHeight: number;
    /**The effective height of the event */
    effectiveBlockHeight: number;
    /**Signature of the event */
    signature: string;
    /**Security signature of the event */
    signSignature?: string;
    /**Remark of the event */
    remark: {
        [key: string]: string;
    };
    /**Actual event part */
    asset: AssetJSON;
    /**The index object of the event */
    storage?: TransactionStorageJSON;
    /**The index key of the event, provide the field name used for additional queries */
    storageKey?: TransactionStorageJSON["key"];
    /**The index value of the event, provides the field value used for additional queries */
    storageValue?: TransactionStorageJSON["value"];
    /**Event pow noise */
    nonce: number;
}
```
## BFMeta.RANGE_TYPE

```typescript
enum RANGE_TYPE {
  /**Unlimited range */
  EMPTY = 0,
  /**Multiple addresses */
  MULTI_ADDRESS = 1,
  /**DAppid range */
  MULTI_DAPPID = 2,
  /**Chain domain name range */
  MULTI_LOCATION_NAME = 4,
}
```
## BFMeta.TransactionStorageJSON

```typescript
interface TransactionStorageJSON { 
    /**The index key of the event, provide the field name used for additional queries */
    key: string;
    /**The index value of the event, provide the field value used for additional queries */
    value: string;
}
```
## BFMetaPC.MemInfoModel.AccountInfoAndAsset

```typescript
type AccountInfoAndAsset = {
    /**Account basic information，JSON object */
    accountInfo: AccountInfoModel;
    /**Account equity information，JSON object */
    accountAssets: AccountAssetsModel;
};
```
## BFMetaPC.MemInfoModel.AccountInfoModel

```typescript
type AccountInfoModel = {
    /**Account address */
    address: string;
    /**Account public key */
    publicKey?: string;
    /**Account security public key */
    secondPublicKey?: string;
    /**Account alias */
    username?: string;
    /**Account status：0 means normal 1 means frozen */
    accountStatus: number;
    /**Whether it is a trustee：false means normal true means trustee */
    isDelegate: boolean;
    /**Whether to receive votes：false means not to receive votes true means receiving cotes */
    isAcceptVote: boolean;
    /**The number of blocks that have been forged */
    producedblocks: number;
    /**The number of dropped blocks, the number of times that the corresponding block was not forged due to various reasons after being selected as the trustee and reaching the forging block */
    missedblocks: number;
    /**The number of accumulated event */
    numberOfTransactions: number;
    /**The information of obtained equity */
    voteInfo: VoteInfoModel;
    /**Equity information of participating in governance voting */
    equityInfo: EquityInfoModel;
    /**Information on the amount of equity and event at the end of the previous round, the round information of the last round, if it is currently 600, which means it is currently the 11th round, and this value refers to the 10th round */
    lastRoundInfo: RoundInfoModel;
    /**Account change block height */
    height: number;
    /**Account online rate */
    productivity?: number;
};
```
## BFMetaPC.MemInfoModel.VoteInfoModel

```typescript
type VoteInfoModel = {
    /**The round that equity belongs to */
    round: number;
    /**The number of obtained equity */
    vote: bigint;
};
```
## BFMetaPC.MemInfoModel.EquityInfoModel

```typescript
type EquityInfoModel = {
    /**The round that equity belongs to */
    round: number;
    /**Residual value of equity  */
    equity: bigint;
    /**Initial value of equity*/
    fixedEquity: bigint;
};
```
## BFMetaPC.MemInfoModel.RoundInfoModel

```typescript
type RoundInfoModel = {
    /**The round it belongs to */
    round: number;
    /**The amount of equity held at the end of the round */
    assetNumber: bigint;
    /**The number of events at the end of the round */
    txCount: number;
};
```
## BFMetaPC.MemInfoModel.AccountAssetsModel

```typescript
type AccountAssetsModel = {
    /**Account address */
    address: string;
    /**Account public key */
    publicKey?: string;
    /**Account equity */
    assets: AssetsModel;
    /**Accumulated fees */
    paidFee: bigint;
    /**Accumulated voting income*/
    votingRewards: bigint;
    /**Accumulated blocking income */
    forgingRewards: bigint;
    /**Account change block height */
    height: number;
};
```
## BFMetaPC.MemInfoModel.AssetsModel

```typescript
type AssetsModel = {
    /**Equity information of a chain */
    [sourceChainMagic: string]: {
        /**Certain equity information */
        [assetType: string]: AccountAssetSubModel;
    };
};
```
## BFMetaPC.MemInfoModel.AccountAssetSubModel

```typescript
type AccountAssetSubModel = {
    /**The network identifier of the chain that the equity belongs to */
    sourceChainMagic: string;
    /**Equity name */
    assetType: string;
    /**Name of the main chain that the equity belongs to*/
    sourceChainName?: string;
    /**The number of equity */
    assetNumber: bigint;
    /**The amount of equity and event at the end of the previous two round*/
    penultimateRoundInfo?: RoundInfoModel;
    /**The amount of equity and event at the end of the previous round */
    lastRoundInfo?: RoundInfoModel;
};
```
## BFMetaPC.ApiRequest.TRANSACTION.TrCommonParam

```typescript
interface TrCommonParam { 
    /**The public key of the initiating account */
    publicKey: string;
    /**The security public key of the initiating account  */
    secondPublicKey?: string;
    /**The receiving account address of the event, base58-encoded hexadecimal string */
    recipientId?: string;
    /**The receiving range type of the event can only be one of 0, 1, 2, 4, 0 means that the operating range is not limited, 1 means that only the specified account address can operate on this event, 2 means that only specified dappid can operate on this event, 4 means that only the specified bit name can operate on this event, the default is 0*/
    rangeType?: number;
    /**Event receiving range, when rangeType is 0, no data can be filled in, when rangeType is 1, only account address can be filled in, when rangeType is 2, only dappid can be filled in, when rangeType is 4, only bit name can be filled in, the default is null*/
    range?: string[];
    /**Fee of the event */
    fee: string;
    /**The initiating height of the event, composed of 0-9 and no dot included, optional, the latest height of the current blockchain is used by default */
    applyBlockHeight: number;
    /**Event remarks information, json object */
    remark?: {
        [key: string]: string;
    };
    /**The dappid that the event belongs to, uppercase letters or numbers, 8 characters, the default is null */
    dappid?: string;
    /**The bit name to which the event belongs, 2-1024 characters, the maximum length of each level name is 128 characters, the first level bit name can only be composed of lowercase letters, and the beginning and end of the second level and above can only be composed of lowercase letters or numbers, and can contain an underscore, the root name must be the chain name of the chain, optional, the default is null */
    lns?: string;
    /**The source IP of the event, IPv4 or IPv6, the header and the end are not included (for example: 127.0.0.1), the default is null */
    sourceIP?: string;
    /**The source chain network identifier of the event, composed of uppercase letters or numbers, 5 characters, and the magic of the genesis block is used by default */
    fromMagic?: string;
    /**The to-chain network identifier of the event, composed of uppercase letters or numbers, 5 characters, the magic of the genesis block is used by default */
    toMagic?: string;
    /**The expired block interval of the event, the maximum expiration time parameter of the genesis block is used by default, composed of 0-9 and decimal points are not included */
    numberOfEffectiveBlocks?: number;
}
```

## BFMeta.TransferAssetAssetJSON

```typescript
interface TransferAssetAssetJSON { 
    /**Information attached to the equity transfer event */
    transferAsset: TransferAssetJSON;
}
```
## BFMeta.TransferAssetJSON

```typescript
interface TransferAssetJSON { 
    /**The chain name to which the transferred equity belongs, composed of lowercase letters, 3-8 digits */
    sourceChainName: string;
    /**The chain network identifier to which the transferred equity belongs, composed of capital letter or number, 5 characters, the last bit is the check bit */
    sourceChainMagic: string;
    /**Name of the transferred equity, composed of uppercase letters, 3-5 characters */
    assetType: string;
    /**The amount of transferred equity, composed of 0-9 and decimal points are not included, must be greater than 0 */
    amount: string;
}
```
## BFMeta.SignatureAssetJSON

```typescript
interface SignatureAssetJSON { 
    /**Information attached to the security password setting event */
    signature: SignatureJSON;
}
```
## BFMeta.SignatureJSON

```typescript
interface SignatureJSON { 
    /**The public key generated by the security key, 128-byte hexadecimal string */
    publicKey: string;
}
```
## BFMeta.UsernameAssetJSON

```typescript
interface UsernameAssetJSON { 
    /**Information attached to the address name event */
    username: UsernameJSON;
}
```
## BFMeta.UsernameJSON

```typescript
interface UsernameJSON { 
    /**User name string, composed of uppercase and lowercase letters, numbers and underscores, 1-20 characters, the chain name cannot be included */
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
    /**Information attached to the voting event */
    vote: VoteJSON;
}
```
## BFMeta.VoteJSON

```typescript
interface VoteJSON { 
    /**The number of equity voted, composed of 0-9 and the decimal point is not included, allowed to be 0  */
    equity: string;
}
```
## BFMeta.DAppAssetJSON

```typescript
interface DAppAssetJSON { 
    /**Information attached to dapp release event */
    dapp: DAppJSON;
}
```
## BFMeta.DAppJSON

```typescript
interface DAppJSON { 
    /**The chain name that dappid belongs to, composed of lowercase letters, 3-8 digits */
    sourceChainName: string;
    /**The chain network identifier that dappid belongs to, composed of uppercase letters or numbers, 5 characters, and the last digit is the check digit */
    sourceChainMagic: string;
    /**dappid，composed of uppercase letters or numbers, 8 characters, the last digit is the check digit */
    dappid: string;
    /**The type of dappid, can only be 0 or 1; 0 means this dappid is paid, 1 means this dappid is free */
    type: BFMeta.DAPP_TYPE;
    /**Purchase the equity of using dappid */
    purchaseAsset?: DAppPurchaseAssetJSON;
}
```
## BFMeta.DAPP_TYPE

```typescript
enum DAPP_TYPE {
  /**Paid application */
  PAID_APP = 0, // "PAID_APP",
  /**free application*/
  FREE_APP = 1, // "FREE_APP",
}
```
## BFMeta.DAppPurchaseAssetJSON

```typescript
interface DAppPurchaseAssetJSON { 
    /**The chain name that the equity of purchasing dappid use right belongs to, composed of lowercase letters, 3-8 digits */
    sourceChainName: string;
    /**The chain network identifier that the DAPPID payment of purchasing dappid use right belongs to, composed of uppercase letters or numbers, 5 characters, the last digit is the check digit */
    sourceChainMagic: string;
    /**The equity name of purchasing dappid use right, composed of uppercase letters, 3-5 characters */
    assetType: string;
    /**The number of equity required to purchase dappid use right(if dappid is a paid application, it must be carried, if it is a free application, it does not need to be carried), composed of 0-9 and the decimal points are not included, must be greater than 0 */
    amount: string;
}
```
## BFMeta.DAppPurchasingAssetJSON

```typescript
interface DAppPurchasingAssetJSON { 
    /**Information attached to dapp purchase event */
    dappPurchasing: DAppPurchasingJSON;
}
```
## BFMeta.DAppPurchasingJSON

```typescript
interface DAppPurchasingJSON { 
    /**Purchased dapp information */
    dappAsset: DAppJSON;
}
```
## BFMeta.MarkAssetJSON

```typescript
interface MarkAssetJSON { 
    /**Information attached to the attestation event */
    mark: MarkJSON;
}
```
## BFMeta.MarkJSON

```typescript
interface MarkJSON { 
    /**Attestation content, is any string */
    content: string;
    /**Attestation type, is any string, used for distinguish attestation */
    action: string;
    /**Dapp information used by the attestation event */
    dapp: DAppJSON;
}
```
## BFMeta.IssueAssetAssetJSON

```typescript
interface IssueAssetAssetJSON { 
    /**Information attached to the issue of equity event */
    issueAsset: IssueAssetJSON;
}
```
## BFMeta.IssueAssetJSON

```typescript
interface IssueAssetJSON { 
    /**The chain name to which the equity belongs, composed of lowercase letters, 3-8 digits */
    sourceChainName: string;
    /**The chain network identifier to which the equity belongs, composed of uppercase letters or numbers, 5 characters, the last digit is the check digit  */
    sourceChainMagic: string;
    /**Equity name, composed of uppercase letters, 3-5 characters */
    assetType: string;
    /**Total number of new equity issued, the number of equity is composed of ten numbers from 0-9, the number of equity does not include decimal points and must be greater than 0 */
    expectedIssuedAssets: string;
}
```
## BFMeta.DestoryAssetAssetJSON

```typescript
interface DestoryAssetAssetJSON { 
    /**Information attached to the event of equity destruction */
    destoryAsset: DestoryAssetJSON;
}
```
## BFMeta.DestoryAssetJSON

```typescript
interface DestoryAssetJSON { 
    /**The chain name to which the destroyed equity belongs, composed of lowercase letters, 3-8 digits */
    sourceChainName: string;
    /**The chain network identifier to which the destroyed equity belongs, composed of uppercase letters or numbers, 5 characters, the last digit is the check digit */
    sourceChainMagic: string;
    /**Destroyed equity name, composed of uppercase letters, 3-5 characters */
    assetType: string;
    /**The number of destroyed equity, composed of 0-9 and the decimal points are not included, must be greater than 0 */
    amount: string;
}
```
## BFMeta.ToExchangeAssetAssetJSON

```typescript
interface ToExchangeAssetAssetJSON { 
    /**Information attached to the event of initiating equity exchange */
    toExchangeAsset: ToExchangeAssetJSON;
}
```
## BFMeta.ToExchangeAssetJSON

```typescript
interface ToExchangeAssetJSON { 
    /**The public key array generated by the encryption key */
    cipherPublicKeys: string[];
    /**The source chain network identifier of the equity used for exchange, composed of uppercase letters or numbers, 5 characters, the last digit is the check digit */
    toExchangeSource: string;
    /**The source chain network identifier of the equity being exchanged, composed of uppercase letters or numbers, 5 characters, the last digit is the check digit */
    beExchangeSource: string;
    /**The source chain name of the equity used for exchange, composed of lowercase letters, 3-8 digits */
    toExchangeChainName: string;
    /**The source chain name of the equity source being exchanged, composed of lowercase letters, 3-8 digits */
    beExchangeChainName: string;
    /**The name of the equity used for exchange, composed of uppercase letters, 3-5 characters */
    toExchangeAsset: string;
    /**The name of the equity being exchanged, composed of uppercase letters, 3-5 characters */
    beExchangeAsset: string;
    /**The number of equity used for exchange, composed of 0-9 and the decimal points are not included, must be greater than 0 */
    toExchangeNumber: string;
    /**The exchange ratio of equity */
    exchangeRate: BFMeta.RateJSON<string>;
}
```
## BFMeta.RateJSON

```typescript
interface RateJSON<T extends number | bigint | string = number> { 
    /**Front weight */
    prevWeight: T;
    /**Back weight */
    nextWeight: T;
}
```
## BFMeta.BeExchangeAssetAssetJSON

```typescript
interface BeExchangeAssetAssetJSON { 
    /**Information attached to the event of receiving equity exchange */
    beExchangeAsset: BeExchangeAssetJSON;
}
```
## BFMeta.BeExchangeAssetJSON

```typescript
interface BeExchangeAssetJSON { 
    /**The signature of the event that initiated the equity exchange, 128-byte hexadecimal string */
    transactionSignature: string;
    /**Signature array generated by encryption key */
    ciphertextSignature?: AccountSignatureJSON;
    /**The number of equity used for exchange. The number of equity is composed of ten numbers from 0-9, the number of equity does not include decimal points and must be greater than 0 */
    toExchangeNumber: string;
    /**The number of equity obtained by the exchange, the number of equity is composed of a total of ten numbers from 0-9, the number of equity does not include the decimal point and must be greater than 0 */
    beExchangeNumber: string;
    /**Information about equity exchange */
    exchangeAsset: ToExchangeAssetJSON;
}
```
## BFMeta.AccountSignatureJSON

```typescript
interface AccountSignatureJSON { 
    /**Public key generated by account key */
    publicKey: string;
    /**Signature generated by account public key */
    signature: string;
    /**Public key generated by account security key */
    secondPublicKey?: string;
    /**Signature generated by account security public key */
    signSignature?: string;
}
```
## BFMeta.ToExchangeSpecialAssetAssetJSON

```typescript
interface ToExchangeSpecialAssetAssetJSON { 
    /**Information attached to the event of initiating asset exchange */
    toExchangeSpecialAsset: ToExchangeSpecialAssetJSON;
}
```
## BFMeta.ToExchangeSpecialAssetJSON

```typescript
interface ToExchangeSpecialAssetJSON { 
    /**The public key array generated by the encryption key */
    cipherPublicKeys: string[];
    /**The source chain network identifier of the equity/asset used for exchange, composed of uppercase letters or numbers, 5 characters, the last digit is the check digit */
    toExchangeSource: string;
    /**The source chain network identifier of the asset/equity being exchanged, composed of uppercase letters or numbers, 5 characters, the last digit is the check digit */
    beExchangeSource: string;
    /**The source chain name of the equity/asset used for exchange, composed of lowercase letters, 3-8 digits */
    toExchangeChainName: string;
    /**The source chain name of the asset/equity being exchanged, composed of lowercase letters, 3-8 digits */
    beExchangeChainName: string;
    /**The equity/asset name used for exchange, composed of uppercase letters, 3-5 characters */
    toExchangeAsset: string;
    /**The name of the asset/equity being exchanged, composed of uppercase letters, 3-5 characters */
    beExchangeAsset: string;
    /**The number of the equity used for exchange or the equity obtained by exchange, the number of equity is composed of ten numbers from 0-9, the number of equity does not include decimal points and must be greater than 0 */
    exchangeNumber: string;
    /**The type of asset, can only be 0 or 1, 0 indicates dappid, 1 indicates bit name */
    exchangeAssetType: BFMeta.SPECIAL_ASSET_TYPE;
    /**The source of the asset, can only be 0 or 1, 0 indicates sale, 1 indicates wanting to purchase */
    exchangeDirection: BFMeta.EXCHANGE_DIRECTION;
}
```
## BFMeta.SPECIAL_ASSET_TYPE

```typescript
enum SPECIAL_ASSET_TYPE {
  /**Special asset type：dapp */
  DAPP_ID,
  /**Special asset type：chain domain name */
  LOCATION_NAME,
}
```
## BFMeta.EXCHANGE_DIRECTION

```typescript
enum EXCHANGE_DIRECTION {
  /**Special assets come from the initiating account of the to transaction, which is sale*/
  ASSET_FROM_SENDER = 0,
  /**Special assets come from the initiating account of the to transaction, which is wanting to purchase */
  ASSET_FROM_RECIPIENT = 1,
}
```
## BFMeta.BeExchangeSpecialAssetAssetJSON

```typescript
interface BeExchangeSpecialAssetAssetJSON { 
    /**Information attached to the event of receiving asset exchange */
    beExchangeSpecialAsset: BeExchangeSpecialAssetJSON;
}
```
## BFMeta.BeExchangeSpecialAssetJSON

```typescript
interface BeExchangeSpecialAssetJSON { 
    /**The signature of the event that initiates asset exchange, 128-byte hexadecimal string */
    transactionSignature: string;
    /**Signature array generated by encryption key */
    ciphertextSignature?: AccountSignatureJSON;
    /**Information about asset exchange */
    exchangeSpecialAsset: ToExchangeSpecialAssetJSON;
}
```
## BFMeta.GiftAssetAssetJSON

```typescript
interface GiftAssetAssetJSON { 
    /**Information attached to the event of equity gift */
    giftAsset: GiftAssetJSON;
}
```
## BFMeta.GiftAssetJSON

```typescript
interface GiftAssetJSON { 
    /**The public key array generated by the encryption key */
    cipherPublicKeys: string[];
    /**The chain name of the equity to which the given equity belongs, composed of lowercase letters, 3-8 digits */
    sourceChainName: string;
    /**The chain network identifier to which the given equity belongs, composed of uppercase letters or numbers, 5 characters, the last digit is the check digit */
    sourceChainMagic: string;
    /**The name of given equity, composed of uppercase letters, 3-5 characters */
    assetType: string;
    /**The number of given equity, composed of 0-9 and the decimal points are not included, must be greater than 0 */
    amount: string;
    /**The number of times that can be received, composed of 0-9 and the decimal points are not included, must be greater than 0 */
    totalGrabableTimes: number;
    /**The height of the block being signed at the beginning, composed of 0-9 and the decimal points are not included */
    beginUnfrozenBlockHeight?: number;
    /**Receiving rules, can only be 0, 1 or 2, 0 means even distribution, 1 means random distribution according to the address of any account, 2 means random distribution according to the account address in the recipient list */
    giftDistributionRule: BFMeta.GIFT_DISTRIBUTION_RULE;
}
```
## BFMeta.GIFT_DISTRIBUTION_RULE

```typescript
enum GIFT_DISTRIBUTION_RULE {
  /**Equal distribution */
  AVERAGE,
  /**Random distribution based on the address of any account */
  RANDOM,
  /**Random distribution based on the account address in the recipient list
   * This rule will ensure that the given assets can be distributed as much as possible, and ensure that each receiving account can obtain a certain amount
   */
  RECIPIENT_RANDOM,
};
```
## BFMeta.GrabAssetAssetJSON

```typescript
interface GrabAssetAssetJSON { 
    /**Information attached to the event of receiving equity gift */
    grabAsset: GrabAssetJSON;
}
```
## BFMeta.GrabAssetJSON

```typescript
interface GrabAssetJSON { 
    /**The signature of the block where the gift event exists, 128-byte hexadecimal string */
    blockSignature: string;
    /**The signature of the gift event, 128-byte hexadecimal string */
    transactionSignature: string;
    /**Calculated according to the consensus rules: the amount grabbed */
    amount: string;
    /**The cryptographic signature used for verifying identity, if needed */
    ciphertextSignature?: AccountSignatureJSON;
    /**The following are redundant fields
     * All can be queried from `transactionSignature`, but this is still stored, which ensures that it can still be rendered in an independent situation
     */
    /**Gift configuration */
    giftAsset: GiftAssetJSON;
}
```
## BFMeta.TrustAssetAssetJSON

```typescript
interface TrustAssetAssetJSON { 
    /**Information attached to the witness event */
    trustAsset: TrustAssetJSON;
}
```
## BFMeta.TrustAssetJSON

```typescript
interface TrustAssetJSON { 
    /**Witness account address array, base58-encoded hexadecimal string array */
    trustees: string[];
    /**The number of witnesses' signatures required when signing for receipt, composed of 0-9, must be greater than 0, the maximum value is the number of designated trustees + 2 */
    numberOfSignFor: number;
    /**The chain name of the witnessed equity, composed of lowercase letters, 3-8 digits */
    sourceChainName: string;
    /**The chain network identifier to which the witnessed equity belongs, composed of uppercase letters or numbers, 5 characters, the last digit is the check digit  */
    sourceChainMagic: string;
    /**The name of the witnessed equity, composed of uppercase letters, 3-5 characters */
    assetType: string;
    /**The number of witnessed equity, composed of 0-9 and the decimal points are not included, must be greater than 0 */
    amount: string;
}
```
## BFMeta.SignForAssetAssetJSON

```typescript
interface SignForAssetAssetJSON { 
    /**Information attached to the event of sign for the witness event  */
    signForAsset: SignForAssetJSON;
}
```
## BFMeta.SignForAssetJSON

```typescript
interface SignForAssetJSON { 
    /**The signature of the witness transaction，*/
    transactionSignature: string;
    /**The address of the initiating account of the witness transaction, a base58-encoded hexadecimal string */
    trustSenderId: string;
    /**The receiving account address of the witness transaction, base58-encoded hexadecimal string */
    trustRecipientId: string;
    /**Witness information */
    trustAsset: TrustAssetJSON;
}
```
## BFMeta.EmigrateAssetAssetJSON

```typescript
interface EmigrateAssetAssetJSON { 
    /**Information attached to the event of equity transferred out */
    emigrateAsset: EmigrateAssetJSON;
}
```
## BFMeta.EmigrateAssetJSON

```typescript
interface EmigrateAssetJSON { 
    /**Genesis trustee signature */
    genesisDelegateSignature: AccountSignatureJSON;
    /**The chain name to which the equity transferred out belongs, composed of lowercase letters, 3-8 digits */
    sourceChainName: string;
    /**The chain network identifier of the equity transferred out, composed of uppercase letters or numbers, 5 characters, the last digit is the check digit */
    sourceChainMagic: string;
    /**The name of the equity transferred out, composed of uppercase letters, 3-5 characters */
    assetType: string;
    /**The number of equity transferred out, composed of 0-9 and the decimal points are not included, must be greater than 0 */
    amount: string;
}
```
## BFMeta.ImmigrateAssetAssetJSON

```typescript
interface ImmigrateAssetAssetJSON { 
    /**Information attached to the event of equity transferred in */
    immigrateAsset: ImmigrateAssetJSON;
}
```
## BFMeta.ImmigrateAssetJSON

```typescript
interface ImmigrateAssetJSON { 
    /**Signeture of the genesis trustee */
    genesisDelegateSignature: AccountSignatureJSON;
    /**Complete event of main equity transferred out*/
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
    /**Information attached to the bit name event of registration/deregistration */
    locationName: LocationNameJSON;
}
```
## BFMeta.LocationNameJSON

```typescript
interface LocationNameJSON { 
    /**Registered/logged out bit name, 2-1024 characters, the maximum length of each level of domain name is 128 characters, the first-level domain name can only be composed of lowercase letters, and the beginning and end of the second level and above can only be lowercase letters or numbers, can contain underscore, the root domain name must be the chain name of this chain */
    name: string;
    /**The source chain name of the registered/logged out bit name, composed of lowercase letters, 3-8 digits */
    sourceChainName: string;
    /**The source chain network identifier of the registered/logged out bit name, composed of uppercase letters or numbers, 5 characters, the last digit is the check digit */
    sourceChainMagic: string;
    /**Operation type, can only be 0 or 1, 0 means registration bit name, 1 means deregistration bit name */
    operationType: BFMeta.LOCATION_NAME_OPERATION_TYPE;
}
```
## BFMeta.LOCATION_NAME_OPERATION_TYPE

```typescript
enum LOCATION_NAME_OPERATION_TYPE {
  /**Register lns */
  REGISTRATION,
  /**Logout */
  CANCELLATION,
};
```
## BFMeta.SetLnsManagerAssetJSON

```typescript
interface SetLnsManagerAssetJSON { 
    /**Information attached to the event of setting bit name administrator */
    lnsManager: SetLnsManagerJSON;
}
```
## BFMeta.SetLnsManagerJSON

```typescript
interface SetLnsManagerJSON { 
    /**Bit name, 2-1024 characters, the maximum length of each level of domain name is 128 characters, the first-level domain name can only be composed of lowercase letters, the beginning and end of the second level and above can only be composed of lowercase letters or numbers, can contain underscore, the root domain name must be the chain name of this chain */
    name: string;
    /**The source chain name of bit name, composed of lowercase letters, 3-8 digits */
    sourceChainName: string;
    /**The source chain network identifier of the bit name, composed of uppercase letters or numbers, 5 characters, the last digit is the check digit  */
    sourceChainMagic: string;
}
```
## BFMeta.SetLnsRecordValueAssetJSON

```typescript
interface SetLnsRecordValueAssetJSON { 
    /**Information attached to the event of setting the resolution value of the bit name */
    lnsRecordValue: SetLnsRecordValueJSON;
}
```
## BFMeta.SetLnsRecordValueJSON

```typescript
interface SetLnsRecordValueJSON { 
    /**Bit name, 2-1024 characters, the maximum length of each level of domain name is 128 characters, the first-level domain name can only be composed of lowercase letters, the beginning and end of the second level and above can only be composed of lowercase letters or numbers, underscore can be included, the root domain name must be the chain name of this chain */
    name: string;
    /**The source chain name of bit name, composed of lowercase letters, 3-8 digits */
    sourceChainName: string;
    /**The source chain network identifier of bit name, composed of uppercase letters or numbers, 5 characters, the last digit is the check digit */
    sourceChainMagic: string;
    /**Operation type, can only be 0 or 1 or 2, 0 means adding, 1 means deleting, 2 means update */
    operationType: BFMeta.RECORD_OPERATION_TYPE;
    /**Added resolution value, the type of resolution value can only be A or AAAA or LNG_LAT or BLOCK_CHAIN_ACCOUNT_ADDRESS, A means ipV4, AAAA means ipV6, LNG_LAT means latitude and longitude, BLOCK_CHAIN_ACCOUNT_ADDRESS means on-chain account address, the underscore is preceded by the resolution value type, the underscore is followed by the parse value, optional, required when the operation type is 0 or 2 */
    addRecord?: LocationNameRecordJSON;
    /**Deleted resolution value, the type of resolution value can only be A or AAAA or LNG_LAT or BLOCK_CHAIN_ACCOUNT_ADDRESS, A means ipV4, AAAA means ipV6, LNG_LAT means latitude and longitude, BLOCK_CHAIN_ACCOUNT_ADDRESS means on-chain account address, the underscore is preceded by the resolution value type, the underscore is followed by the parse value, optional, required when the operation type is 1 or 2 */
    deleteRecord?: LocationNameRecordJSON;
}
```
## BFMeta.RECORD_OPERATION_TYPE

```typescript
enum RECORD_OPERATION_TYPE {
  /**Add resolution value */
  ADD,
  /**Delete resolution value */
  DELETE,
  /**Update resolution value */
  UPDATE,
};
```
## BFMeta.LocationNameRecordJSON

```typescript
interface LocationNameRecordJSON { 
    /**Resolution value type */
    recordType: BFMeta.RECORD_TYPE;
    /**Resolution value */
    recordValue: string;
}
```
## BFMeta.RECORD_TYPE

```typescript
enum RECORD_TYPE {
  /**IPV4 resolution */
  IPV4 = "A",
  /**IPV6 resolution */
  IPV6 = "AAAA",
  /**Latitude and longitude resolution */
  LNG_LAT = "LNG_LAT",
  /**Account address resolution */
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
    /**Event */
    transactions: TransactionInBlockJSON[];
}
```
## BFMeta.BlockWithoutTransactionJSON

```typescript
interface BlockWithoutTransactionJSON<AssetJSON extends object = object> { 
    /**Block version number */
    version: number;
    /**Block height */
    height: number;
    /**Block sie */
    blockSize: number;
    /*Block timestamp */
    timestamp: number;
    /**Block signature */
    signature: string;
    /**Block security signature */
    signSignature?: string;
    /**Forger gong'yao */
    generatorPublicKey: string;
    /**Forger's security public key */
    generatorSecondPublicKey?: string;
    /**Forger's equity */
    generatorEquity: string;
    /**Number of block event */
    numberOfTransactions: number;
    /**The summary of block event */
    payloadHash: string;
    /**The length of the summary of the block event */
    payloadLength: number;
    /**Front block signature */
    previousBlockSignature: string;
    /**Total amount of assets generated */
    totalAmount: string;
    /**Total fees generated */
    totalFee: string;
    /**Block reward value */
    reward: string;
    /**Chain identifier of the block */
    magic: string;
    /**Block participation */
    blockParticipation: string;
    /**Block remarks */
    remark: {
        [key: string]: string;
    };
    /**Block additional information */
    asset: AssetJSON;
    /**Block statistics */
    statisticInfo: StatisticInfoJSON;
    /**Disconnected list of forgers */
    roundOfflineGeneratersHashMap: RoundOfflineGeneratersHashMap;
}
```
## BFMeta.GenesisBlockAssetJSON

```typescript
interface GenesisBlockAssetJSON { 
    /**Information attached to the genesis block */
    genesisAsset: GenesisAssetJSON;
}
```
## BFMeta.GenesisAssetJSON

```typescript
interface GenesisAssetJSON extends RoundDelegateJSON { 
    /**Chain name */
    chainName: string;
    /**Chain main equity name */
    assetType: string;
    /**Chain network identifier */
    magic: string;
    /**Chain network type, can only be'b' or'c', b is the official network, and c is the test network */
    bnid: string;
    /**Chain genesis time */
    beginEpochTime: number;
    /**Chain genesis bit name */
    genesisLocationName: string;
    /**The amount of main equity initially held by the chain genesis account */
    genesisAmount: string;
    /**Minimum fee for each byte of chain event */
    minTransactionFeePerByte: FractionJSON;
    /**The maximum bytes of on-chain event body */
    maxTransactionSize: number;
    /**The maximum number of bytes of on-chain block body */
    maxBlockSize: number;
    /**The largest processing event of the on-chain block tps*/
    maxTPSPerBlock: number;
    /**The minimum height required to lag behind when deleting a fork */
    consessusBeforeSyncBlockDiff: number;
    /**The number of registered trustees that can be processed in each round */
    maxDelegateTxsPerRound: number;
    /**The maximum number of times that can be grabbed for equity gift event */
    maxGrabTimesOfGiftAsset: number;
    /**The minimum amount of chain main equity held by the account issuing equity */
    issueAssetMinChainAsset: string;
    /**The minimum amount of main equity held by the account that registers the genesis block */
    registerChainMinChainAsset: string;
    /**The maximum number of expired block intervals  */
    maxApplyAndConfirmedBlockHeightDiff: number;
    /**The number of blocks per round */
    blockPerRound: number;
    /**The number of genesis trustees */
    delegates: number;
    /**Whether the trustee is allowed to continuously participate in the election */
    whetherToAllowDelegateContinusElections: boolean;
    /**Block interval */
    forgeInterval: number;
    /**Reward distribution ratio, JSON object */
    rewardPercent: RewardPercentJSON;
    /**Blockchain port number, JSON object */
    ports: PortsJSON;
    /**Block reward, JSON object */
    rewardPerBlock: RewardPerBlockJSON;
    /**Account participation weight ratio, JSON object  */
    accountParticipationWeightRatio: AccountParticipationWeightRatioJSON;
    /**Block participation weight ratio, JSON object */
    blockParticipationWeightRatio: BlockParticipationWeightRatioJSON;
    /**Difficulty coefficient of constructing tpow*/
    averageComputingPower: number;
    /**Block height for tpow exemption */
    tpowOfWorkExemptionBlocks: number;
    /**tpow configuration, JSON object */
    transactionPowOfWorkConfig: TransactionPowOfWorkConfigJSON;
}
```
## BFMeta.RoundDelegateJSON

```typescript
interface RoundDelegateJSON { 
    /**The next round of blocking account list */
    nextRoundDelegates: NextRoundDelegateJSON[];
    /**Newly registered trustee */
    newDelegates: string[];
    /**The maximum amount of end-of-round main equity in the previous round of voting accounts */
    maxBeginBalance: string;
    /**The largest amount of events in the previous round of voting accounts */
    maxTxCount: number;
    /**The ratio of the maximum amount of the end-of-round main equity in the previous round of voting accounts to the largest event in the previous round of voting accounts */
    rate: string;
}
```
## BFMeta.NextRoundDelegateJSON

```typescript
interface NextRoundDelegateJSON { 
    /**Trustee account address */
    address: string;
    /**The equity obtained by the trustee in the previous round */
    equity: string;
}
```
## BFMeta.FractionJSON

```typescript
interface FractionJSON<T extends number | bigint | string = number> { 
    /**Molecule */
    numerator: T;
    /**Denominator */
    denominator: T;
}
```
## BFMeta.RewardPercentJSON

```typescript
interface RewardPercentJSON { 
    /**The percentage of votes to the total block reward */
    votePercent: FractionJSON;
    /**The percentage of blocking to the total block reward */
    forgePercent: FractionJSON;
}
```
## BFMeta.PortsJSON

```typescript
interface PortsJSON { 
    /**Consensus port number */
    port: number;
    /**Node scan port number */
    scan_peer_port: number;
}
```
## BFMeta.RewardPerBlockJSON

```typescript
interface RewardPerBlockJSON { 
    /**Block mileage of reward changes */
    readonly heights: number[];
    /**The number of assets corresponding to the rewards of each block mileage */
    readonly rewards: string[];
}
```
## BFMeta.AccountParticipationWeightRatioJSON

```typescript
interface AccountParticipationWeightRatioJSON { 
    /**The weight of the amount of main equity the account held at the end of the previous round */
    balanceWeight: number;
    /**The weight of the account's previous round of events */
    numberOfTransactionsWeight: number;
}
```
## BFMeta.BlockParticipationWeightRatioJSON

```typescript
interface BlockParticipationWeightRatioJSON { 
    /**The weight of the amount of main equity packaged in the block */
    balanceWeight: number;
    /**The weight of the amount of events packaged in the block */
    numberOfTransactionsWeight: number;
}
```
## BFMeta.TransactionPowOfWorkConfigJSON

```typescript
interface TransactionPowOfWorkConfigJSON { 
    /**Chain's pow difficulty growth coefficient  */
    growthFactor: FractionJSON<string>;
    /**Chain's pow calculate participation rate */
    participationRatio: FractionJSON;
}
```
## BFMeta.StatisticInfoJSON

```typescript
interface StatisticInfoJSON { 
    /**Total fee for events packaged by the block */
    totalFee: string;
    /**Total equity value of the event packaged by the block (without distinguishing the equity type) */
    totalAsset: string;
    /**Total main equity value of the event packaged by the block */
    totalChainAsset: string;
    /**Total number of accounts involved in the event packaged by the block */
    totalAccount: number;
    /**Statistic details of event equity type packaged by block, JSON object */
    assetStatisticHashMap: {
        [index: number]: AssetStatisticJSON;
    };
}
```
## BFMeta.AssetStatisticJSON

```typescript
interface AssetStatisticJSON extends AssetInfoJSON { 
    /**The index of the equity in the block */
    index: number;
    /**Statistical details of event types packaged by the block, JSON object */
    typeStatisticHashMap: {
        [baseType: number]: CountAndAmountStatisticJSON;
    };
    /**Statistics of event equity type packaged by block, JSON object */
    total: CountAndAmountStatisticJSON;
}
```
## BFMeta.AssetInfoJSON

```typescript
type AssetInfoJSON = {
    /**The chain network identification to which the equity belongs, composed of uppercase letters or numbers, 5 characters, the last digit is the check digit */
    magic: string;
    /**Equity name, composed of uppercase letters, 3-5 characters */
    assetType: string;
};
```
## BFMeta.CountAndAmountStatisticJSON

```typescript
interface CountAndAmountStatisticJSON { 
    /**Total value of changes, an increase or decrease in an account will have an impact */
    changeAmount: string;
    /**Total number of changes, an increase or decrease in an account will have an impact */
    changeCount: number;
    /**Total number of asset transfer */
    moveAmount: string;
    /**Transaction volume */
    transactionCount: number;
}
```
## BFMeta.RoundOfflineGeneratersHashMap

```typescript
interface RoundOfflineGeneratersHashMap { 
    /**
     * Address separated by commas
     * address,address */
    [roundOffset: string]: string;
}
```
## BFMeta.TransactionInBlockJSON

```typescript
interface TransactionInBlockJSON<T extends TransactionJSON = TransactionJSON> extends SomeTransactionJSON<T> { 
    /**The index of the event in the block */
    index: number;
    /**Block height */
    height: number;
    /**The amount of events in the event initiating account  */
    numberOfSenderTransactions: number;
    /**Information about changes in account equity involved in the event */
    transactionAssetChanges: TransactionAssetChangeJSON[];
    /**The latest information about the equity before the equity are destroyed */
    assetPrealnum?: AssetPrealnumJSON;
    /**The signature of the block forger */
    signature: string;
    /**The security signature of the block forger */
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
    /**Account type */
    accountType: number;
    /**The index of equity in the block */
    assetTypes: number;
    /**The latest amount of equity held by the account */
    assetBalance: string;
}
```
## BFMeta.AssetPrealnumJSON

```typescript
type AssetPrealnumJSON = {
    /**The number of the rest of equity */
    remainAssetPrealnum: string;
    /**The number of frozen main equity */
    frozenMainAssetPrealnum: string;
};
```
## BFMetaPC.Config.ConfigRevisable

```typescript
interface ConfigRevisable { 
    /**Process parameter configuration  */
    coreForProcess: {
        /**Whether to use the configured number of cores */
        forceUseConfig: boolean;
        /**Number of processes that processes transaction */
        coreNumForDealTransaction: number;
        /**Number of processes that processes account */
        coreNumForMemInfo: number;
        /**Number of processes that processes unprocessed transactions */
        coreNumForUntreatedTrs: number;
    };
    /**Event parameters */
    transactionConfig: {
        /**Whether to receive votes */
        receiveVoteEnable: boolean;
        /**The maximum number of voting transactions that can be processed in each round */
        maxTransactionLimitForVote: number;
        /**Transaction limit of each block */
        transactionsLimitPerBlock: number;
        /**Minimum handling fee for miners */
        minFeePerByte: {
            /**Molecule */
            numerator: number;
            /**Denominator */
            denominator: number;
        };
        /**Maximum handling fee for the event, unit: B */
        maxFee: string;
        /**Automatic voting configuration */
        autoVote: AutoVoteModel;
    };
    /**File log configuration */
    logConfig: {
        /**Console output level: "error" | "warn" | "info"  */
        consoleLogLevel: string;
        /**Log level: "error" | "warn" | "info"  */
        fileLogLevel: string;
        /**Log file size limit(mb) */
        fileLogLimit: number;
        /**Number of copies divided when backing up the log */
        fileLogBackup: number;
        /**Whether to divide by date */
        fileLogDateExpire: boolean;
        /**Log retention days */
        fileLogDaysToKeep: number;
    };
    /**Startup parameter */
    startConfig: {
        /**Whether to enable blocking, true: start to blocking, false: stop to block */
        generateBlockEnable: boolean;
        /**Block mark */
        remark: string;
        /**Use critical checkpoint to start */
        useCheckPoint: boolean;
        /**Number of critical checkpoint retention rounds */
        numberOfReservedCheckPoint: number;
        /**Node initial connection ip */
        peers: string[];
        /**Maximum number of connections */
        maxChannelNumber: number;
    };
    /**Network configuration */
    networkConfig: {
        /**Whether to enable grpc */
        grpcEnable: boolean;
    };
    /**Traffic control configuration */
    flowControlConfig: {
        /**Traffic control */
        requestLimit: {
            /**Whether to enable */
            enable: boolean;
            /**Total limit of visits */
            count: number;
            /**Maximum number of accesses to apis */
            apiRequestInterface: {
                [apiName: string]: number;
            };
            /**Reset interval */
            time: number;
        };
        /**Total traffic limit */
        flowRequestLimit: {
            enable: boolean;
            /**Upper limit of total traffic */
            count: number;
            time: number;
        };
    };
    /**Disk Monitoring */
    diskMonitorConfig: {
        enable: boolean;
        /**Monitor disk path and configuration */
        clearPath: string[];
        /**Clean up when the disk space is less than x */
        clearWhenFreeSpaceLowerThan: number;
        /**Check time interval */
        checkInterval: number;
        /**Clean up files with suffix xxx */
        clearWithSuffix?: string[];
        /**Files with suffix xxx will not be cleaned up */
        noClearWithSuffix?: string[];
        /**Files whose last modification time is greater than n milliseconds will not be cleaned up */
        noClearWithLastModifyTimeGreaterThen?: number;
    };
    /**Notification configuration */
    noticeConfig: {
        enable: boolean;
        /**Email sending conditions  */
        sendCondition: {
            usageCPU: number;
            usageDisk: number;
            usageMemory: number;
            missedBlocks: number;
        };
        /**Email sending interval */
        sendIntervalTime: number;
    };
    /**Service market configuration */
    serviceConfig: {
        /**Whether to enable service market */
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
    /**Whether to enable automatic voting */
    enable: boolean;
    /**Whether to use configuration fee */
    useConfigFee: boolean;
    /**Fee for automatic voting */
    fee: string;
    /**Whether to give priority to the number of referees */
    priorRecommendedNumber: boolean;
    /**Maximum number of referees selected */
    maxNumberOfRecommended: number;
    /**Selected block range, the latest 100 rounds */
    numberOfRounds: number;
    /**The proportion of online rate */
    productivityPercent: number;
    /**The proportion of the number of blocking */
    forgedBlocksPercent: number;
    /**The proportion of packaged transactions */
    applyTxPercent: number;
    /**The Proportion of votes in the previous round */
    votePercent: number;
    /**The proportion of new trustees */
    newDelegatePercent: number;
    /**The minimum account online rate that can be recommended */
    minBeSelectProductivity: number;
};
```
## BFMetaPC.Config.ConfigReadable

```typescript
interface ConfigReadable extends ConfigRevisable { 
     /**Chain port number */
    chainPort: ConfigPort;
}
```
## BFMetaPC.Config.ConfigPort

```typescript
interface ConfigPort { 
    /**turnsever server port */
    turnserver: number;
    /**Database use port */
    mongodb: number;
    /**Node management use port */
    system_websocket_port: number;
    /**Process lock use port */
    clusterLock: number;
    /**grpc use port */
    grpc: number;
    /**Update service use port  */
    update_service_port: number;
    /**comlink use port */
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
