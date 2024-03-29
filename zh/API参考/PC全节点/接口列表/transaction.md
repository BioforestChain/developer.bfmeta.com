# 事件类接口
## 1 转账事件
### 1.1 创建转账事件
- 简介
    - 链上最基本的权益转移，可使某个资产从A地址直接转入B地址。
    - 资产类型为assetType，数量为amount 最小单位为1。 1BFM = 100000000
- 接口全称：`trTransferAsset`
- 接口简写：`tta`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trTransferAsset`
- 请求参数：
```typescript
interface TrTransferAsset extends TransactionCommonParams{
    /**转移的权益所属链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic?: string;
    /**转移的权益所属链名，小写字母组成，3-8 位 */
    sourceChainName?: string;
    /**转移的权益类型，大写字母组成，3-5 个字符 */
    assetType?: string;
    /**转移的权益数量，0-9 组成并且不包含小数点，必须大于 0 */
    amount: string;
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId: string;
}

```
- 返回参数：
```typescript
type TrTransferAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的转移事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.TransferAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 1.2 创建转账事件(带安全密钥)
- 接口全称：`trTransferAssetWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trTransferAssetWithSign`
- 请求参数：
```typescript
interface PackageTrTransferAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrTransferAssetRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 1.3 发送转账事件
- 接口全称：`transferAsset`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/transferAsset`
- 请求参数：
```typescript
interface SendTrTransferAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrTransferAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的转移事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.TransferAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 2 设置安全密码事件
### 2.1 创建设置安全密码事件
- 简介
    - 此事件将为地址设置安全密码。设置了安全密码的地址，在此后的所有事件都需要安全密码的签名才可验证通过。
    - 重复发送该事件能够修改安全密码，最新的安全密码以最后一次上链事件为准。
- 接口全称：`trSignature`
- 接口简写：`tsi`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trSignature`
- 请求参数：
```typescript
interface TrSignature extends TransactionCommonParams{
    newSecondSecret: string;
}

```
- 返回参数：
```typescript
type TrSignatureRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的设置安全密码事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.SignatureAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 2.2 创建设置安全密码事件(带安全密钥)
- 接口全称：`trSignatureWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trSignatureWithSign`
- 请求参数：
```typescript
interface PackageTrSignatureParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrSignatureRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 2.3 发送设置安全密码事件
- 接口全称：`signature`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/signature`
- 请求参数：
```typescript
interface SendTrSignatureParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrSignatureRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的设置安全密码事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.SignatureAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 3 设置用户名事件
### 3.1 创建设置用户名事件
- 简介
    - 此事件将为地址设置用户名。用户名将在链上展示。
    - 每个地址仅有一次设置用户名的机会，且用户名不可更改。
- 接口全称：`trUsername`
- 接口简写：`tusr`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trUsername`
- 请求参数：
```typescript
interface TrUsernam extends TransactionCommonParams{
    /**用户名字符串，大小写字母、数字、下划线组成，1-20 个字符，不能包含当前链的名称 */
    alias: string;
}

```
- 返回参数：
```typescript
type TrUsernameRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的设置用户名事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.UsernameAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 3.2 创建设置用户名事件(带安全密钥)
- 接口全称：`trUsernameWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trUsernameWithSign`
- 请求参数：
```typescript
interface PackageTrUsernameParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrUsernameRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 3.3 发送设置用户名事件
- 接口全称：`username`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/username`
- 请求参数：
```typescript
interface SendTrUsernameParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrUsernameRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的设置用户名事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.UsernameAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 4 注册受托人事件
### 4.1 创建注册受托人事件
- 简介
    - 此事件将使地址注册为受托人。成为受托人的地址，当票数足够多时，能够争取到锻造区块的名额。
    - 每个地址仅有一次注册受托人的机会，且注册后无法取消。
- 接口全称：`trDelegate`
- 接口简写：`tdeg`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trDelegate`
- 请求参数：
```typescript
interface TrDelegate extends TransactionCommonParams {
    
}

```
- 返回参数：
```typescript
type TrDelegateRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的注册受托人事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.DelegateAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 4.2 创建注册受托人事件(带安全密钥)
- 接口全称：`trDelegateWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trDelegateWithSign`
- 请求参数：
```typescript
interface PackageTrDelegateParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrDelegateRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 4.3 发送注册受托人事件
- 接口全称：`delegate`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/delegate`
- 请求参数：
```typescript
interface SendTrDelegateParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrDelegateRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的注册受托人事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.DelegateAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 5 接收投票事件
### 5.1 创建接收投票事件
- 简介
    - 此事件将开启受托人地址接收投票功能，只有开启该功能才能够被投票。
- 接口全称：`trAcceptVote`
- 接口简写：`tav`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trAcceptVote`
- 请求参数：
```typescript
interface TrAcceptVote extends TransactionCommonParams {
   
}

```
- 返回参数：
```typescript
type TrAcceptVoteRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的接收投票事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.AcceptVoteAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 5.2 创建接收投票事件(带安全密钥)
- 接口全称：`trAcceptVoteWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trAcceptVoteWithSign`
- 请求参数：
```typescript
interface PackageTrAcceptVoteParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrAcceptVoteRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 5.3 发送接收投票事件
- 接口全称：`acceptVote`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/acceptVote`
- 请求参数：
```typescript
interface SendTrAcceptVoteParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrAcceptVoteRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的接收投票事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.AcceptVoteAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 6 拒绝投票事件 
### 6.1 创建拒绝投票事件 
- 简介
    - 此事件将关闭受托人地址接收投票功能，只有开启该功能才能够被投票。
- 接口全称：`trRejectVote`
- 接口简写：`trv`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trRejectVote`
- 请求参数：
```typescript
interface TrRejectVote extends TransactionCommonParams {
    
}

```
- 返回参数：
```typescript
type TrRejectVoteRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的拒绝投票事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.RejectVoteAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 6.2 创建拒绝投票事件 (带安全密钥)
- 接口全称：`trRejectVoteWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trRejectVoteWithSign`
- 请求参数：
```typescript
interface PackageTrRejectVoteParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrRejectVoteRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 6.3 发送拒绝投票事件
- 接口全称：`rejectVote`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/rejectVote`
- 请求参数：
```typescript
interface SendTrRejectVoteParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrRejectVoteRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的拒绝投票事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.RejectVoteAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 7 投票事件
### 7.1 创建投票事件
- 简介
    - 此事件能够向受托人地址进行投票。
    - 当轮末块时，将统计本轮投票数最高的57个地址选为锻造者。
    - 能否连续成为锻造者由创世块共识决定。
- 接口全称：`trVote`
- 接口简写：`tv`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trVote`
- 请求参数：
```typescript
interface TrVote extends TransactionCommonParams {
    /**投出的权益数，0-9 组成并且不包含小数点，允许为 0 */
    equity: string;
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId: string;
}

