# **BFS Developer Basic Tutorial (Beta)**
## **1. BFS Overview**
- ### What is BFS?

    BFS, the full name is Bioforest Chain System, Bioforest is the full name of Biology Forest, and BFS means the Biological Chain Forest system. It is a self-developed, domestically-made, and controllable underlying technology of the blockchain by Instinct Lab, breaking the constraint that traditional blockchain technology must run on x86 PCs, and expanding the scope of blockchain applications to mobile phones, tablets, and smart phones. Wearable devices and IoT device platforms. The system is designed like a living organism, and the generated chain is more like a biological chain. Coupled with its inherent cross-chain characteristics, all the links that conform to the consensus mechanism can be integrated and integrated into a whole, and the intuitive experience is just like composition A forest that is alive and can grow on its own.
    
    > `Note:`BFS is currently in a continuous iteration stage, and there may be structural adjustments. If you encounter problems in use, You can give feedback in [GitHub](https://github.com/BioforestChain/bfcc.dev/issues/new). 

- ### BFS target

    BFS is the underlying system that can provide service interfaces. Developers can create DAPPs based on the open capabilities of BFS.

    -Basic services: provide the underlying infrastructure of the blockchain
    -Cross-chain service: freely growing cross-chain technology
    -Modular design: the underlying development framework of the blockchain, providing multi-component services
    -Extensibility: Provide visual ecological expansion (TUI, IDE, community, etc.)
    
- ### BFS architecture
    ![](./media/bfs-jiagou.png)


- ### Glossary

    - **BFS Application**
      
        BFS applications are blockchain applications built based on the capabilities provided by the BFS platform.

    - **What is TUI? **
    
        TUI (Text User Interface) is a built-in command line interface of BFS, used to interact with the BFS system, it can be analogous to the cmd terminal on windows and the shell on linux. You can call BFS system functions through the command line interface, or you can customize instructions through the interface exposed by BFS, and control the TUI state according to the specified protocol, so as to achieve interaction with BFS (see the BFS-TUI communication protocol description for details).

    - **Custom Instructions**
      
        1. Custom instructions are commands that can be entered and executed through the TUI command line interface. Developers can create custom instructions in accordance with the instruction rules.
        2. The custom instruction is essentially a BFS application.
        3. The difference between custom instructions and other BFS applications: custom instructions can interact with the TUI command line interface and are applications that conform to the TUI communication protocol.
    
## **2. Quick start**

- ### Install BFS SDK
    In the local system terminal (such as cmd under Windows, powershell, terminal under Mac, bash under linux, if there is no special instructions, the "local system terminal" below all have this meaning) run the following command to install bfs-sdk
    ```
    > npm install bfs-sdk -g
    ```
- ### Install bfsp (BFS auxiliary tool)
    ```
    > npm install bfsp -g
    ```
- ### Run BFS SDK
    Run the following command in the local system terminal to start bfs
    ```
    > bfs
    ```
    
- ### Create project
    Use bfsp to create a project in the local system terminal
    ```
    > bfsp create projectName
    ```
- ### Installation dependencies
    Enter the newly created project in the local system terminal and install the dependencies
    ```
    > cd projectName
    > bfsp init
    ```

- ### Development application
    See 4. Example for details
    
- ### Build the app
    In the local system terminal, execute in the current application directory
    ```
    > bfsp bundle
    ```
    Generate projectName.napp to exist locally
- ### Install the app
    Start BFS, and start the installer interface through the BFS command line or TUI.
    The following commands are executed in TUI
    ```
    > core install
    ```
    In the installer interface, select the local projectName.napp file, and then click the install button to complete the installation operation.

- ### Publish application
    App market is about to go online...

## **3. Project file directory structure description**


- ### Project file directory structure 

```
├─project               [project root directory]
│ ├─bundle              [Project supporting-static registration information module directory]
│ │ ├─src               [Project supporting-static registration information source file directory]
│ │ │ └─index.ts        [Project supporting-static registration information file]
│ │ ├─typings           [Project supporting-static registration information type declaration directory]
│ │ │ ├─src             [Project supporting-static registration information type declaration source directory]
│ │ │ │ ├─@types.ts     [Project supporting-static registration information type declaration file]
│ │ │ │ └─index.ts      [Project supporting-static registration information type declaration entry file]
│ │ | └─bfsp.json       [Project matching-static registration information type declaration project configuration file]
│ │ └─bfsp.json         [Project information description file for the project supporting-static registration information module]
│ ├─src                 [project source file directory]
│ | ├─index.ts          [/]
│ | └─main.ts           [Project entry file]
│ ├─typings             [Project supporting-project type declaration directory]
│ | ├─src               [Project supporting-project type declaration source directory]
│ | | ├─@types.ts       [Project supporting-project type declaration file]
│ | | └─index.ts        [Project supporting-entry file for project type declaration]
│ | └─bfsp.json         [Project matching-project type declaration configuration file]
│ └─bfsp.json           [Project configuration file]
```



## **4. Example**

- ### ① Create BFS application
    - project/bundle/src/index.ts file
        ```javascript
        // Import the core module
        import * as bfs from "@bfs/core";
        // Import custom type files
        import "packagename-typings";

        // Import the method of constructing static data of the application
        export function packagenameAppBuilder() {
            const app = bfs.appBundle(uid<string>, appName<string>, version<number>, {
                // The name of the protocol type supported by the current application
                // can be multiple
                supportMimes: ["mime"],
            });

            const runner: BFS.Exec.Runner = new bfs.Resource(
                {
                    // Import program test execution body
                    //'path/filename.js' is the compiled result of project/src/main.ts
                    main: `importScripts('path/filename.js')`,
                },

                // Resource Name,
                "bfchainv3-resc",
                
                // Resource Type
                "bfs/exec"
            );

            return {app, runner };
        }

        ```

    - project/bundle/typings/src/@types.ts file
        ```javascript
        // Declaration type
        declare namespace TypeName{
    
        }
        ```
        
    - project/bundle/typings/src/index.ts file
        ```javascript
        // Import typings of other modules
        import "other/typings";
        import "./@types";
        ```
    
    - project/bundle/typings/bfsp.json file
    
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
    
        
    
    - project/bundle/bfsp.json file
    
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
    
    - project/src/index.ts file
        ```
        // temporarily not used
        ```
    
    - project/src/main.ts file
    
        ```javascript
        // Import runtime
        import "@bfs/bfchain-runtime-typings";
            
        
        // Gateway After the conditions are matched, the process will ask whether to raise the card
        // If the card is not raised, the code in the bfsprocess.onLinkedBrl.attach() part will not be executed;
        bfsprocess.onGatwayBrl.attach(info => {
            //You can use info to judge whether to raise a card
            info.offer(1)
        })
        
        // After the card is raised, if the process selects to execute the current application, the following callback will be executed
        bfsprocess.onLinkedBrl.attach((linked) =>{
            // You can judge whether to execute the program through linked
            const {createActivity} = bfsprocess.import("@bfs/lib-dom-activity");
            const {registerActivity} = bfsprocess.import("@bfs/lib-dom-activity");
            const afterInited = (tuiActivity: BFS.Lib.DomActivity) => {
        
                // The executive body of the program...
                // Load other WEB applications
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
    
    - project/typings/src/@types.ts file
    
      ```javascript
      // Declaration type
      declare namespace TypeName{
      
      }
      ```
    
      
    
    - project/typings/src/index.ts file
    
      ```javascript
      // Import typings of other modules
      import "other/typings";
      import "./@types";
      ```
    
      
    
    - project/typings/bfsp.json file
    
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
    
      
    
    - project/bfsp.json file 
    
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
    
      
    
- ### ② Custom instruction

    - project/bundle/src/index.ts file
        ```javascript
        import * as bfs from "@bfs/core";
        import "packagename-typings";

        export function commandsAppBuilder() {
            const app = bfs.appBundle("cli.tui.core.commands.org", "Core", 1, {
                supportMimes: ["core"],
                shared: {
                    // Statically register comamndMime and command metadata
                    // shared.commands is the standard indicating that the internal data object is instruction type data
                    commands:{
                        // core is understood as an instruction type
                        // ["help", "install", "open"] === command The command set of all packages below
                        // Usage:
                        //> core help
                        //> core install
                        //> core open test://home.org
                        core: ["help", "install", "open"]
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
        
            return {app, runner };
        }
        ```
        
    - project/bundle/typings/src/@types.ts file
        ```javascript
        // Declaration type
        declare namespace TypeName{

        }
        ```
        
    - project/bundle/typings/src/index.ts file
    
        ```javascript
        // Import typings of other modules
        import "other/typings";
        import "./@types";
        ```
    
        
    
    - project/bundle/typings/bfsp.json file
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
    
    - project/bundle/bfsp.json file
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
    
    - project/src/index.ts file
        ```javascript
        // Temporarily can be empty
        ```
    
    - project/src/main.ts file
        ```javascript
        import "@bfs/bfchain-runtime-typings";
        const bfs = bfsprocess.import('bfs');
        const register = bfs.getRegister("cli.tui.org")
        const BTPTools = bfs.BTPTools;
        register('core','help', (commandID: number, send: {(responseMessage: string):void}, params?: string) => {
            // execute instruction body
            // Send a message to Tui via send
            send(BTPTools.createResMessageNormal("The first message", commandID, 0))
            // Notify Tui to stop receiving messages and the current command is executed
            send(BTPTools.createEntryInputStateNormalProtocol(commandID)) /** will also close tui to accept messages */
        })
        ```
    
    - project/typings/src/@types.ts file
        ```javascript
        // Declaration type
        declare namespace TypeName{
        
        }
        ```
    
    - project/typings/src/index.ts file
        ```javascript
        // Import typings of other modules
        import "other/typings";
        import "./@types";
        ```
    
    - project/typings/bfsp.json file
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
    
    - project/bfsp.json file 
    
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

