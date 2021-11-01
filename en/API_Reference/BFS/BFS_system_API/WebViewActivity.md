# @bfs/lib-webview-activity

## 1. Description

​ Used to build

## 2. Method
- **createAcivity(activityName: string, ActivityCtor: AC = (DomActivity as unknown) as AC)** is used to create a page instance
  - **Parameter Description**
    - activityName: page name
    - ActivityCtor: page constructor
  - **Return Value**: Port for duplex communication

## 3. WebViewAcivity class

- **Method**
  - **open()** Open the page
  - **close()** close the page
  - **postMessage(data: unkown)** Send a message to the inside of the page
  - **evalJavascript(code: string)** inject JS script into the page
- Attributes
  - **background** Get or set the background color of the page, the default is "white"
  - **titleText** Get or set the page title, which is passed in by the first parameter of the createActivity method
  - **titleHeight** Get title height
  - **titleVisible** Get or set the title to be visible, the default is true
  - **closeAble** Set whether the page can be closed, if it is true, the back button will be displayed on the left side of the title bar; if it is false, the back button will not be displayed, the default is true
  - **src** URL address of the page
  - **width** The width of the page
  - **height** The height of the page (window.devicePixelRatio * heightVal)px
  - **onMessage** `Evt<unknown>` event trigger

## 4. Related type declaration

```javascript
interface DomActivity {
    readonly $d: Document;
    open(): void;
    close(): void;
    waitForClose(): Promise<void>;
    $appendDomToBody(ele: HTMLElement): void;
    closeAble: boolean;
    background: string;
    titleText: string;
    titleHeight: number;
    titleVisible: boolean;
}
namespace DomActivity {
    interface Ctor<A extends DomActivity = DomActivity> {
    $d: Document;
    new (name: string): A;
    prototype: A;
    }
    interface ActivityLifeCycle<A extends DomActivity = DomActivity> {
      onInit(activity: A): void;
      afterInited(activity: A): void;
    }
}
declare namespace BFS.Lib {
  interface WebviewActivity extends DomActivity {
    readonly devicePixelRatio: number;
    src: string;
    width: number;
    height: number;
    postMessage(data: unknown): void;
    onMessage: Evt<unknown>;
    evalJavascript(code: string): void;
    onLoaded: Evt<Window>;
  }
  type WebviewActivityCtor = DomActivity.Ctor<WebviewActivity>;
}

```

## 5. Example

```javascript
import "@bfs/bfchain-runtime-typings";
const {createActivity} = bfsprocess.import('@bfs/lib-dom-activity');
const libWebview = bfsprocess.import("@bfs/lib-webview-activity");

// Create a page instance
const mainAc = createActivity("v3", libWebview.WebViewActivity);
// Set the background color of the page
mainAc.background = "red";

// Set whether the title is visible
mainAc.titleVisible = true;
// Set whether the page can be closed
mainAc.closeAble = false;
// open the page
mainAc.open();
// Set page URL
mainAc.src = "http://localhost:8100";
// Set the page title
mainAc.titleText = "This is a custom title";
// code injection
mainAc.evalJavascript(`console.log("This is the injected code")`)
// Close the page after 5s
setTimeout(()=>{
    mainAc.close()
}, 5000)
```