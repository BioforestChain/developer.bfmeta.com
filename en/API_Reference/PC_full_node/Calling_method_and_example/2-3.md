# SDK

- The following code is implemented，taking typescript as an example，the method to use\ @bfmeta/node-sdk to call the interface, see https://github.com/BioforestChain/BFMeta-Node-SDK


```ts
import { BFMetaSDK, REQUEST_PROTOCOL} from "@bfmeta/node-sdk";

// Can also create config/config.json in the run directory, fill in the following content，new means there is no need to pass the parameter

const config: BFMetaNodeSDK.ApiConfig = {
    node: {
        /**Node ip, default value [127.0.0.1] */
        ip: "127.0.0.1",
        /**Node port number, default value 9003 */
        port: 9003,
    },
    //  "The duration of the request timeout, unit is ms, default value is 10000",
    requestTimeOut: 10000,
    // "Request protocol, http || websocket, default value is websocket",
    requestProtocol: REQUEST_PROTOCOL.WEBSOCKET,
};

// The implementation here is based on the local environment, refer to the following
import { CryptoHelper } from "@bfmeta/node-sdk/build/test/helpers/cryptoHelper";
const cryptoHelper = new CryptoHelper();
const bfmetaSDK = new BFMetaSDK({ netType: "testnet", cryptoHelper }, config);

(async () => {
    // Generate public and private key pairs according to mnemonics
    let keypair = await bfmetaSDK.bfchainSignUtil.createKeypair("your secret");

    // This is the address of the account
    const address = await bfmetaSDK.bfchainSignUtil.getAddressFromPublicKey(keypair.publicKey);
    // This is the public key (string) of the account.
    const publicKey = keypair.publicKey.toString("hex");
    // Obtain the latest height of the node
    const lastblockResult = await bfmetaSDK.api.basic.getLastBlock();
    // Get blocks based on conditions
    const blockResult = await bfmetaSDK.api.basic.getBlock({});
    // Get transaction based on conditions
    const transactionResult = await bfmetaSDK.api.basic.getTransactions({});
    
    const createResult = await bfmetaSDK.api.transaction.createTransferAsset({
        // Transfer amount, 1 represents the smallest unit. 100000000 = 1 BFMTEST
        amount: "100000000000",
        // The type of asset transferred, which can be null, defaults to the main equity of the current chain
        assetType: "BFMTEST",
        // Receiving address
        recipientId: address,
        // The public key of the initiating address
        publicKey,
        // Fee
        fee: "2000",
        // Initiating height, must be set based on the height of the connected node. Nodes can only process the transactions within the legal height
        applyBlockHeight: 1,
        // Failure height, is used together with the initiating height. If the initiating height is 1000, and the failure height is 10, then this transaction can only be confirmed within the height of 1000~1010. These two fields can constrain the effective height range of this transaction. The maximum value of this field is determined by consensus in the Genesis block.
        numberOfEffectiveBlocks: 10
    });
    if (!createResult.success) {
        throw createResult;
    }
    const buffer = createResult.result.buffer;
    const bytes = Buffer.from(buffer, "base64");
    // Generate signature
    const signature = (await bfmetaSDK.bfchainSignUtil.detachedSign(bytes, keypair.secretKey)).toString("hex");
    // Broadcast transaction
    const broadcastResult = await bfmetaSDK.api.transaction.broadcastTransferAsset({
        buffer,
        signature,
    });
    if (!broadcastResult.success) {
        throw broadcastResult;
    }
    // The transaction body will be returned. See the actual Return Type for the format of the transaction body.
    console.log(broadcastResult);
    // Succeeded in broadcasting transaction to node 
})();

```