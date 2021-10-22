# Websocket

## Implement a websocket calling tool

The following code uses typescript as an example to implement a universal websocket calling interface tool class.

// The package used by the websocket client can be installed with npm install socket.io-client
```ts

import * as io from "socket.io-client";

/**

* websocket connection helper

*/

class WebsocketConnectHelp {

    /** Websocket connection object */

    socket: SocketIOClient.Socket;

    constructor(url: string/**connected url */) {

        this.socket = io.connect(`http://${url}/systemChannel`, {

            transports: ["websocket"],//allowed connection method, here is websocket

            reconnection: true,//Whether to disconnect and reconnect

        });

        // Triggered when the websocket connection is successful

        this.socket.on("connect", () => {

            console.log(`connected to ${url} `);

        });

        // Triggered when websocket connection fails

        this.socket.on("connect_error", (data: any) => {

            console.error(`${url} connect_error with data=${data}`);

        });

        // Triggered when data transmission fails in websocket

        this.socket.on("error", (data: any) => {

            console.error(`${url} error with data=${data}`);

        });

    }

    /**
    
    * Universal sending websocket request function
    
    * @param cmd interface name
    
    * @param data interface input parameters
    
    */

    async sendWebsocketRequest(cmd: string, data: any) {

        return new Promise((resolve, reject) => {

            try {

                //Trigger the event of `post/api/system/${cmd}`

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

## Use tool class to call interface

After having the above code, you can call the interface according to the actual situation of the websocket link, where url is the ip and port number of the node that needs to be called, cmd represents the name of the interface that needs to be called, and data represents the incoming data of the interface that needs to be called. , According to websocketConnectHelp.sendWebsocketRequest(cmd,
data) function to call different interfaces.

For example, the following is an example when the safety shutdown is called. From the \<[safe shutdown node](#_safe-shutdown-node)\> in the interface description of this chapter, the full name of the interface is safetyClose, and the parameter that needs to be passed in is verifyType. verifyKey, isShutdown, we can use the following code to call.
```ts

(async () => {

    const url = "192.168.0.1:19003";

    const websocketConnectHelp = new
        WebsocketConnectHelp(url);//Instantiate a websocket connection help class

    const cmd = "safetyClose";//interface name

    //Incoming parameters

    const data = {

        verifyType: "001",

        verifyKey:

            "6f43a87af3a52c35150cabb80ce868318e89668aaea755295b15282df7bd89f3",

        isShutdown: true

    }

    const result = await websocketConnectHelp.sendWebsocketRequest(cmd,
        data);//Call interface

    console.log(result);//Get return parameters

})()
```
