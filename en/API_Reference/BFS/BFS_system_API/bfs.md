# bfs module

## 1. Description

​Encapsulates the methods exposed by the BFS system.

## 2. Method

- **`openBrl(brl: string): PromiseLike<BFS.Channel.Port | undefined>`**

   - **Parameter Description**
     - brl: Open path of the application

   - **Return Value**: Port used for duplex communication between two applications
   ```javascript
   import "@bfs/bfchain-runtime-typings";
   const bfs = bfsprocess.import("bfs");
   (async ()=>{
       // Open an application and get a port for duplex communication between two applications
       const duplexPort = await bfs.openBrl("test://home.org");
   })()
   ```

- **`importService(mime: string, serviceName: string)`**

   - **Description: Import bfs system service**

   - **Parameter Description:**

     - mime: service type
     - serviceName: service name

     ```javascript
     const testLoader = bfs.importService("borker", "testService");
     (async ()=>{
         const test = await testLoader;
         const block = test.getBlock();
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