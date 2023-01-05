# Node identity binding

When you complete the node deployment, the system will automatically start the node program-BCF.
When the node is started for the first time or when the node is in an unauthorized state, the node authorization needs to be completed before the node can be operated and managed normally.


## Node identity binding


When the user successfully starts the BCF, the interface will give a prompt "Waiting to bind the address". You need to bind an identity (address) to your node so that your node can connect to the blockchain network for block synchronization and participation On-chain governance.

You can complete the node identity binding in the following two ways:

- Send commands through the command line window to complete identity binding

1. Start the node program-BCF;
2. Open the Linux command window;
3. Enter the following command in the command window:

```
cd /data/bfmeta/
./bcf -ba systemSecret="Your node password", delegateSecret="The master password of the identity you plan to bind"
```

> Note: In the above command, the content of double quotes needs to be replaced with the corresponding content

4. When the node program-BCF interface gives the bound identity (address) information, it means that the node's identity (address) is successfully bound, as shown in the following figure:
![bindingaddress1](./images/bindingaddress1.png)


- Complete identity binding through node manager

1. Please refer to the BFM Node Manager Tutorial to get more information.



When the node completes the above identity binding, it means that your node is connected to the BFChainMeta blockchain network and can directly participate in on-chain governance and block synchronization.