```
- 返回参数：
```typescript
type TrVoteRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的投票事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.VoteAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 7.2 创建投票事件(带安全密钥)
- 接口全称：`trVoteWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trVoteWithSign`
- 请求参数：
```typescript
interface PackageTrVoteParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrVoteRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 7.3 发送投票事件
- 接口全称：`vote`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/vote`
- 请求参数：
```typescript
interface SendTrVoteParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrVoteRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的投票事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.VoteAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 8 发行 dapp 事件
### 8.1 创建发行 dapp 事件
- 简介
  - 实验性接口
  - 在链上创建一个唯一的 dappid
  - 免费的 dappid 则开放给所有人使用，付费的 dappid 其他人在使用前需要先购买使用权，购买一次，终身使用
  - 如果 dappid 的拥有者是受托人并且开启了收票，则其他人再使用 dappid 前的必须在两轮次内给拥有者投票
- 接口全称：`trDapp`
- 接口简写：`tda`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trDapp`
- 请求参数：
```typescript
interface TrDapp extends TransactionCommonParams {
     /**不包含校验位的 dappid，大写字母或数字组成，7 个字符 */
    newDappid: string;
    /**dappid 的类型，只能为 0 或 1，0 表示这个 dappid 是付费使用的，1 表示这个 dappid 是免费使用的 */
    type: number;
    /**购买 dappid 使用权需要的权益数量(如果 dappid 是付费应用则必须携带，如果是免费应用则无需携带)，0-9 组成并且不包含小数点，必须大于 0 */
    amount?: string;
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId: string;
}

```
- 返回参数：
```typescript
type TrDappRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的发行dapp事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.DAppAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 8.2 创建发行 dapp 事件(带安全密钥)
- 接口全称：`trDappWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trDappWithSign`
- 请求参数：
```typescript
interface PackageTrDappParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrDappRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 8.3 发送发行 dapp 事件
- 接口全称：`dapp`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/dapp`
- 请求参数：
```typescript
interface SendTrDappParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrDappRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的发行dapp事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.DAppAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 9  dapp 购买事件
### 9.1 创建 dapp 购买事件
- 简介
  - 实验性接口
  - 购买某个 dappid 的使用权
- 接口全称：`trDappPurchasing`
- 接口简写：`tdap`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trDappPurchasing`
- 请求参数：
```typescript
interface TrDappPurchasing extends TransactionCommonParams{
    /**购买的 dappid 的发行事件签名，128 个字节的 16 进制字符串 */
    transactionSignature: string;
}

```
- 返回参数：
```typescript
type TrDappPurchasingRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的dapp购买事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.DAppPurchasingAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 9.2 创建 dapp 购买事件(带安全密钥)
- 接口全称：`trDappPurchasingWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trDappPurchasingWithSign`
- 请求参数：
```typescript
interface PackageTrDappPurchasingParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrDappPurchasingRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 9.3 发送 dapp 购买事件
- 接口全称：`dappPurchasing`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/dappPurchasing`
- 请求参数：
```typescript
interface SendTrDappPurchasingParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrDappPurchasingRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的dapp购买事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.DAppPurchasingAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 10 存证事件
### 10.1 创建存证事件
- 简介
  - 实验性接口
  - 把某个数据永久的存储到区块链上
  - 需要使用 dappid
- 接口全称：`trMark`
- 接口简写：`tmr`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trMark`
- 请求参数：
```typescript
interface TrMark extends TransactionCommonParams{
    /**购买的 dappid 的发行事件签名，128 个字节的 16 进制字符串 */
    transactionSignature: string;
    /**存证内容，为任意字符串 */
    content: string;
    /**存证类型，为任意字符串，用于区别存证 */
    action: string;
}

```
- 返回参数：
```typescript
type TrMarkRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的存证事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.MarkAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 10.2 创建存证事件(带安全密钥)
- 接口全称：`trMarkWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trMarkWithSign`
- 请求参数：
```typescript
interface PackageTrMarkParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrMarkRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 10.3 发送存证事件
- 接口全称：`mark`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/mark`
- 请求参数：
```typescript
interface SendTrMarkParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrMarkRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的存证事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.MarkAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 11 权益发行事件
### 11.1 创建权益发行事件
- 简介
  - 冻结账户，在链上创建出指定数量的新权益
  - 发起该事件需要一定的主权益数量。
  - 发起该事件将直接冻结本地址，所冻结的地址无法发起任何事件，但能够接收事件，可以通过资产转移向地址转入主权益，从而提高冻结的主权益数量。
  - 冻结的主权益数量越多反映了发行的权益有更多的主权益作为保障。
  - 资产销毁时冻结的主权益将以销毁数量为比例赎回至指定账户。
- 接口全称：`trIssueAsset`
- 接口简写：`tia`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trIssueAsset`
- 请求参数：
```typescript
interface TrIssueAsset extends TransactionCommonParams{
    /**发行的权益名，大写字母组成，3-5 个字符 */
    assetType: string;
    /**发行的新权益总数，权益数量由0-9共十个数字组成，权益数量不包含小数点且必须大于0 */
    expectedIssuedAssets: string;
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId: string;
}

