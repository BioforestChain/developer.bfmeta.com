# Node management

You can manage nodes in the following two ways:

-Send commands through the command line window for management
-Managed by the BFM node manager provided by the BFMeta

This chapter only introduces how to manage your nodes through the command line window. If you want to learn how to manage nodes in the BFM Node Manager, please refer to [\<BFM Node Manager Tutorial\>](/en/Tutorial/Node_manager_tutorial/index.md)


## Modify node password
The node password can be modified by executing the following command:

```
./bcf --setSystemKey systemKeyOld="your old password",systemKeyNew="your new password"

```

> Note: After installing the BCF node program, it is strongly recommended that you change the node password first.

## Mining block

By executing the following commands, you can participate in the election of block Miners, and you will be selected as a Miner.

1. Bind the identity (address) to the BCF node

For details, please refer to [\<Node Identity Binding\>](/en/Tutorials/PC_full_node_tutorial/Linux/authorize.md)

> Note: Please ensure that the bound address has enough main chain digital rights to pay the event chain fee.

2. Apply to become a trustee

You need to initiate an "application to become a trustee" event, set your bound address as a trustee, and execute the following command:

```
./bcf --trUsername secret="123",fee=100,alias="username" // set the username
./bcf --trDelegate secret="123",fee=100 // Apply to become a trustee
```
3. Turn on receiving votes

After becoming a trustee, you need to initiate a "receive vote" event to ensure that your address can be voted to obtain votes. Only enough votes and online rate can guarantee that you can be selected as the next block forger.

The command to initiate a "receive vote" event is as follows:

```
./bcf --trAcceptVote sercet="123",fee=100

```

4. Open block for the node

When your node is ready (the block is synchronized to the highest level, the node is in normal state, and the network is good), you can start block printing for your node by executing the following command:

```
./bcf --setSystemConfig verifyType="001",verifyKey="SystemKey",generateBlockEnale=true

```
At this point, your node has started the block forging campaign. Once selected, it means that your node has become the next round of block forging and will participate in the next round of block forging work.

> Note: When your node is about to be upgraded or restarted, it is recommended to turn off block-breaking first, and then turn on block-breaking after the node returns to normal, so as not to affect your probability of being selected as a miner.



## On-chain voting governance

1. Bind the identity (address) to the BCF node

For details, please refer to [\<Node Identity Binding\>](/en/Tutorials/PC_full_node_tutorial/Linux/authorize.md)

> Note: Please ensure that the bound address has enough main chain digital rights to pay the event chain fee.

2. Turn on automatic/manual voting

-Automatic voting

Execute the following commands through the command line window to enable automatic voting for nodes:

```
./bcf --setSystemConfig verifyType="001",verifyKey="SystemKey",autoVoteEnable=true

```

-Manual voting
If users need to vote for a trustee with a known address, they can initiate a "voting" event through the command line

```
./bcf --trVote secret="123",fee=500,equity=0,recipientId=address

```




## Update node program

Currently, users can only update nodes through the BFMeta node manager.




## Restart the node program

Currently, users can only restart the node through the BFMeta node manager.

> Note: After the node restarts, the identity (address) originally bound to the node will be unbound, and the user needs to bind again. For details, please refer to [\<Node Identity Binding\>](/en/Tutorials/PC_full_node_tutorial/Linux/authorize.md)
