# 节点管理

您可以通过如下两种方式管理节点：

- 通过命令行窗口发送命令进行管理
- 通过生物链林提供的节点管理器管理

本章节仅介绍如何通过命令行窗口管理您的节点。如您想学习如何在节点管理器中管理节点，请参见[BFM 节点管理器教程](https://serviceapi.instinct.one/marketplace/public/file/ext?path=/default/NodeManagerOperationManual.pdf)


## 修改节点密码

> 注:在执行如下命令前，请先确保您的BCF节点软件已经启动。



通过执行如下命令，可完成节点密码修改：

```

cd /data/bfmeta/    //执行该命令将路径切至BFChainMeta主目录下

./bcf --setSystemKey systemKeyOld="您的节点旧密码",systemKeyNew="您要设置的新密码"       //执行节点密码修改命令，命令中双引号中文内容需要替换成对应的内容

```

> 注：BCF节点程序默认初始节点密码为：tgjR6sV4j。安装好BCF节点程序后，强烈建议您先修改节点密码。

## 锻造区块

> 注:在执行本章节命令前，请先确保
>    1. 您的BCF节点软件已经启动
>    2. 当前命令窗口已在BFChainMeta主目录下,若不是，则可执行如下命令完成切换
>    
>    ```
>    cd /data/bfmeta/  //执行该命令将路径切至BFChainMeta主目录下
>    ```

1. 为BCF节点绑定身份(地址)

详情请参考[\<节点身份绑定\>](/zh/教程/PC全节点教程/Linux/authorize.md#节点身份绑定)

> 注：请确保绑定的地址有足够的主链数字权益以支付事件上链费。

2. 申请成为受托人

您需要发起一笔“申请成为受托人”事件，将您绑定的地址身份置为受托人，执行如下命令：

```
./bcf --trUsername secret="123",fee=100,alias="username" // 设置用户名
./bcf --trDelegate secret="123",fee=100 // 申请成为受托人
```
3. 开启接收投票

成为受托人后，您需要发起一笔“接收投票”事件，以保证您的地址可以被投票，从而获得选票。只有足够多的得票数和在线率，才能保证您能被选中成为下一轮的区块锻造者。

发起一笔“接收投票”事件的命令如下：

```
./bcf --trAcceptVote sercet="123",fee=100

```

4. 为节点开启打块

当您的节点已经准备好(区块同步到最高、节点状态正常、网络良好)，则可以通过执行如下命令为您的节点开启打块：

```
./bcf --setSystemConfig verifyType="001",verifyKey="SystemKey",generateBlockEnale=true

```
至此，您的节点已开启区块锻造者的竞选，一旦被选中，就意味着您的节点已成为下一轮的区块锻造者，将参与下一轮的区块锻造工作。

> 注：当您的节点将要进行升级或重启时，建议先关闭打块，待节点恢复正常后再开启打块，以免影响您被选中成为锻造者的概率。



## 链上投票治理

> 注:在执行本章节命令前，请先确保
>    1. 您的BCF节点软件已经启动
>    2. 当前命令窗口已在BFChainMeta主目录下,若不是，则可执行如下命令完成切换
>    
>    ```
>    cd /data/bfmeta/  //执行该命令将路径切至BFChainMeta主目录下
>    ```

1. 为BCF节点绑定身份(地址)

详情请参考[\<节点身份绑定\>](/zh/教程/PC全节点教程/Linux/authorize.md#节点身份绑定)

> 注：请确保绑定的地址有足够的主链数字权益以支付事件上链费。

2. 开启自动/手动投票

- 自动投票

通过命令行窗口执行如下命令，可为节点开启自动投票：

```
./bcf --setSystemConfig verifyType="001",verifyKey="SystemKey",autoVoteEnable=true

```

- 手动投票
用户若需要为某个已知地址的受托人投票，则可以通过命令行发起一笔“投票”事件

```
./bcf --trVote secret="123",fee=500,equity=0,recipientId=address

```




## 更新节点程序

当前，用户只能通过BFChainMeta节点管理器进行节点更新。




## 重启节点程序

当前，用户只能通过BFChainMeta节点管理器进行节点重启。

> 注：节点重启后，节点原绑定的身份(地址)会解绑，用户需要重新再绑定一次。详情请参考[\<节点身份绑定\>](/zh/教程/PC全节点教程/Linux/authorize.md#节点身份绑定)