```
- 返回参数：
```typescript
type TrIssueAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的权益发行事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.IssueAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 11.2 创建权益发行事件(带安全密钥)
- 接口全称：`trIssueAssetWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trIssueAssetWithSign`
- 请求参数：
```typescript
interface PackageTrIssueAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrIssueAssetRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 11.3 发送权益发行事件
- 接口全称：`issueAsset`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/issueAsset`
- 请求参数：
```typescript
interface SendTrIssueAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrIssueAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的权益发行事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.IssueAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 12 权益销毁事件
### 12.1 创建权益销毁事件
- 简介
  - 销毁账户下指定数量的权益，赎回链的主权益
  - 主权益和外来权益不能销毁
  - 赎回的主权益会回到recipientId地址中
- 接口全称：`trDestoryAsset`
- 接口简写：`tdya`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trDestoryAsset`
- 请求参数：
```typescript
interface TrDestoryAsset extends TransactionCommonParams{
     /**销毁的权益名，大写字母组成，3-5 个字符 */
    assetType: string;
    /**销毁的权益数，0-9 组成并且不包含小数点，必须大于 0 */
    amount: string;
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId: string;
}
```
- 返回参数：无
### 12.2 创建权益销毁事件(带安全密钥)
- 接口全称：`TrDestoryAssetWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/TrDestoryAssetWithSign`
- 请求参数：
```typescript
interface PackageTrDestoryAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：无
### 12.3 发送权益销毁事件
- 接口全称：`destoryAsset`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/destoryAsset`
- 请求参数：
```typescript
interface SendTrDestoryAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：无
## 13 权益交换事件
### 13.1 创建权益交换事件
- 简介
  - 把自己账户下的某种权益用指定的比例冻结到链上，等待其他持有某种指定权益的账户来交换
- 接口全称：`trToExchangeAsset`
- 接口简写：`ttea`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trToExchangeAsset`
- 请求参数：
```typescript
interface TrToExchangeAsset extends TransactionCommonParams{
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
- 返回参数：
```typescript
type TrToExchangeAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的权益交换事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.ToExchangeAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 13.2 创建权益交换事件(带安全密钥)
- 接口全称：`trToExchangeAssetWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trToExchangeAssetWithSign`
- 请求参数：
```typescript
interface PackageTrToExchangeAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrToExchangeAssetRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 13.3 发送权益交换事件
- 接口全称：`toExchangeAsset`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/toExchangeAsset`
- 请求参数：
```typescript
interface SendTrToExchangeAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrToExchangeAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的权益交换事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.ToExchangeAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 14 接受权益交换事件
### 14.1 创建接受权益交换事件
- 简介
  - 用自己账户上的某种权益换取链上其他人冻结的某种权益
  - transactionSignature 对应交换事件的签名
- 接口全称：`trBeExchangeAsset`
- 接口简写：`tbea`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trBeExchangeAsset`
- 请求参数：
```typescript
interface TrBeExchangeAsset extends TransactionCommonParams{
    /**to 事件的签名，128 个字节的 16 进制字符串 */
    transactionSignature: string;
    /**用于交换的权益数量，权益数量由0-9共十个数字组成，权益数量不包含小数点且必须大于0 */
    beExchangeNumber: string;
    /**交换得到的权益数量，权益数量由0-9共十个数字组成，权益数量不包含小数点且必须大于0 */
    toExchangeNumber?: string;
    /**加密密钥，如果权益交换事件填写了加密密钥，则必须携带某个权益交换事件指定密钥以生成密钥签名对 */
    ciphertext?: string;
}

```
- 返回参数：
```typescript
type TrBeExchangeAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的接受权益交换事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.BeExchangeAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 14.2 创建接受权益交换事件(带安全密钥)
- 接口全称：`trBeExchangeAssetWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trBeExchangeAssetWithSign`
- 请求参数：
```typescript
interface PackageTrBeExchangeAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrBeExchangeAssetRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 14.3 发送接受权益交换事件
- 接口全称：`beExchangeAsset`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/beExchangeAsset`
- 请求参数：
```typescript
interface SendTrBeExchangeAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrBeExchangeAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的接受权益交换事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.BeExchangeAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 15 资产交换事件
### 15.1 创建资产交换事件
- 简介
  - 已被其他事件替代，暂不建议使用。
  - 把自己账户下的某种资产冻结到链上，等待其他持有某种指定资产的账户来交换
- 接口全称：`trToExchangeSpecAsset`
- 接口简写：`ttesa`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trToExchangeSpecAsset`
- 请求参数：
```typescript
interface TrToExchangeSpecAsset extends TransactionCommonParams{
    /**加密密钥组，如果填写了密钥，则接受资产交换的事件必须携带某个密钥生成的签名对 */
    ciphertexts?: string[];
}

```
- 返回参数：
```typescript
type TrToExchangeSpecAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的资产交换事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.ToExchangeSpecialAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 15.2 创建资产交换事件(带安全密钥)
- 接口全称：`trToExchangeSpecAssetWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trToExchangeSpecAssetWithSign`
- 请求参数：
```typescript
interface PackageTrToExchangeSpecAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrToExchangeSpecAssetRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 15.3 发送资产交换事件
- 接口全称：`toExchangeSpecAsset`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/toExchangeSpecAsset`
- 请求参数：
```typescript
interface SendTrToExchangeSpecAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrToExchangeSpecAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的资产交换事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.ToExchangeSpecialAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 16 接受资产交换事件
### 16.1 创建接受资产交换事件
- 简介
  - 已被其他事件替代，暂不建议使用。
  - 用自己账户下的某种资产换取链上其他人冻结的某种资产
- 接口全称：`trBeExchangeSpecAsset`
- 接口简写：`tbesa`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trBeExchangeSpecAsset`
- 请求参数：
```typescript
interface TrBeExchangeSpecAsset extends TransactionCommonParams{
    /**to 事件的签名，128 个字节的 16 进制字符串 */
    transactionSignature: string;
    /**加密密钥，如果资产交换事件填写了加密密钥，则必须携带某个资产交换事件指定密钥以生成密钥签名对 */
    ciphertext?: string;
}

```
- 返回参数：
```typescript
type TrBeExchangeSpecAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的接受资产交换事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.BeExchangeSpecialAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 16.2 创建接受资产交换事件(带安全密钥)
- 接口全称：`trBeExchangeSpecAssetWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trBeExchangeSpecAssetWithSign`
- 请求参数：
```typescript
interface PackageTrBeExchangeSpecAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrBeExchangeSpecAssetRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 16.3 发送接受资产交换事件
- 接口全称：`beExchangeSpecAsset`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/beExchangeSpecAsset`
- 请求参数：
```typescript
interface SendTrBeExchangeSpecAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrBeExchangeSpecAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的接受资产交换事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.BeExchangeSpecialAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 17 权益赠与事件（红包事件）
### 17.1 创建权益赠与事件（红包事件）
- 简介
  - 把账户下的一部分某种权益冻结到链上，免费赠送给某个人或者某些人
- 接口全称：`trGiftAsset`
- 接口简写：`tga`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trGiftAsset`
- 请求参数：
```typescript
interface TrGiftAsset extends TransactionCommonParams{
    /**加密密钥组，如果填写了密钥，则接收权益交换的事件必须携带某个密钥生成的签名对 */
    ciphertexts?: string[];
}

```
- 返回参数：
```typescript
type TrGiftAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的权益赠与事件（红包事事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.GiftAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 17.2 创建权益赠与事件（红包事件）(带安全密钥)
- 接口全称：`trGiftAssetWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trGiftAssetWithSign`
- 请求参数：
```typescript
interface PackageTrGiftAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrGiftAssetRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 17.3 发送权益赠与事件（红包事件）
- 接口全称：`giftAsset`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/giftAsset`
- 请求参数：
```typescript
interface SendTrGiftAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrGiftAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的权益赠与事件（红包事事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.GiftAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 18 接受权益赠与事件（抢红包事件）
### 18.1 创建接受权益赠与事件（抢红包事件）
- 简介
  - 接受链上其他人赠送的某种权益，通过本事件你可以免费获得一些其他人赠送的权益
- 接口全称：`trGrabAsset`
- 接口简写：`tgra`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trGrabAsset`
- 请求参数：
```typescript
interface TrGrabAsset extends TransactionCommonParams{
    /**接收的赠送权益数量，0-9 组成并且不包含小数点 */
    amount?: string;
    /**赠送事件所在的区块签名，128 个字节的 16 进制字符串 */
    blockSignature: string;
    /**赠送事件的签名，128 个字节的 16 进制字符串 */
    transactionSignature: string;
    /**加密密钥，如果权益赠送事件填写了加密密钥，则必须携带某个权益交换事件指定密钥以生成密钥签名对 */
    ciphertext?: string;
}

```
- 返回参数：
```typescript
type TrGrabAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的接收权益赠与事件（抢红包事事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.GrabAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 18.2 创建接受权益赠与事件（抢红包事件）(带安全密钥)
- 接口全称：`trGrabAssetWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trGrabAssetWithSign`
- 请求参数：
```typescript
interface PackageTrGrabAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrGrabAssetRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 18.3 发送接受权益赠与事件（抢红包事件）
- 接口全称：`grabAsset`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/grabAsset`
- 请求参数：
```typescript
interface SendTrGrabAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrGrabAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的接收权益赠与事件（抢红包事事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.GrabAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 19 权益委托事件
### 19.1 创建权益委托事件
- 简介
  - 实验性接口，暂不建议使用
  - 把某种资产冻结到链上，并且指定需要哪些账户确认以及确认数量
  - 确认数量达到指定个数时会把见证的资产转移到指定的账户下
