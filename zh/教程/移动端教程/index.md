# **BFS开发者基础教程(Beta)**
## **1. BFS 概述**
- ### BFS是什么？

    BFS，全称为Bioforest Chain System，Bioforest为Biology Forest的全称，BFS意为生物链林系统。它是本能实验室自主研发，国产自主可控的区块链底层技术，打破了传统区块链技术必须运行在x86 PC上的约束，将区块链的应用范围扩大到了手机，平板电脑、智能穿戴设备以及物联网设备平台上。系统在设计上像有生命的生物群，生成的链更像是生物链，再加上其内在的跨链特性，可以让所有符合共识机制的链接入并成为一个整体，直观体验就像是组成一片有生命并且可以自我生长的森林。
    
    > `注意：`BFS目前处于持续迭代阶段，可能会出现架构调整现象，如在使用中遇到问题，您可以在[GitHub](https://github.com/BioforestChain/bfcc.dev/issues/new)中进行反馈。

- ### BFS目标

    BFS是可以提供服务接口的底层系统，开发者可以基于BFS开放的能力创建DAPP。

    - 基础服务：提供区块链底层基础设施
    - 跨链服务：自由生长的跨链技术
    - 模块化设计：区块链底层开发框架，提供多组件服务
    - 扩展性：提供可视化生态扩展（TUI、IDE、社区等）   
    
- ### BFS架构
    ![](./media/bfs-jiagou.png)


- ### 名词解释

    - **BFS应用**
      
        BFS应用是基于BFS平台提供的能力，构建的区块链应用。

    - **TUI是什么？**
    
        TUI(Text User Interface)是一个BFS内置的命令行界面，用于与BFS系统进行交互，可以类比windows上的cmd终端，linux上的shell。可以通过该命令行界面调用BFS系统功能，也可通过BFS暴露的接口进行自定义指令，并按照指定的协议控制TUI状态，从而实现同BFS交互（详见BFS-TUI通信协议说明）。

    - **自定义指令**
      
        1. 自定义指令是可以通过TUI命令行界面输入执行的命令，开发者可以按照指令规则创建自定义指令。
        2. 自定义指令本质上是一个BFS应用。
        3. 自定义指令同其他BFS应用的区别：自定义指令可以同TUI命令行界面交互，并符合TUI通信协议的应用。
    
## **2. 快速开始**

- ### 安装 BFS SDK 
    在本地系统终端（如Windows下的cmd,powershell, Mac下的terminal, linux下的bash,如无特殊说明下文中的"本地系统终端"均为此义）运行如下命令来安装bfs-sdk
    ```
    > npm install bfs-sdk -g
    ```
- ### 安装 bfsp (BFS辅助工具)
    ```
    > npm install bfsp -g
    ```
- ### 运行 BFS SDK
    在本地系统终端运行如下命令来启动bfs
    ```
    > bfs
    ```
    
- ### 创建工程
    在本地系统终端使用bfsp创建工程
    ```
    > bfsp create projectName
    ```
- ### 安装依赖
    在本地系统终端进入到新创建的工程，并安装依赖
    ```
    > cd projectName
    > bfsp init
    ```

- ### 开发应用
    详见4.示例
    
- ### 构建应用
    本地系统终端内，在当前应用目录下执行
    ```
    > bfsp bundle
    ```
    生成projectName.napp存在本地
- ### 安装应用
    启动BFS，通过BFS命令行即TUI,启动安装程序界面。
    以下命令在TUI中执行
    ```
    > core install
    ```
    在安装程序界面，选择本地的projectName.napp文件，然后点击安装按钮，完成安装操作。

- ### 发布应用
    应用市场即将上线...

## **3. 工程文件目录结构说明**


- ### 工程文件目录结构

```
├─project               [工程根目录]
│ ├─bundle              [工程配套的-静态注册信息模块目录]
│ │ ├─src               [工程配套的-静态注册信息源文件目录]
│ │ │ └─index.ts        [工程配套的-静态注册信息文件]
│ │ ├─typings           [工程配套的-静态注册信息类型申明目录]
│ │ │ ├─src             [工程配套的-静态注册信息类型申明源目录]
│ │ │ │ ├─@types.ts     [工程配套的-静态注册信息类型声明文件]
│ │ │ │ └─index.ts      [工程配套的-静态注册信息类型声明入口文件]
│ │ | └─bfsp.json       [工程配套的-静态注册信息类型声明工程配置文件]
│ │ └─bfsp.json         [工程配套的-静态注册信息模块的工程信息说明文件]
│ ├─src                 [工程的源文件目录]
│ | ├─index.ts          [/]
│ | └─main.ts           [工程的入口文件]
│ ├─typings             [工程配套的-工程的类型声明目录]
│ | ├─src               [工程配套的-工程的类型声明源目录]
│ | | ├─@types.ts       [工程配套的-工程类型声明文件]
│ | | └─index.ts        [工程配套的-工程类型申明的入口文件]
│ | └─bfsp.json         [工程配套的-工程类型申明的配置文件]
│ └─bfsp.json           [工程的配置文件]
```



## **4. 示例**

- ### ① 创建BFS应用
    - project/bundle/src/index.ts 文件
        ```javascript
        // 导入核心模块
        import * as bfs from "@bfs/core";
        // 引入 自定义类型文件
        import "packagename-typings";

        // 导入构建应用静态数据的方法
        export function packagenameAppBuilder() {
            const app = bfs.appBundle(uid<string>, appName<string>, version<number>, {
                // 当前应用支持的协议类型名称
                // 可以是多个
                supportMimes: ["mime"],
            });

            const runner: BFS.Exec.Runner = new bfs.Resource(
                {
                    // 导入程序测执行体
                    // 'path/filename.js' 是 project/src/main.ts文件编译出来的结果
                    main: `importScripts('path/filename.js')` ,
                },

                // 资源名称,
                "bfchainv3-resc",
                
                // 资源类型
                "bfs/exec"
            );

            return { app, runner };
        }

        ```

    - project/bundle/typings/src/@types.ts 文件
        ```javascript
        // 声明类型
        declare namespace TypeName{
    
        }
    
        ```
        
    - project/bundle/typings/src/index.ts 文件
        ```javascript
        // 导入其他模块的typings
        import "other/typings";
        import "./@types";
        ```
    
    - project/bundle/typings/bfsp.json 文件
    
        ```javascript
        {
            "name": "packagename-bundle-typings",
            "version": "0.0.0",
            "vars": {},
            "source": {
                "mainFilename": "index.ts",
                "dirName": "src"
            },
            "projects": [],
            "dependencies": [],
            "plugins": {
            	"bdkTsc": {
                	"target": ["cjs", "esm", "esm-es5"]
             	}
            }
        }
        ```
    
        
    
    - project/bundle/bfsp.json 文件
    
      ```javascript
      {
              "name": "packagename-bundle",
              "version": "0.0.0",
              "vars": {},
              "source": {
                  "mainFilename": "index.ts",
                  "dirName": "src"
              },
              "projects": ["./typings"],
              "dependencies": [],
              "plugins": {
                  "bdkTsc": {
                  "target": ["cjs", "esm", "esm-es5"]
                  }
              }
          }s
      ```
    
    - project/src/index.ts 文件
        ```
        // 暂时没使用
        ```
    
    - project/src/main.ts 文件
    
        ```javascript
        // 导入运行时
        import "@bfs/bfchain-runtime-typings";
            
        
        // 网关 条件匹配后进程会询问是否举牌
        // 如果没有举牌就不会执行 bfsprocess.onLinkedBrl.attach() 部分的代码；
        bfsprocess.onGatwayBrl.attach(info => {
            //可以通过 info 来判断是否举牌
            info.offer(1)
        })
        
        // 举牌后 如果进程选中了执行当前应用就会执行下面的回调
        bfsprocess.onLinkedBrl.attach((linked) =>{
            // 可以通过 linked 判断是否执行程序
            const { createActivity } = bfsprocess.import("@bfs/lib-dom-activity");
            const { registerActivity } = bfsprocess.import("@bfs/lib-dom-activity");
            const afterInited = (tuiActivity: BFS.Lib.DomActivity) => {
        
                // 程序的执行体……
                // 载入其他的 WEB 应用
                const DOMIframe = tuiActivity.$d.createElement("iframe");
                DOMIframe.style.width = "100%";
                DOMIframe.style.height = "100%";
                DOMIframe.src = new URL("./index.html", location.origin).href;
                tuiActivity.$appendDomToBody(DOMIframe);
            }
            const mainAc = createActivity("appname", registerActivity({afterInited}));
            mainAc.open();
        })
        
        ```
    
    - project/typings/src/@types.ts 文件
    
      ```javascript
      // 声明类型
      declare namespace TypeName{
      
      }
      ```
    
      
    
    - project/typings/src/index.ts 文件
    
      ```javascript
      // 导入其他模块的typings
      import "other/typings";
      import "./@types";
      ```
    
      
    
    - project/typings/bfsp.json 文件
    
      ```javascript
      {
          "name": "packagename-typings",
          "version": "0.0.0",
          "vars": {},
          "source": {
              "dirName": "src",
              "mainFilename": "index.ts"
          },
          "projects": [],
          "dependencies": [],
          "plugins": {
              "bdkTsc": {
              	"target": ["cjs", "esm", "esm-es5"]
              }
          }
      }
      ```
    
      
    
    - project/bfsp.json 文件
    
      ```javascript
      {
          "name": "packagename",
          "version": "0.0.0",
          "vars": {},
          "source": {
              "dirName": "src",
              "mainFilename": "index.ts"
           },
           "projects": ["bundle", "./typings"],
           "dependencies": ["@bfs/bfchain-runtime-typings"],
           "plugins": {
               "bdkTsc": {
                   "target": ["cjs", "esm", "esm-es5"]
               },
               "rollup": {
                   "sourceInputFile": "main.ts",
                   "rollupProfileOptions": {
                       "jsRuntime": "web"
                   },
                   "outputs": [
                       {
                           "externalLiveBindings": false,
                           "preferConst": true,
                           "file": "path/filename.js",
                           "name": "name",
                           "format": "iife"
                       }
                   ]
               }
           }
      }
      ```
    
      
    
- ### ② 自定义指令

    - project/bundle/src/index.ts 文件
        ```javascript
        import * as bfs from "@bfs/core";
        import "packagename-typings";

        export function commandsAppBuilder() {
            const app = bfs.appBundle("cli.tui.core.commands.org", "Core", 1, {
                supportMimes: ["core"],
                shared: {
                    // 静态注册comamndMime 和 command 元数据
                    // shared.commands 是标准表示内部的数据对象是指令类型的数据
                    commands:{
                        // core 理解为指令类型 
                        // ["help", "install", "open"] === command 下面全部包函的指令集
                        // 用法:
                        // > core help
                        // > core install
                        // > core open test://home.org           
                        core:  ["help", "install", "open"]
                    }
                }
            });
            const runner: BFS.Exec.Runner = new bfs.Resource(
                {
                    main: `importScripts('../../bfchain/commands.js')`,
                },
                "bfchainv3-resc",
        "bfs/exec"
            );
        
            return { app, runner };
        }
        ```
        
    - project/bundle/typings/src/@types.ts 文件
        ```javascript
        // 声明类型
        declare namespace TypeName{

        }
        ```
        
    - project/bundle/typings/src/index.ts 文件
    
        ```javascript
        // 导入其他模块的typings
        import "other/typings";
        import "./@types";
        ```
    
        
    
    - project/bundle/typings/bfsp.json 文件
        ```javascript
        {
            "name": "packagename-typings",
            "version": "0.0.0",
            "vars": {},
            "source": {
                "dirName": "src",
                "mainFilename": "index.ts"
            },
            "projects": [],
            "dependencies": [],
            "plugins": {
                "bdkTsc": {
                "target": ["cjs", "esm", "esm-es5"]
                }
            }
        }       
        ```
    
    - project/bundle/bfsp.json 文件
        ```javascript
        {
            "name": "packagename",
            "version": "0.0.0",
            "vars": {},
            "source": {
                "dirName": "src",
                "mainFilename": "index.ts"
            },
            "projects": ["bundle", "./typings"],
            "dependencies": ["@bfs/bfchain-runtime-typings"],
            "plugins": {
                "bdkTsc": {
                    "target": ["cjs", "esm", "esm-es5"]
                },
                "rollup": {
                    "sourceInputFile": "main.ts",
                    "rollupProfileOptions": {
                        "jsRuntime": "web"
                    },
                    "outputs": [
                        {
                            "externalLiveBindings": false,
                            "preferConst": true,
                            "file": "path/filename.js",
                            "name": "name",
                            "format": "iife"
                        }
                    ]
                }
        	}
        }
        ```
    
    - project/src/index.ts 文件
        ```javascript
        // 暂时可为空
        ```
    
    - project/src/main.ts 文件
        ```javascript
        import "@bfs/bfchain-runtime-typings";
        const bfs = bfsprocess.import('bfs');
        const register = bfs.getRegister("cli.tui.org")
        const BTPTools = bfs.BTPTools;
        register('core', 'help', (commandID: number, send: {(responseMessage: string):void}, params?: string) => {
            // 执行指令体 
            // 通过 send 向 Tui 发送消息 
            send(BTPTools.createResMessageNormal("The first message", commandID, 0))
            // 通知Tui停止接收消息，当前指令执行完毕
            send(BTPTools.createEntryInputStateNormalProtocol(commandID)) /** 会同时关闭 tui 接受消息 */
        })
        ```
    
    - project/typings/src/@types.ts 文件
        ```javascript
        // 声明类型
        declare namespace TypeName{
        
        }
        ```
    
    - project/typings/src/index.ts 文件
        ```javascript
        // 导入其他模块的typings
        import "other/typings";
        import "./@types";
        ```
    
    - project/typings/bfsp.json 文件
       ```javascript
       {
            "name": "packagename-typings",
            "version": "0.0.0",
            "vars": {},
            "source": {
                "dirName": "src",
                "mainFilename": "index.ts"
            },
            "projects": [],
            "dependencies": [],
            "plugins": {
                "bdkTsc": {
                	"target": ["cjs", "esm", "esm-es5"]
                }
            }
       }
       ```
    
    - project/bfsp.json 文件
    
      ```javascript
      {
              "name": "packagename",
              "version": "0.0.0",
              "vars": {},
              "source": {
                  "dirName": "src",
                  "mainFilename": "index.ts"
              },
              "projects": ["bundle", "./typings"],
              "dependencies": ["@bfs/bfchain-runtime-typings"],
              "plugins": {
                  "bdkTsc": {
                      "target": ["cjs", "esm", "esm-es5"]
                  },
                  "rollup": {
                      "sourceInputFile": "main.ts",
                      "rollupProfileOptions": {
                          "jsRuntime": "web"
                      },
                      "outputs": [
                          {
                              "externalLiveBindings": false,
                              "preferConst": true,
                              "file": "path/filename.js",
                              "name": "name",
                              "format": "iife"
                          }
                      ]
                  }
              }
          }
      ```

