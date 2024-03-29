# SDK

- 下列代码实现，以typescript为例，使用\ @bfmeta/node-sdk，来调用接口的方式,详见 https://github.com/BioforestChain/BFMeta-Node-SDK
- 文档中的参数若与接口类型有出入，以接口类型为准。


```ts
import { BFMetaSDK, REQUEST_PROTOCOL} from "@bfmeta/node-sdk";

// 也可以再运行目录下建 config/config.json 填入以下内容，new 的时候就不用传参

const config: BFMetaNodeSDK.ApiConfig = {
    node: {
        /**节点 ip, 默认值 [127.0.0.1] */
        ip: "127.0.0.1",
        /**节点端口号, 默认值 9003 */
        port: 9003,
    },
    //  "请求超时时间, 单位 ms, 默认 10000",
    requestTimeOut: 10000,
    // "请求协议, http || websocket, 默认值 websocket",
    requestProtocol: REQUEST_PROTOCOL.WEBSOCKET,
};

// 这里根据本地环境去实现，可参考以下
import { CryptoHelper } from "@bfmeta/node-sdk/build/test/helpers/cryptoHelper";
const cryptoHelper = new CryptoHelper();
const bfmetaSDK = new BFMetaSDK({ netType: "testnet", cryptoHelper }, config);

(async () => {
    // 根据助记词生成公私钥对
    let keypair = await bfmetaSDK.bfchainSignUtil.createKeypair("your secret");

    // 这是该账户的地址
    const address = await bfmetaSDK.bfchainSignUtil.getAddressFromPublicKey(keypair.publicKey);
    // 这是该账户的公钥(string)
    const publicKey = keypair.publicKey.toString("hex");
    // 获取节点最新高度
    const lastblockResult = await bfmetaSDK.api.basic.getLastBlock();
    // 根据条件获取区块
    const blockResult = await bfmetaSDK.api.basic.getBlock({});
    // 根据条件获取交易
    const transactionResult = await bfmetaSDK.api.basic.getTransactions({});
    
    const createResult = await bfmetaSDK.api.transaction.createTransferAsset({
        // 转账金额 1 代表最小单位。100000000 = 1 BFMTEST
        amount: "100000000000",
        // 转移的资产类型，可为空，默认为当前链的主权益
        assetType: "BFMTEST",
        // 接收地址
        recipientId: address,
        // 发起地址的公钥
        publicKey,
        // 手续费
        fee: "2000",
        // 发起高度，需要根据连接节点的高度设置。节点只能处理合法高度内的交易
        applyBlockHeight: 1,
        // 失效高度，同发起高度一起使用。 若发起高度为1000，失效高度为10.那么这笔交易只能在 1000~1010的高度内被确认。这两个字段能够约束本笔交易的有效高度区间。此字段的最大值为创世块内共识决定。
        numberOfEffectiveBlocks: 10
    });
    if (!createResult.success) {
        throw createResult;
    }
    const buffer = createResult.result.buffer;
    const bytes = Buffer.from(buffer, "base64");
    // 生成签名
    const signature = (await bfmetaSDK.bfchainSignUtil.detachedSign(bytes, keypair.secretKey)).toString("hex");
    // 广播交易
    const broadcastResult = await bfmetaSDK.api.transaction.broadcastTransferAsset({
        buffer,
        signature,
    });
    if (!broadcastResult.success) {
        throw broadcastResult;
    }
    // 将返回交易体，交易体的格式见实际返回类型。
    console.log(broadcastResult);
    // 广播交易到节点成功
})();

```