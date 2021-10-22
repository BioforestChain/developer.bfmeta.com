# evt模块

## 1. 描述

​		基于Ctx事件触发器，封装了满足多种需求的事件触发器。

## 2. 类

### Ctx: 事件触发器

  - **方法**

    - post(data: T): void;

      - 描述：触发事件

    - `attach<L extends BFS.Evt.Listener<T>(listener: L): L`

      - 描述：绑定一个事件

      ```js
      const ctx = new Ctx<string>();
      ctx.attach((name:string)=>{
          console.log('name', name);
      })
      ctx.post('john');
      ```

    - `dettach(listener: Listener<T>): void;`

      - 描述：移除一个事件

      ```javascript
      const ctx = new Ctx<string>();
      const listener = (name:string)=>{
          console.log('name', name);
      };
      ctx.attach(listener)
      ctx.post('john');
      ctx.dettach(listener)
      ctx.post('john');
      ```

    - detachAll(): void;

      - 描述：移除所有事件

      ```javascript
      const ctx = new Ctx<string>();
      const listener = (name:string)=>{
          console.log('name', name);
      };
      ctx.attach(listener)
      ctx.post('john');
      ctx.detachAll()
      ctx.post('john');
      ```

### AttachOnlyEvt: 事件绑定器

  - **方法**
    - `attach(listener: Listener<T>): Listener<T>`
      - 描述：绑定事件
    - `attachOnce(listener: Listener<T>): Listener<T>;`
      - 描述：绑定的事件仅被触发一次
    - `detach(listener: Listener<T>): void`
      - 描述：解绑事件
    - `attachCtx(ctx: Ctx<T>): void`
      - 描述：绑定一个事件触发器
    - `detachCtx(ctx: Ctx<T>): void`
      - 描述：移除一个事件触发器
    - detachAll(): void
      - 描述：移除所有的事件触发器
    - `bindPoster(poster: PostableEvt<T>)`
      - 描述：绑定一个事件发送器
    - `unbindPoster(poster: PostableEvt<T>): void`
      - 描述：移除一个事件发送器

### PostableEvt: 事件发送器

  - **方法**
    - `bindCtx(ctx: Ctx<T>): void`
      - 描述：绑定一个事件触发器
    - `unbindCtx(ctx: Ctx<T>): boolean`
      - 描述：解绑一个事件触发器
    - pause(): void
      - 描述：清空消息发送队列
    - resume(): void
      - 描述: 重新发送消息
    - post(data: T): void
      - 描述:  发送消息

