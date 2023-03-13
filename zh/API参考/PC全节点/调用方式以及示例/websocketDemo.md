# Websocket

## 实现一个websocket调用工具

下述代码以typescript为例，实现一个通用的websocket调用接口工具类。

// websocket客户端使用的包,可用npm install socket.io-client安装
```ts

import * as io from "socket.io-client";

/**

* websocket连接帮助类

*/

class WebsocketConnectHelp {

    /** websocket的连接对象 */

    socket: SocketIOClient.Socket;

    constructor(url: string/**连接的url */) {

        this.socket = io.connect(`http://${url}/systemChannel`, {

            transports: ["websocket"],//允许的连接方式，这里为websocket

            reconnection: true,//是否断开重连

        });

        // 当websocket连接成功时触发

        this.socket.on("connect", () => {

            console.log(`connected to ${url} `);

        });

        // 当websocket连接失败时触发

        this.socket.on("connect_error", (data: any) => {

            console.error(`${url} connect_error with data=${data}`);

        });

        // 当websocket时数据发送失败时触发

        this.socket.on("error", (data: any) => {

            console.error(`${url} error with data=${data}`);

        });

    }

    /**
    
    * 通用的发送websocket请求函数
    
    * @param cmd 接口名称
    
    * @param data 接口输入参数
    
    */

    async sendWebsocketRequest(cmd: string, data: any) {

        return new Promise((resolve, reject) => {

            try {

                //触发`post/api/system/${cmd}`这个事件

                this.socket.emit(`post/api/system/${cmd}`, data, (result: any) => {

                    resolve(result);

                });

            } catch (e) {

                reject(e);

            }

        });

    }
}
```

## 使用工具类调用接口

在拥有上述代码后，可以根据websocket链接的实际情况，进行接口的调用，其中url是需要调用的节点的ip以及端口号，cmd表示需要调用的接口名称，data表示需要调用的接口传入的数据，可以根据websocketConnectHelp.sendWebsocketRequest(cmd,
data)函数，调用不同的接口。

例如，以下是调用安全关闭时的示例，由本章的接口描述中的\<[安全关闭节点](#_安全关闭节点)\>得知，接口全称为safetyClose，需要传入的参数为verifyType，verifyKey，isShutdown，我们可以使用下述代码进行调用。
```ts

(async () => {

    const url = "192.168.0.1:19003";

    const websocketConnectHelp = new
        WebsocketConnectHelp(url);//实例化一个websocket连接帮助类

    const cmd = "safetyClose";//接口名称

    //传入参数

    const data = {

        verifyType: "001",

        verifyKey:

            "6f43a87af3a52c35150cabb80ce868318e89668aaea755295b15282df7bd89f3",

        isShutdown: true

    }

    const result = await websocketConnectHelp.sendWebsocketRequest(cmd,
        data);//调用接口

    console.log(result);//获取返回参数

})()
```