- 接口全称：`trTrustAsset`
- 接口简写：`ttra`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trTrustAsset`
- 请求参数：
```typescript
interface TrTrustAsset extends TransactionCommonParams {
    /**委托权益所属链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic?: string;
    /**委托权益所属链名，小写字母组成，3-8 位 */
    sourceChainName?: string;
    /**委托权益类型，大写字母组成，3-5 个字符 */
    assetType?: string;
    /**委托的权益数量，0-9 组成并且不包含小数点，必须大于 0 */
    amount: string;
    /**签收时需要的委托人签名数量，0-9 组成，必须大于 0，最大值为指定的受托人数量 +2 */
    numberOfSignFor: number;
    /**指定的委托人地址数组，base58 编码的 16 进制字符串 */
    trustees: string[];
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId: string;
}

```
- 返回参数：
```typescript
type TrTrustAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的权益委托事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.TrustAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 19.2 创建权益委托事件(带安全密钥)
- 接口全称：`trTrustAssetWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trTrustAssetWithSign`
- 请求参数：
```typescript
interface PackageTrGrabAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrGrabAssetRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 19.3 发送权益委托事件
- 接口全称：`trustAsset`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/trustAsset`
- 请求参数：
```typescript
interface SendTrTrustAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrTrustAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的权益委托事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.TrustAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 20 签收权益委托事件
### 20.1 创建签收权益委托事件
- 简介
  - 实验性接口，暂不建议使用
  - 接受见证，满足条件时见证实物将自动解冻到指定账户下
- 接口全称：`trSignForAsset`
- 接口简写：`tsfa`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trSignForAsset`
- 请求参数：
```typescript
interface TrSignForAsset extends TransactionCommonParams{
    /**委托事件的签名，128 个字节的 16 进制字符串 */
    transactionSignature: string;
}

```
- 返回参数：
```typescript
type TrSignForAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的签收权益委托事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.SignForAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 20.2 创建签收权益委托事件(带安全密钥)
- 接口全称：`trSignForAssetWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trSignForAssetWithSign`
- 请求参数：
```typescript
interface PackageTrSignForAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrSignForAssetRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 20.3 发送签收权益委托事件
- 接口全称：`signForAsset`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/signForAsset`
- 请求参数：
```typescript
interface SendTrSignForAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrSignForAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的签收权益委托事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.SignForAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 21 权益迁出交易
### 21.1 创建权益迁出交易
- 简介
  - 实验性接口，暂不建议使用
  - 把生物链林架构下某条链的主权益迁出本链，发本事件的账户会被冻结，被冻结账户无法再发起任何事件
  - 账户只能持有主权益，并且只能迁出账户上所有的主权益
- 接口全称：`trEmigrateAsset`
- 接口简写：`tema`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trEmigrateAsset`
- 请求参数：
```typescript
interface TrEmigrateAsset extends TransactionCommonParams{
    /**凭证版本 */
    version: string;
    /**迁出凭证生成时间 Date.now().getTimes() */
    timestamp: number;
    /**迁出链的唯一标识 version+自定义格式，目前是 version/magic/chainName/genesisBlockSignature */
    fromChainId: string;
    /**迁入链的唯一标识 version+自定义格式，目前是 version/magic/chainName/genesisBlockSignature */
    toChainId: string;
    /**发起账户的唯一标识 version/address */
    fromId: string;
    /**接收账户的唯一标识 version/address */
    toId: string;
    /**迁出的权益：version/assetType */
    assetId: string;
    /**迁出的权益数量，0-9 组成并且不包含小数点，必须大于0 */
    assetPrealnum: string;
    /**发起账户签名 version/publicKey-signature/secondPublicKey-signSignature */
    signature: string;
    /**迁出链的签名 version/publicKey-signature/secondPublicKey-signSignature */
    fromAuthSignature: string;
}

```
- 返回参数：
```typescript
type TrEmigrateAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的权益迁出事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.EmigrateAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 22 权益迁入交易
### 22.1 创建权益迁入交易
- 简介
  - 实验性接口，暂不建议使用
  - 把生物链林架构下某条链的主权益迁入本链
- 接口全称：`trImmigrateAsset`
- 接口简写：`tima`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trImmigrateAsset`
- 请求参数：
```typescript
interface TrImmigrateAsset extends TransactionCommonParams{
    /**凭证版本 */
    version: string;
    /**迁出凭证生成时间 Date.now().getTimes() */
    timestamp: number;
    /**迁出链的唯一标识 version+自定义格式，目前是 version/magic/chainName/genesisBlockSignature */
    fromChainId: string;
    /**迁入链的唯一标识 version+自定义格式，目前是 version/magic/chainName/genesisBlockSignature */
    toChainId: string;
    /**发起账户的唯一标识 version/address */
    fromId: string;
    /**接收账户的唯一标识 version/address */
    toId: string;
    /**迁出的权益：version/assetType */
    assetId: string;
    /**迁出的权益数量，0-9 组成并且不包含小数点，必须大于0 */
    assetPrealnum: string;
    /**发起账户签名 version/publicKey-signature/secondPublicKey-signSignature */
    signature: string;
    /**迁出链的签名 version/publicKey-signature/secondPublicKey-signSignature */
    fromAuthSignature: string;
    /**迁入链的授权签名头 version */
    toAuthSignature: string;
}

```
- 返回参数：
```typescript
type TrImmigrateAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的权益迁入事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.ImmigrateAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 23 注册、注销位名事件
### 23.1 创建注册、注销位名事件
- 简介
  - 注册：在链上创建出一个唯一的位名，同过这个位名可以定位到某个地址或者某个 ip 或者经纬度等等（解析值），不能越级创建位名
  - 注销：删除链上某个位名，删除位名会同步删掉这个位名的解析值，不能越级注销位名
