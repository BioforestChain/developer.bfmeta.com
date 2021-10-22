# openBrl

## 1. 描述

​		用来打开一个应用。

## 2. 方法

- **openBrl(brl: string): PromiseLike<BFS.Channel.Port | undefined>;**

  - **参数说明**
    - brl:  应用打开路径
  - **返回值**： 用于两个应用间双工通信的端口

  ```javascript
  import "@bfs/bfchain-runtime-typings";
  const bfs = bfsprocess.import("bfs");
  (async ()=>{
      // 打开一个应用B，得到一个当前应用A与应用B双工通信的端口
      const duplexPort = await bfs.openBrl("test://home.org");
      if (duplexPort) {
          // 接收应用B发送的消息
          duplexPort.onRec.attach((data)=>{
             console.log("应用B发送的消息：", data);
          });
          // 应用A向应用B发送消息
          duplexPort.send('Hello, I am A');
  	}
  })()
  ```

## 3. 相关类型声明

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