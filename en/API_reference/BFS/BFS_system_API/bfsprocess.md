# bfsprocess module

## 1. Description
​ An instance of a process, through which modules can be imported and used in applications, such as "bfs", "channel" and other modules.

## 2. Method

- **`import<T extends ImportNames>(name: T):Module<T>`**
  - **Parameter Description**
    -name: module name

  - **Return value** type
    - `Module<T>`

## 3. Properties
- **onGatewayBrl:** Gateway event trigger, all openBrl requests will be sent to the event, and the event will be captured by attach

  ```javascript
  bfsprocess.onGatewayBrl.attach(info=>{
      const {brl, offer} = info;
      // Determine whether to raise a placard to process the request based on the information in info.brl
      if (brl.protocal ==='upgrade') {
          offer(1); // Indicates that this request is handled by raising a card
      }
      
  })
  ```

  

- **onLinkedBrl:** Application start event trigger

  ```javascript
  bfsprocess.onLinkedBrl.attach(linked=>{
      const {url, duplex} = linked;
      // url contains the requested metadata
      // duplex is the internal duplex communication port of the application, which means sending and receiving messages
      // receive message
      duplex.onRec.detach((data)=>{
          console.log('data', data);
    })
      // send messages
      duplex.send('Hello');
  })
  ```
  
  

## 4. Related type declaration

```javascript
type ImportNames = "bfs" | "evt" | "openBrl" | "link" | "webrtc" | "channel"

const enum EXIT_SIGN {
  // complete the task normally
  DONE = 0,
  // Did not complete the task
  FAIL = 1,
  // crash
  CRASH = 2,
}

type ModuleFactory<N> = (
    protocal: ProtocalEndpoint,
    bfsprocess: InsideProcess
) => Module[N]

interface Module {
    openBrl: (brl: string) => PromiseLike<BFS.Channel.Port | undefined>;
    bfs: {
      openBrl: (brl: string) => PromiseLike<BFS.Channel.Port | undefined>;
      importService: ExecRuntime.ImportService;
    };
}
namespace BFS {
    namespace Channel {
        interface Port<I = unknown, O = unknown> {
          send(msg: O, transfer?: any): void;
          close(): void;
          onRec: BFS.Evt.AttachOnlyEvtBase<I>;
          onRecError: BFS.Evt.AttachOnlyEvtBase<unknown>;
          onClosed: BFS.Evt.StatefulEvt.AttachOnlyEvtBase<boolean>;
        }
    }
    namespace Exec.ProcessProtocal {
        type BrlInfo = {
            fromAppUid: Meta.Application.Uid; // source application id
            fromPid: PROCESS_ID; // source process id
            href: string; // link
            origin: string; // original path
            host: string; // domain
            hostname: string; // domain name
            pathname: string; // path
            hash: string; // hash
            search: string; // query parameters
            protocol: string; // protocol (mime)
            port: string; // port
            password: string; // password
            username: string; // username
      };
      type GatewayBrlData = {
        brl: BrlInfo;
        port: BFS.Channel.Port;
      };
      type LinkedBrlData = {
        linkId: number;
        brl: BrlInfo;
        port: BFS.Channel.Port;
      };
    }
}
```

## 5. Example

```javascript
// Example 1:
import "@bfs/bfchain-runtime-typings";
bfsprocess.onGatewayBrl.attach(info=>{
    const {brl, offer} = info;
    info.offer(1); // Indicates that this request is handled by raising a card
})
bfsprocess.onLinkedBrl.attach(linked=>{
    const {url, duplex} = linked;
    // url contains the requested metadata
    // Duplex is the internal duplex communication port of the application, which means to send and receive messages. For usage, please refer to BFS.Channel.Port
    duplex.onRec.attach(data=>{
        console.log('data:', data);
    })
})
```

```javascript
// Example 2: Import bfs module usage example
import "@bfs/bfchain-runtime-typings";
const bfs = bfsprocess.import('bfs');
(async ()=>{
    const duplexPort = await bfs.openBrl("test://home.org");
    duplexPort.send("hello, testHomeApp");
})()
```