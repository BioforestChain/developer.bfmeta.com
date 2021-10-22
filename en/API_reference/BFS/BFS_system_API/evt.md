# evt module

## 1. Description

​ Based on Ctx event triggers, it encapsulates event triggers that meet a variety of needs.

## 2. Class

### Ctx: Event Trigger

  - **Method**

    - post(data: T): void;

      - Description: trigger event

    - `attach<L extends BFS.Evt.Listener<T>(listener: L): L`

      -Description: Bind an event

      ```js
      const ctx = new Ctx<string>();
      ctx.attach((name:string)=>{
          console.log('name', name);
      })
      ctx.post('john');
      ```

    - `dettach(listener: Listener<T>): void;`

      - Description: Remove an event

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

      - Description: Remove all events

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

### AttachOnlyEvt: Event Binder

  - **Method**
    - `attach(listener: Listener<T>): Listener<T>`
      -Description: binding event
    - `attachOnce(listener: Listener<T>): Listener<T>;`
      -Description: The bound event is triggered only once
    - `detach(listener: Listener<T>): void`
      -Description: Unbinding event
    - `attachCtx(ctx: Ctx<T>): void`
      -Description: Bind an event trigger
    - `detachCtx(ctx: Ctx<T>): void`
      -Description: Remove an event trigger
    - detachAll(): void
      -Description: Remove all event triggers
    - `bindPoster(poster: PostableEvt<T>)`
      -Description: Bind an event sender
    - `unbindPoster(poster: PostableEvt<T>): void`
      -Description: Remove an event sender

### PostableEvt: Event sender

  - **Method**
    - `bindCtx(ctx: Ctx<T>): void`
      -Description: Bind an event trigger
    - `unbindCtx(ctx: Ctx<T>): boolean`
      -Description: Unbind an event trigger
    - pause(): void
      -Description: Clear the message sending queue
    - resume(): void
      -Description: resend the message
    - post(data: T): void
      -Description: Send message

### Evt: encapsulates the AttachOnlyEvt and PostableEvt classes, and has the functions of the two classes at the same time

  - **Method**

    - `Evt<T>.post: (data: T) => void;`

      -Description: Send a message

    - `Evt<T>.attach: (listener: BFS.Evt.Listener<T, unknown>) => BFS.Evt.Listener<T, unknown>`

      - Description: Bind an event

      ```javascript
      const {Evt} = bfsprocess.import('evt');
      const evt = new Evt<string>();
      evt.attach((name)=>{
          console.log('Name:', name received)
      })
      evt.post('Tom');
      evt.post('Cat');
      // operation result:
      // Received name:Tom
      // Received name:Cat
      ```

    - `Evt<T>.attachOnce: (listener: BFS.Evt.Listener<T, unknown>) => BFS.Evt.Listener<T, unknown>`

      - Description: Bind an event that is executed only once

      ```javascript
      const {Evt} = bfsprocess.import('evt');
      const evt = new Evt<string>();
      evt.attachOnce((name)=>{
          console.log('Name:', name received)
      })
      evt.post('Tom');
      evt.post('Cat');
      // operation result:
      // Received name:Tom
      ```

      

    - `Evt<T>.attachCtx: (ctx: Ctx<T>) => void`

      - Description: Bind an event trigger

      ```javascript
      const {Evt, Ctx} = bfsprocess.import('evt');
      const ctx = new Ctx<string>();
      ctx.attach((data)=>{
          console.log(data);
      });
      evt.attachCtx(ctx);
      evt.post('good job');
      // operation result:
      // good job
      ```

      

    - `Evt<T>.detach: (listener: BFS.Evt.Listener<T, unknown>) => void`

      - Description: Remove an event

      ```javascript
      const {Evt} = bfsprocess.import('evt');
      const evt = new Evt<string>();
      const listner = (name)=>{
          console.log('Name:', name received)
      }
      evt.attach(listner)
      evt.post('Tom');
      evt.detach(listner);
      evt.post('Cat');
      // operation result:
      // Received name:Tom
      ```

      

    - `Evt<T>.detachCtx: (ctx: Ctx<T>) => void`

      - Description: Remove an event trigger

      ```javascript
      const {Evt, Ctx} = bfsprocess.import('evt');
      const evt = new Evt<string>();
      const ctx = new Ctx<string>();
      ctx.attach((data)=>{
          console.log(data);
      });
      evt.attachCtx(ctx);
      evt.post('good job');
      evt.detachCtx(ctx);
      evt.post('bad job');
      // operation result:
      // good job
      ```

      

    - `Evt<T>.detachAll: () => void;`

      - Description: Remove all events

      ```javascript
      const {Evt} = bfsprocess.import('evt');
      const evt = new Evt<string>();
      const listner1 = (name)=>{
          console.log('Name1:', name received)
      }
      const listner2 = (name)=>{
          console.log('Name2:', name received)
      }
      const listner3 = (name)=>{
          console.log('Name3:', name received)
      }
      evt.attach(listner1);
      evt.attach(listner2);
      evt.attach(listner3);
      evt.post('Tom');
      evt.detach(listner2);
      evt.post('Cat');
      evt.detachAll();
      evt.post('Tony');
      
      // operation result:
      // Received name1: Tom
      // Received name2: Tom
      // Received name3: Tom
      // Received name1: Cat
      // Received name3: Cat
      ```

      

    - `Evt<T>.pause: () => void;`

      -Description: Clear the messages buffered in the sending queue

    - `Evt<T>.resume: () => void;`

      - Description: Resend the message buffered in the queue, because every time a message is sent, the message will be buffered.

      ```javascript
      const {Evt} = bfsprocess.import('evt');
      evt.attachCtx((data)=>{
          console.log(data);
      });
      evt.pause();
      evt.post('good job');
      evt.post('badjob');
      evt.resume(); // resend the message in the buffer
      evt.pause(); // Clear the cached messages in the queue
      evt.resume();// Resend the message in the buffer, in fact, the message in the queue here is already empty, and no message will be sent
      
      // operation result:
      // good job
      // bad job
      // good job
      // bad job
      ```

      

### waitFor: wait for an event to complete

  - `waitFor<T>(from: BFS.Evt.AttachOnlyEvtBase<T>, listener: BFS.Evt.Listener<T, boolean>): Promise<T>`

    - Description: Used to wait for the event to receive a specific message or reach a certain state

    ```javascript
    const evt = new Evt<string>();
    (async()=>{
        try {
            setTimeout(()=>{
                evt.post('finish')
            }, 2000)
            await waitFor(evt.attacher, (data) => data ==='finish');
        } finally {
            console.log('has finished')
        }
    })()
    
    // operation result:
    // Print after two seconds: has finishd
    ```

## 3. Related type declaration

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