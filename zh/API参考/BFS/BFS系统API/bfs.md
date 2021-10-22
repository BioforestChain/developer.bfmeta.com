# bfs模块

## 1. 描述

​封装了BFS系统暴露的方法。

## 2. 方法

- **`openBrl(brl: string): PromiseLike<BFS.Channel.Port | undefined>`**

  - **参数说明**
    - brl:  应用的打开路径

  - **返回值**： 用于两个应用间双工通信的端口
  ```javascript
  import "@bfs/bfchain-runtime-typings";
  const bfs = bfsprocess.import("bfs");
  (async ()=>{
      // 打开一个应用，得到一个用于两个应用间双工通信的端口
      const duplexPort = await bfs.openBrl("test://home.org");
  })()
  ```

- **`importService(mime: string, serviceName: string)`**

  - **描述：导入bfs系统服务**

  - **参数说明：**

    - mime: 服务类型
    - serviceName: 服务名称

    ```javascript
    const testLoader = bfs.importService("borker", "testService");
    (async ()=>{
        const test = await testLoader;
        const block = test.getBlock();
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