- 接口全称：`trLocationName`
- 接口简写：`tln`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trLocationName`
- 请求参数：
```typescript
interface TrLocationName extends TransactionCommonParams {
    /**位名，2-1024 个字符，每级位名最大长度为 128 个字符，一级位名只能是小写字母组成，二级及以上开头及结尾只能由小写字母或数字组成，中间可以包含下划线，根位名必须时本链链名 */
    name: string;
    /**操作类型，只能是 0 或 1，0 表示注册位名，1 表示注销位名 */
    operationType: number;
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId: string;
}

```
- 返回参数：
```typescript
type TrLocationNameRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的注册、注销位名事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.LocationNameAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 23.2 创建注册、注销位名事件(带安全密钥)
- 接口全称：`trLocationNameWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trLocationNameWithSign`
- 请求参数：
```typescript
interface PackageTrLocationNameParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrLocationNameRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 23.3 发送注册、注销位名事件
- 接口全称：`locationName`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/locationName`
- 请求参数：
```typescript
interface SendTrImmigrateAssetParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrImmigrateAssetRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的权益迁入事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.ImmigrateAssetAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 24 设置位名管理员事件
### 24.1 创建设置位名管理员事件
- 简介
  - 设置链上某个位名的管理员，管理员可以设置位名的解析值
- 接口全称：`trSetLnsManager`
- 接口简写：`tslm`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trSetLnsManager`
- 请求参数：
```typescript
interface TrSetLnsManager extends TransactionCommonParams {
    /**位名，2-1024 个字符，每级位名最大长度为 128 个字符，一级位名只能是小写字母组成，二级及以上开头及结尾只能由小写字母或数字组成，中间可以包含下划线，根位名必须时本链链名 */
    name: string;
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId: string;
}

```
- 返回参数：
```typescript
type TrSetLnsManagerRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的设置位名管理员事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.SetLnsManagerAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 24.2 创建设置位名管理员事件(带安全密钥)
- 接口全称：`trSetLnsManagerWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trSetLnsManagerWithSign`
- 请求参数：
```typescript
interface PackageTrSetLnsManagerParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrSetLnsManagerRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 24.3 发送设置位名管理员事件
- 接口全称：`setLnsManager`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/setLnsManager`
- 请求参数：
```typescript
interface SendTrSetLnsManagerParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrSetLnsManagerRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的设置位名管理员事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.SetLnsManagerAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 25 设置位名解析值事件
### 25.1 创建设置位名解析值事件
- 简介
  - 设置链上某个位名的解析值
- 接口全称：`trSetLnsRecordValue`
- 接口简写：`tslns`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trSetLnsRecordValue`
- 请求参数：
```typescript
interface TrSetLnsRecordValue extends TransactionCommonParams {
    /**位名，2-1024 个字符，每级位名最大长度为 128 个字符，一级位名只能是小写字母组成，二级及以上开头及结尾只能由小写字母或数字组成，中间可以包含下划线，根位名必须时本链链名 */
    name: string;
    /**操作类型，只能为 0 或 1 或 2，0 表示添加，1 表示删除，2 表示更新 */
    operationType: number;
    /**增加的解析值，解析值的类型只能为 1 或 2 或 3 或 4 或 0，1 表示 ipV4，2 表示 ipV6，3 表示经纬度，4 表示链上账户地址，0 表示自定义的任意类型，可选，操作类型为 0 或 2 时必填 */
    addRecord?: string[];
    /**删除的解析值，解析值的类型只能为 1 或 2 或 3 或 4 或 0，1 表示 ipV4，2 表示 ipV6，3 表示经纬度，4 表示链上账户地址，0 表示自定义的任意类型，可选，操作类型为 1 或 2 时必填 */
    deleteRecord?: string[];
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId: string;
}

```
- 返回参数：
```typescript
type TrSetLnsRecordValueRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的设置位名解析值事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.SetLnsRecordValueAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 25.2 创建设置位名解析值事件(带安全密钥)
- 接口全称：`trSetLnsRecordValueWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trSetLnsRecordValueWithSign`
- 请求参数：
```typescript
interface PackageTrSetLnsRecordValueParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrSetLnsRecordValueRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 25.3 发送设置位名解析值事件
- 接口全称：`setLnsRecordValue`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/setLnsRecordValue`
- 请求参数：
```typescript
interface SendTrSetLnsRecordValueParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrSetLnsRecordValueRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的设置位名解析值事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.SetLnsRecordValueAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 26 发行资产权益模板事件
### 26.1 创建发行资产权益模板事件
- 简介
  - 用冻结资产的方式在链上创建某个资产权益模板，发送这个事件会冻结账户，被冻结的账户无法再发起任何事件
  - 对应现实中某种类型的实物大类，比如画，电影，小说等等，并且设定这个大类下的资产权益的一些属性
- 接口全称：`trIssueEntityFactory`
- 接口简写：`tief`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trIssueEntityFactory`
- 请求参数：
```typescript
interface TrIssueEntityFactory extends TransactionCommonParams {
    /**非同质资产模板 */
    factoryId: string;
    /**允许发行的非同质资产数量 */
    entityPrealnum: string;
    /**发行非同质资产时冻结的主权益数量，销毁时解冻 */
    entityFrozenAssetPrealnum: string;
    /**购买模板使用全的主权益数量 */
    purchaseAssetPrealnum: string;
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId: string;
}

```
- 返回参数：
```typescript
type TrIssueEntityFactoryRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的发行资产权益模板事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.IssueEntityFactoryAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 26.2 创建发行资产权益模板事件(带安全密钥)
- 接口全称：`trIssueEntityFactoryWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trIssueEntityFactoryWithSign`
- 请求参数：
```typescript
interface PackageTrIssueEntityFactoryParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrIssueEntityFactoryRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 26.3 发送发行资产权益模板事件
- 接口全称：`issueEntityFactory`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/issueEntityFactory`
- 请求参数：
```typescript
interface SendTrIssueEntityFactoryParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrIssueEntityFactoryRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的发行资产权益模板事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.IssueEntityFactoryAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 27 发行资产权益模板事件
### 27.1 创建发行资产权益模板事件
- 简介
  - 用销毁资产的方式在链上创建某个资产权益模板，发送这个事件会销毁账户上对应数量的主权益
  - 对应现实中某种类型的实物大类，比如画，电影，小说等等，并且设定这个大类下的资产权益的一些属性
- 接口全称：`trIssueEntityFactoryV1`
- 接口简写：`tief1`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trIssueEntityFactoryV1`
- 请求参数：
```typescript
interface TrIssueEntityFactory extends TransactionCommonParams {
    /**非同质资产模板 */
    factoryId: string;
    /**允许发行的非同质资产数量 */
    entityPrealnum: string;
    /**发行非同质资产时冻结的主权益数量，销毁时解冻 */
    entityFrozenAssetPrealnum: string;
    /**购买模板使用全的主权益数量 */
    purchaseAssetPrealnum: string;
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId: string;
}

