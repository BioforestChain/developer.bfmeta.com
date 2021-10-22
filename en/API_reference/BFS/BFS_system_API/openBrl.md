# openBrl

## 1. Description

​ Used to open an application.

## 2. Method

- **openBrl(brl: string): PromiseLike<BFS.Channel.Port | undefined>;**

   - **Parameter Description**
     -brl: application open path
   - **Return Value**: Port used for duplex communication between two applications

   ```javascript
   import "@bfs/bfchain-runtime-typings";
   const bfs = bfsprocess.import("bfs");
   (async ()=>{
       // Open an application B, get a current application A and application B duplex communication port
       const duplexPort = await bfs.openBrl("test://home.org");
       if (duplexPort) {
           // Receive the message sent by application B
           duplexPort.onRec.attach((data)=>{
              console.log("Message sent by application B:", data);
           });
           // App A sends a message to App B
           duplexPort.send('Hello, I am A');
   }
   })()
   ```

## 3. Related type declaration
```javascript
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
}
```