### Evt：封装了AttachOnlyEvt和PostableEvt的类，同时具有两个类的功能

  - **方法**

    - `Evt<T>.post: (data: T) => void;`

      - 描述：发送消息

    - `Evt<T>.attach: (listener: BFS.Evt.Listener<T, unknown>) => BFS.Evt.Listener<T, unknown>`

      - 描述：绑定一个事件

      ```javascript
      const { Evt } = bfsprocess.import('evt');
      const evt = new Evt<string>();
      evt.attach((name)=>{
          console.log('接收到了name:', name)
      })
      evt.post('Tom');
      evt.post('Cat');
      // 运行结果：
      // 接收到了name:Tom
      // 接收到了name:Cat
      ```

    - `Evt<T>.attachOnce: (listener: BFS.Evt.Listener<T, unknown>) => BFS.Evt.Listener<T, unknown>`

      - 描述：绑定一个只执行一次的事件

      ```javascript
      const { Evt } = bfsprocess.import('evt');
      const evt = new Evt<string>();
      evt.attachOnce((name)=>{
          console.log('接收到了name:', name)
      })
      evt.post('Tom');
      evt.post('Cat');
      // 运行结果：
      // 接收到了name:Tom
      ```

      

    - `Evt<T>.attachCtx: (ctx: Ctx<T>) => void`

      - 描述：绑定一个事件触发器

      ```javascript
      const { Evt, Ctx } = bfsprocess.import('evt');
      const ctx = new Ctx<string>();
      ctx.attach((data)=>{
          console.log(data);
      });
      evt.attachCtx(ctx);     
      evt.post('good job'); 
      // 运行结果：
      // good job
      ```

      

    - `Evt<T>.detach:  (listener: BFS.Evt.Listener<T, unknown>) => void`

      - 描述：移除一个事件

      ```javascript
      const { Evt } = bfsprocess.import('evt');
      const evt = new Evt<string>();
      const listner = (name)=>{
          console.log('接收到了name:', name)
      }
      evt.attach(listner)
      evt.post('Tom');
      evt.detach(listner);
      evt.post('Cat');
      // 运行结果：
      // 接收到了name:Tom
      ```

      

    - `Evt<T>.detachCtx: (ctx: Ctx<T>) => void`

      - 描述：移除一个事件触发器

      ```javascript
      const { Evt, Ctx } = bfsprocess.import('evt');
      const evt = new Evt<string>();
      const ctx = new Ctx<string>();
      ctx.attach((data)=>{
          console.log(data);
      });
      evt.attachCtx(ctx);     
      evt.post('good job'); 
      evt.detachCtx(ctx);   
      evt.post('bad job'); 
      // 运行结果：
      // good job
      ```

      

    - `Evt<T>.detachAll: () => void;`

      - 描述：移除所有事件

      ```javascript
      const { Evt } = bfsprocess.import('evt');
      const evt = new Evt<string>();
      const listner1 = (name)=>{
          console.log('接收到了name1:', name)
      }
      const listner2 = (name)=>{
          console.log('接收到了name2:', name)
      }
      const listner3 = (name)=>{
          console.log('接收到了name3:', name)
      }
      evt.attach(listner1);
      evt.attach(listner2);
      evt.attach(listner3);
      evt.post('Tom');
      evt.detach(listner2);
      evt.post('Cat');
      evt.detachAll();
      evt.post('Tony');
      
      // 运行结果：
      // 接收到了name1: Tom
      // 接收到了name2: Tom
      // 接收到了name3: Tom
      // 接收到了name1: Cat
      // 接收到了name3: Cat
      ```

      

    - `Evt<T>.pause: () => void;`

      - 描述：清空发送队列缓存的消息

    - `Evt<T>.resume: () => void;`

      - 描述：重新发送队列中缓存的消息，由于每发送一次消息，消息都会被缓存。

      ```javascript
      const { Evt } = bfsprocess.import('evt');
      evt.attachCtx((data)=>{
          console.log(data);
      }); 
      evt.pause();
      evt.post('good job'); 
      evt.post('badjob');
      evt.resume(); // 重新发送缓存中的消息
      evt.pause(); // 清空队列中缓存的消息
      evt.resume();// 重新发送缓存中的消息,实际上这里队列中的消息已经为空，不会发出任何消息
      
      // 运行结果：
      // good job
      // bad job
      // good job
      // bad job
      ```

      

### waitFor: 等待一个事件完成

  - `waitFor<T>(from: BFS.Evt.AttachOnlyEvtBase<T>, listener: BFS.Evt.Listener<T, boolean>): Promise<T>`

    - 描述：用于等待事件接收到某个特定的消息或者达成某种状态

    ```javascript
    const evt = new Evt<string>();
    (async()=>{
        try {
            setTimeout(()=>{
                evt.post('finish')
            }, 2000)
            await waitFor(evt.attacher, (data) => data === 'finish');
        } finally {
            console.log('has finished')
        }
    })()
    
    // 运行结果：
    // 两秒钟后打印：has finishd
    ```

## 3. 相关类型声明

```javascript
namespace BFS.Evt {
    type Listener<T, R = unknown> = (data: T) => R;
 	interface Ctx<T> {
        attach<L extends Listener<T>>(listener: L): L;
        detach(listener: Listener<T>): void;
        detachAll(): void;
        post(data: T): void;
 	}
 	interface AttachOnlyEvtBase<T> {
        attach(listener: Listener<T>): Listener<T>;
        attachOnce(listener: Listener<T>): Listener<T>;
        detach(listener: Listener<T>): void;
        attachCtx(ctx: Ctx<T>): void;
        detachCtx(ctx: Ctx<T>): void;
        detachAll(): void;
  	}
    interface AttachOnlyEvt<T> extends AttachOnlyEvtBase<T> {
        bindPoster(poster: PostableEvt<T>): void;
        unbindPoster(poster: PostableEvt<T>): void;
    }
    interface PostableEvtBase<T> {
        pause(): void;
        resume(): void;
        post(data: T): void;
	}
  	interface PostableEvt<T> extends PostableEvtBase<T> {
    	bindCtx(ctx: Ctx<T>): void;
    	unbindCtx(ctx: Ctx<T>): void;
  	}
}
```