```
- 返回参数：
```typescript
type TrIssueEntityFactoryRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的发行资产权益模板事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.IssueEntityFactoryAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 27.2 创建发行资产权益模板事件(带安全密钥)
- 接口全称：`trIssueEntityFactoryWithSignV1`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trIssueEntityFactoryWithSignV1`
- 请求参数：无
- 返回参数：无
### 27.3 发送发行资产权益模板事件
- 接口全称：`issueEntityFactoryV1`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/issueEntityFactoryV1`
- 请求参数：
```typescript
interface SendTrIssueEntityFactoryParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrIssueEntityFactoryRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的发行资产权益模板事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.IssueEntityFactoryAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 28 发行资产权益事件
### 28.1 创建发行资产权益事件
- 简介
  - 在链上创建某个资产权益，资产权益可以用来绑定现实中的某个实物，比如一幅画或者一瓶酒
  - 资产权益需要指定一个资产权益模板
- 接口全称：`trIssueEntity`
- 接口简写：`tie`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trIssueEntity`
- 请求参数：
```typescript
interface TrIssueEntityFactory extends TransactionCommonParams {
    /**非同质权益模板，3-15 个字符，小写字母或数字组成 */
    factoryId: string;
    /**不包含非同质权益模板的非同质权益，3-30 个字符，小写字母或数字组成 */
    entityId: string;
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId: string;
}

```
- 返回参数：
```typescript
type TrIssueEntityFactoryRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的发行资产权益模板事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.IssueEntityFactoryAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 28.2 创建发行资产权益事件(带安全密钥)
- 接口全称：`trIssueEntityWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trIssueEntityWithSign`
- 请求参数：
```typescript
interface PackageTrIssueEntityParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrIssueEntityRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 28.3 发送发行资产权益事件
- 接口全称：`issueEntity`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/issueEntity`
- 请求参数：
```typescript
interface SendTrIssueEntityFactoryParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrIssueEntityFactoryRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的发行资产权益模板事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.IssueEntityFactoryAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 29 销毁资产权益事件
### 29.1 创建销毁资产权益事件
- 简介
  - 销毁自己账户上的某个资产权益，销毁后这个资产权益不能再在链上流通
- 接口全称：`trDestoryEntity`
- 接口简写：`tde`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trDestoryEntity`
- 请求参数：
```typescript
interface TrDestoryEntity extends TransactionCommonParams{
    /**要销毁的非同质权益 */
    entityId: string;
}

```
- 返回参数：无
### 29.2 创建销毁资产权益事件(带安全密钥)
- 接口全称：`trDestoryEntityWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trDestoryEntityWithSign`
- 请求参数：
```typescript
interface PackageTrDestoryEntityParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：无
### 29.3 发送销毁资产权益事件
- 接口全称：`destoryEntity`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/destoryEntity`
- 请求参数：
```typescript
interface SendTrDestoryEntityParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：无
## 30 任意资产交换事件
### 30.1 创建任意资产交换事件
- 简介
  - 链上任意类型的资产互换（一换一），比如 用 1 个 主权益 换 1 个链域名
  - 冻结自己帐户下的某个资产，等待持有指定资产的同意换购
- 接口全称：`trToExchangeAny`
- 接口简写：`tteay`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trToExchangeAny`
- 请求参数：
```typescript
interface TrToExchangeAny extends TransactionCommonParams{
    /**用于交换的资产/权益来源链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    toExchangeSource: string;
    /**被交换的资产/权益来源链网络标识符，大写字母或数字组成，5个字符，最后一位是校验位 */
    beExchangeSource: string;
    /**用于交换的资产/权益来源链名，小写字母组成，5-10 位 */
    toExchangeChainName: string;
    /**被交换的资产/权益来源链名，小写字母组成，5-10 位 */
    beExchangeChainName: string;
    /**用于交换的资产所属类型，1 dappid，2 位名 3 entityId 4 权益 5 */
    toExchangeParentAssetType: number;
    /**被交换的资产所属类型，1 dappid，2 位名 3 entityId 4 权益 5 */
    beExchangeParentAssetType: number;
    /**用于交换的权益名，可能为 entityId, dappid，位名或者权益名 */
    toExchangeAssetType: string;
    /**被交换的资产/权益名，可能为 entityId, dappid，位名或者权益名 */
    beExchangeAssetType: string;
    /**用于交换或交换得到的权益数量，0-9 组成并且不包含小数点 */
    toExchangeAssetPrealnum: string;
    /**被交换或交换得到的权益数量，0-9 组成并且不包含小数点 */
    beExchangeAssetPrealnum?: string;
    /**用于交换的资产权重 */
    toExchangeAssetWeight?: string;
    /**被交换的资产权重 */
    beExchangeAssetWeight?: string;
    /**发行 entityId 事件的签名，128 个字节的 16 进制字符串 */
    transactionSignature?: string;
    /**加密密钥组，如果填写了密钥，则接受资产交换的事件必须携带某个密钥生成的签名对 */
    ciphertexts?: string[];
}

