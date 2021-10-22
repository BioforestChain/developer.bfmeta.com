# bfsprocess模块

## 1. 描述
​		进程的实例,通过该实例可以导入模块,在应用中使用，如"bfs", "channel"等模块。

## 2. 方法

- **`import<T extends ImportNames>(name: T):Module<T>`**
  - **参数说明**
    - name: 模块名称

  - **返回值**类型
    - `Module<T>`

## 3. 属性
- **onGatewayBrl：** 网关事件触发器，所有openBrl的请求都会发送给该事件，通过attach捕获事件

  ```javascript
  bfsprocess.onGatewayBrl.attach(info=>{
      const {brl, offer} = info;
      // 根据info.brl 的信息进行判断是否要举牌处理该请求
      if (brl.protocal === 'upgrade') {
          offer(1); // 表示举牌处理这个请求
      }
      
  })
  ```

  

- **onLinkedBrl：** 应用启动事件触发器

  ```javascript
  bfsprocess.onLinkedBrl.attach(linked=>{
      const {url, duplex} = linked;
      // url包含了请求的元数据
      // duplex 为应用内部双工通信端口，表示向外发送消息，接收消息
      // 接收消息
      duplex.onRec.detach((data)=>{
          console.log('data', data);
    })
      // 发送消息
      duplex.send('Hello');
  })
  ```
  
  

## 4. 相关类型声明

```javascript
type ImportNames = "bfs" | "evt" | "openBrl" | "link" | "webrtc" | "channel"

const enum EXIT_SIGN {
  // 正常完成任务
  DONE = 0,
  // 没有完成任务
  FAIL = 1,
  // 崩溃
  CRASH = 2,
}

type ModuleFactory<N> = (
    protocal: ProtocalEndpoint,
    bfsprocess: InsideProcess
) => Module[N]

interface Module {
    openBrl: (brl: string) => PromiseLike<BFS.Channel.Port | undefined>;
    bfs:  {
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
            fromAppUid: Meta.Application.Uid; // 来源应用id
            fromPid: PROCESS_ID; // 来源进程id
            href: string; // 链接
            origin: string; // 原始路径
            host: string; // 域
            hostname: string; // 域名
            pathname: string; // 路径
            hash: string; // 哈希
            search: string; // 查询参数
            protocol: string; // 协议（mime）
            port: string; // 端口
            password: string; // 密码
            username: string; // 用户名
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

## 5. 示例

```javascript
// 示例1：
import "@bfs/bfchain-runtime-typings";
bfsprocess.onGatewayBrl.attach(info=>{
    const {brl, offer} = info;
    info.offer(1); // 表示举牌处理这个请求
})
bfsprocess.onLinkedBrl.attach(linked=>{
    const {url, duplex} = linked;
    // url包含了请求的元数据
    // duplex 为应用内部双工通信端口，表示向外发送消息，接收消息，用法参考BFS.Channel.Port
    duplex.onRec.attach(data=>{
        console.log('data:', data);
    })
})
```

```javascript
// 示例2：导入bfs模块使用实例
import "@bfs/bfchain-runtime-typings";
const bfs = bfsprocess.import('bfs');
(async ()=>{
    const duplexPort = await bfs.openBrl("test://home.org");
    duplexPort.send("hello, testHomeApp")；
})()
```
