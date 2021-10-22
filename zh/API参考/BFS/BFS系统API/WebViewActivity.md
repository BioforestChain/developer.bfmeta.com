# @bfs/lib-webview-activity

## 1. 描述

​		用于构建

## 2. 方法
- **createAcivity(activityName: string, ActivityCtor: AC = (DomActivity as unknown) as AC)** 用于创建一个页面实例
  - **参数说明**
    - activityName: 页面名称
    - ActivityCtor: 页面构造函数
  - **返回值**： 双工通信的端口

## 3. WebViewAcivity 类

- **方法**
  - **open()**  打开页面
  - **close()**  关闭页面
  - **postMessage(data:  unkown)** 向页面内部发送消息
  - **evalJavascript(code: string)** 向页面内部注入JS脚本
- 属性
  - **background** 获取或设置页面背景色, 默认为"white"
  - **titleText** 获取或设置页面标题, 由createActivity方法第一个参数传入
  - **titleHeight** 获取标题高度
  - **titleVisible** 获取或设置标题可见，默认为true
  - **closeAble** 设置页面是否可被关闭，如果为true，标题栏左侧显示返回按钮；如果为false,则不显示返回按钮， 默认为true
  - **src** 页面的URL地址
  - **width** 页面的宽度
  - **height** 页面的高度(window.devicePixelRatio * heightVal)px
  - **onMessage** `Evt<unknown>` 事件触发器

## 4. 相关类型声明

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

## 5. 示例

```javascript
import "@bfs/bfchain-runtime-typings";
const { createActivity } = bfsprocess.import('@bfs/lib-dom-activity');
const libWebview = bfsprocess.import("@bfs/lib-webview-activity");

// 创建一个页面实例
const mainAc = createActivity("v3", libWebview.WebViewActivity);
// 设置页面背景色
mainAc.background = "red";

// 设置标题是否可见
mainAc.titleVisible = true;
// 设置页面是否可关闭
mainAc.closeAble = false;
// 打开页面
mainAc.open();
// 设置页面URL
mainAc.src = "http://localhost:8100";
// 设置页面标题
mainAc.titleText = "这是自定义标题";
// 代码注入
mainAc.evalJavascript(`console.log("这是注入的代码")`)
// 5s后关闭页面
setTimeout(()=>{
    mainAc.close()
}, 5000)
```