```
- 返回参数：
```typescript
type TrToExchangeAnyRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的任意资产交换事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.ToExchangeAnyAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 30.2 创建任意资产交换事件(带安全密钥)
- 接口全称：`trToExchangeAnyWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trToExchangeAnyWithSign`
- 请求参数：
```typescript
interface PackageTrToExchangeAnyParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrToExchangeAnyRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 30.3 发送任意资产交换事件
- 接口全称：`toExchangeAny`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/toExchangeAny`
- 请求参数：
```typescript
interface SendTrToExchangeAnyParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrToExchangeAnyRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的任意资产交换事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.ToExchangeAnyAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 31 接受任意资产交换事件
### 31.1 创建接受任意资产交换事件
- 简介
  - 接受任意任意类型的资产互换（一换一）
  - 用自己账上的某种资产换取链上冻结的某种资产
- 接口全称：`trBeExchangeAny`
- 接口简写：`tbeay`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trBeExchangeAny`
- 请求参数：
```typescript
interface TrBeExchangeAny extends TransactionCommonParams{
    /**发行 entityId 事件的签名，128 个字节的 16 进制字符串 */
    issueEntityTransactionSignature?: string;
    /**to 事件的签名，128 个字节的 16 进制字符串 */
    transactionSignature: string;
    /**用于交换或交换得到的权益数量，0-9 组成并且不包含小数点 */
    toExchangeAssetPrealnum: string;
    /**被交换或交换得到的权益数量，0-9 组成并且不包含小数点 */
    beExchangeAssetPrealnum?: string;
    /**加密密钥，如果资产交换事件填写了加密密钥，则必须携带某个资产交换事件指定密钥以生成密钥签名对 */
    ciphertext?: string;
    /**被交换的资产名 */
    assetType?: string;
}

```
- 返回参数：
```typescript
type TrBeExchangeAnyRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的接受任意资产交换事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.BeExchangeAnyAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
### 31.2 创建接受任意资产交换事件(带安全密钥)
- 接口全称：`trBeExchangeAnyWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trBeExchangeAnyWithSign`
- 请求参数：
```typescript
interface PackageTrBeExchangeAnyParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：
```typescript
type PackageTrBeExchangeAnyRespParam = {
    /**成功 */
    success: true;
    /**打包结果 */
    result: {
        /**事件主体生成的buffer，base64 字符串 */
        buffer: string;
    };
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
}

```
### 31.3 发送接受任意资产交换事件
- 接口全称：`beExchangeAny`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/beExchangeAny`
- 请求参数：
```typescript
interface SendTrBeExchangeAnyParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：
```typescript
type SendTrBeExchangeAnyRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的接受任意资产交换事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.BeExchangeAnyAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
## 32 任意资产转移事件
### 32.1 创建任意资产转移事件
- 简介
  - 把账户下的某种资产（entity，dappid，链域名或者某种权益）转移到指定的账户
- 接口全称：`trTransferAny`
- 接口简写：`ttay`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trTransferAny`
- 请求参数：
```typescript
interface TrTransferAny extends TransactionCommonParams{
    /**发行 entityId 事件的签名，128 个字节的 16 进制字符串 */
    transactionSignature?: string;
    /**转移的权益数量，0-9 组成并且不包含小数点，必须大于 0 */
    amount?: string;
    /**转移的权益类型，大写字母组成，3-8 个字符 */
    assetType?: string;
    /**转移的资产所属类型，1 dappid，2 位名 3 entityId 4 权益 */
    parentAssetType?: import("./").PARENT_ASSET_TYPE;
    /**转移的权益所属链名，小写字母组成，5-10 位 */
    sourceChainName?: string;
    /**转移的权益所属链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic?: string;
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId: string;
}

```
- 返回参数：无
### 32.2 创建任意资产转移事件(带安全密钥)
- 接口全称：`trTransferAnyWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trTransferAnyWithSign`
- 请求参数：
```typescript
interface PackageTrTransferAnyParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：无
### 32.3 发送任意资产转移事件
- 接口全称：`transferAny`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/transferAny`
- 请求参数：
```typescript
interface SendTrTransferAnyParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：无
## 33 任意资产赠与事件
### 33.1 创建任意资产赠与事件
- 简介
  - 把账户下的某种资产冻结到链上，免费送给某个账户或者指定的账户
- 接口全称：`trGiftAny`
- 接口简写：`tgay`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trGiftAny`
- 请求参数：
```typescript
interface TrGiftAny extends TransactionCommonParams{
    /**发行 entityId 的事件的签名，128 个字节的 16 进制字符串 */
    transactionSignature?: string;
    /**赠送的权益所属链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
    sourceChainMagic?: string;
    /**赠送的权益所属链名，小写字母组成，5-10 位 */
    sourceChainName?: string;
    /**转移的资产所属类型，1 dappid，2 位名 3 entityId 4 权益 */
    parentAssetType?: import("./").PARENT_ASSET_TYPE;
    /**赠送的权益类型，大写字母组成，3-8 个字符 */
    assetType?: string;
    /**赠送的权益数量，0-9 组成并且不包含小数点，必须大于 0 */
    amount?: string;
    /**可被接收的次数，0-9 组成并且不包含小数点，必须大于 0 */
    totalGrabableTimes?: number;
    /**从权益赠送事件发起到开始被签收的区块间隔，0-9 组成并且不包含小数点，可选，必须小于等于事件的有效期 */
    numberOfBeginUnfrozenBlocks?: number;
    /**接收规则, 只能为 0，1 或 2，0 表示平均分配，1 表示根据任意账户的地址的随机分配，2 表示根据接收者列表中账户地址的随机分配 */
    giftDistributionRule?: number;
    /**加密密钥组，如果填写了密钥，则接收权益交换的事件必须携带某个密钥生成的签名对 */
    ciphertexts?: string[];
    /**收税信息 */
    taxInformation?: {
        /**收税人 */
        taxCollector: string;
        /**缴纳数量 */
        taxAssetPrealnum: string;
    };
}

```
- 返回参数：无
### 33.2 创建任意资产赠与事件(带安全密钥)
- 接口全称：`trGiftAnyWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trGiftAnyWithSign`
- 请求参数：
```typescript
interface PackageTrGiftAnyParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：无
### 33.3 发送任意资产赠与事件
- 接口全称：`giftAny`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/giftAny`
- 请求参数：
```typescript
interface SendTrGiftAnyParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：无
## 34 接受任意资产赠与事件
### 34.1 创建接受任意资产赠与事件
- 简介
  - 接受任意资产赠送，通过这笔事件，你可以免费获得链上其他账户赠送的某种资产
- 接口全称：`trGrabAny`
- 接口简写：`tgray`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trGrabAny`
- 请求参数：
```typescript
interface TrGrabAny extends TransactionCommonParams{
    /**接收的赠送权益数量，0-9 组成并且不包含小数点 */
    amount?: string;
    /**赠送事件所在的区块签名，128 个字节的 16 进制字符串 */
    blockSignature: string;
    /**赠送事件的签名，128 个字节的 16 进制字符串 */
    transactionSignature: string;
    /**加密密钥，如果权益赠送事件填写了加密密钥，则必须携带某个权益交换事件指定密钥以生成密钥签名对 */
    ciphertext?: string;
}

```
- 返回参数：无
### 34.2 创建接受任意资产赠与事件(带安全密钥)
- 接口全称：`trGrabAnyWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trGrabAnyWithSign`
- 请求参数：
```typescript
interface PackageTrGrabAnyParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：无
### 34.3 发送接受任意资产赠与事件
- 接口全称：`grabAny`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/grabAny`
- 请求参数：
```typescript
interface SendTrGrabAnyParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：无
## 35 批量发行资产权益事件
### 35.1 创建批量发行资产权益事件
- 简介
  - 批量发行 entity
- 接口全称：`trIssueEntityMultiV1`
- 接口简写：`tiem1`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trIssueEntityMultiV1`
- 请求参数：
```typescript
interface TrIssueEntityMultiV1 extends TransactionCommonParams{
    /**非同质权益模板，3-15 个字符，小写字母或数字组成 */
    factoryId: string;
    entityStructList: {
        /**不包含非同质权益模板的非同质权益，3-30 个字符，小写字母或数字组成 */
        entityId: string;
        /**非同质资产流通需要缴纳的版税 */
        taxAssetPrealnum?: string;
    }[];
}

```
- 返回参数：无
### 35.2 创建批量发行资产权益事件(带安全密钥)
- 接口全称：`trIssueEntityMultiWithSignV1`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trIssueEntityMultiWithSignV1`
- 请求参数：
```typescript
interface PackageTrIssueEntityMultiV1Param{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：无
### 35.3 发送批量发行资产权益事件
- 接口全称：`issueEntityMultiV1`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/issueEntityMultiV1`
- 请求参数：
```typescript
interface SendTrIssueEntityMultiV1Param{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：无
## 36 批量任意资产交换事件
### 36.1 创建批量任意资产交换事件
- 简介
  - 链上任意类型的资产互换（多换一），比如 用 1 个 主权益 和 1 个 dappid 换 1 个链域名
  - 冻结自己帐户下的一个或者多个资产，等待持有指定资产的同意换购
- 接口全称：`trToExchangeAnyMulti`
- 接口简写：`tteaym`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trToExchangeAnyMulti`
- 请求参数：
```typescript
interface TrToExchangeAnyMulti extends TransactionCommonParams{
    /**用于交换的资产 */
    toExchangeAssets: {
        /**用于交换的资产来源链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
        toExchangeSource?: string;
        /**用于交换的资产来源链名，小写字母组成，5-10 位 */
        toExchangeChainName?: string;
        /**用于交换的资产名 */
        toExchangeAssetType: string;
        /**用于交换的资产数量，0-9 组成并且不包含小数点 */
        toExchangeAssetPrealnum: string;
        /**交换比例，同质权益交换时必填 */
        assetExchangeWeightRatio?: {
            /**用于交换的权益权重 */
            toExchangeAssetWeight: string;
            /**被交换的权益权重 */
            beExchangeAssetWeight: string;
        };
        /**收税信息 */
        taxInformation?: {
            /**收税人 */
            taxCollector: string;
            /**缴纳数量 */
            taxAssetPrealnum: string;
        };
    }[];
    /**被交换的资产 */
    beExchangeAsset: {
        /**被交换的资产来源链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
        beExchangeSource?: string;
        /**被交换的资产来源链名，小写字母组成，5-10 位 */
        beExchangeChainName?: string;
        /**被交换的资产名 */
        beExchangeAssetType: string;
        /**被交换的资产数量，0-9 组成并且不包含小数点，非同质资产交换时必填 */
        beExchangeAssetPrealnum?: string;
        /**收税信息 */
        taxInformation?: {
            /**收税人 */
            taxCollector: string;
            /**缴纳数量 */
            taxAssetPrealnum: string;
        };
    };
    /**加密密钥组，如果填写了密钥，则接收权益交换的事件必须携带某个密钥生成的签名对 */
    ciphertexts?: string[];
}

```
- 返回参数：无
### 36.2 创建批量任意资产交换事件(带安全密钥)
- 接口全称：`trToExchangeAnyMultiWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trToExchangeAnyMultiWithSign`
- 请求参数：
```typescript
interface PackageTrToExchangeAnyMultiParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：无
### 36.3 发送批量任意资产交换事件
- 接口全称：`toExchangeAnyMulti`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/toExchangeAnyMulti`
- 请求参数：
```typescript
interface SendTrToExchangeAnyMultiParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：无
## 37 接受批量任意资产交换事件
### 37.1 创建接受批量任意资产交换事件
- 简介
  - 接受任意任意类型的资产互换（多换一）
  - 用自己账上的某种资产换取链上冻结的某些资产
- 接口全称：`trBeExchangeAnyMulti`
- 接口简写：`tbeaym`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trBeExchangeAnyMulti`
- 请求参数：
```typescript
interface TrBeExchangeAnyMulti extends TransactionCommonParams{
    /**批量任意资产交换事件的签名，128 个字节的 16 进制字符串 */
    transactionSignature: string;
    /**用于交换的资产 */
    toExchangeAssets: {
        /**用于交换的资产来源链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
        toExchangeSource?: string;
        /**用于交换的资产来源链名，小写字母组成，5-10 位 */
        toExchangeChainName?: string;
        /**用于交换的资产名 */
        toExchangeAssetType: string;
        /**用于交换的资产数量，0-9 组成并且不包含小数点 */
        toExchangeAssetPrealnum: string;
        /**交换比例，同质权益交换时必填 */
        assetExchangeWeightRatio?: {
            /**用于交换的权益权重 */
            toExchangeAssetWeight: string;
            /**被交换的权益权重 */
            beExchangeAssetWeight: string;
        };
        /**收税信息 */
        taxInformation?: {
            /**收税人 */
            taxCollector: string;
            /**缴纳数量 */
            taxAssetPrealnum: string;
        };
    }[];
    /**被交换的资产 */
    beExchangeAsset: {
        /**被交换的资产来源链网络标识符，大写字母或数字组成，5 个字符，最后一位是校验位 */
        beExchangeSource?: string;
        /**被交换的资产来源链名，小写字母组成，5-10 位 */
        beExchangeChainName?: string;
        /**被交换的资产名 */
        beExchangeAssetType: string;
        /**被交换的资产数量，0-9 组成并且不包含小数点，非同质资产交换时必填 */
        beExchangeAssetPrealnum?: string;
        /**收税信息 */
        taxInformation?: {
            /**收税人 */
            taxCollector: string;
            /**缴纳数量 */
            taxAssetPrealnum: string;
        };
    };
    /**加密密钥，如果权益赠送事件填写了加密密钥，则必须携带某个权益交换事件指定密钥以生成密钥签名对 */
    ciphertext?: string;
}

```
- 返回参数：无
### 37.2 创建接受批量任意资产交换事件(带安全密钥)
- 接口全称：`trBeExchangeAnyMultiWithSign`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trBeExchangeAnyMultiWithSign`
- 请求参数：
```typescript
interface PackageTrBeExchangeAnyMultiParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
}

```
- 返回参数：无
### 37.3 发送接受批量任意资产交换事件
- 接口全称：`beExchangeAnyMulti`
- 接口简写：`无`
- 可调用方式：`Http,Websocket`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/send/beExchangeAnyMulti`
- 请求参数：
```typescript
interface SendTrBeExchangeAnyMultiParam{
    /**事件主体生成的buffer，base64 字符串 */
    buffer: string;
    /**事件签名 */
    signature: string;
    /**事件安全签名 */
    signSignature?: string;
}

```
- 返回参数：无
## 38 注册链事件
### 38.1 创建注册链事件
- 简介
  - 实验性接口，暂不建议使用
  - 把生物链林架构下的某条链注册到链上，为主权益跨链做准备
- 接口全称：`trRegisterChain`
- 接口简写：`trc`
- 可调用方式：`Http,Websocket,命令行`
- 调用方法：`post`
- 接口`url`地址：`/api/transaction/trRegisterChain`
- 请求参数：
```typescript
interface TrRegisterChain extends TransactionCommonParams {
    /**创世块 */
    genesisBlock: string;
    /**事件的接收账户地址，base58 编码的 16 进制字符串 */
    recipientId: string;
}

```
- 返回参数：
```typescript
type TrRegisterChainRespParam = {
    /**成功 */
    success: true;
    /**最小手续费 */
    minFee: number;
    /**完整的注册链事件 */
    result: BFMetaCore.TransactionJSON<BFMetaCore.RegisterChainAssetJSON>;
} | {
    /**失败 */
    success: false;
    /**错误信息 */
    message: string;
    /**最小手续费 */
    minFee: number;
    /**错误码 */
    code?: string;
}

```
