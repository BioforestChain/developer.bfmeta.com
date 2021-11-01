## BTPTools

### 1. Description
BFS-TUI communication protocol generation and analysis tool [bfsprocess.import('bfs').BTPTools]

### 2. Method

- **BTPTools.parseHead(commandMessage: string): {[key: string]: string}**
    - Parameter Description:
        ```
        Protocol sent by commandMessage Tui
        ```
    
    - Description: Parse the header information of the command message
        ```
        const commandMessage = `COMMANDER BTP/1.0.0
        content-type:applicationjson
        charset:utf-8
        method:commander
        
        {"type":"string","commandID":161173507822,"body":"core help"}
        
        --------end`
        const head = BTPTools.parseHead(commandMessage)
        
        operation result:
        {
            protocol: "COMMANDERBTP/1.0.0 ",
            content-type: "application/json",
            charset: "utf8",
            method: "commander"
        }
        
        ```
    
    
- **BTPTools.parseBodyByBTPString(commandMessage: string): {[key: string]: string}**
    - Parameter Description:
        ```
        Protocol sent by commandMessage Tui
        ```
    
    - Description: Analyze the body information of the command message
        ```
        const commandMessage = `COMMANDER BTP/1.0.0
        content-type:applicationjson
        charset:utf-8
        method:commander

        {"type":"string","commandID":161173507822,"body":"core help"}

        --------end`
        const body = BTPTools.parseBodyByBTPString(commandMessage)
        
        operation result:
        {
            body: "core help"
            commandID: 1611735078633
            type: "string"
        }
        ```
    
- **BTPTools.parseCommandIDByCommandMessage(commandMessage: string): number**
    - Parameter Description:
        ```
        Protocol sent by commandMessage Tui
        ```
    - Description: Parse the commandID from the command message
        ```
        const commandMessage = `COMMANDER BTP/1.0.0
        content-type:applicationjson
        charset:utf-8
        method:commander

        {"type":"string","commandID":161173507822,"body":"core help"}

        --------end`
        const commandID = BTPTools.parseCommandIDByCommandMessage(commandMessage)
        
        operation result:
        161173507822
        ```
    
- **BTPTools.createResMessageNormal(content: string, commandID: number, level: number): string**

    - Parameter Description:

      ```
      content The content that needs to be sent to TUI to display
      commandID matching command ID
      level The level of the content, the left blank is determined according to the level
      ```

    - Description: The protocol for generating normal return messages

      ```
      BTPTools.createResMessageNormal("Test Text", 1111111, 0)

      operation result:
      `COMMANDER BTP/1.0.0
       content-type:applicationjson
       charset:utf-8
       method:commander
      
       {"type":"string","mateCommandID":1111111,"body":{"level":0,"isBlod":false,"isSpace":false,"isBlock":true,"color":"white ","conent":"test text","type":"string"}}
      
       --------end`
      ```

      

- **BTPTools.createResMessageNormalErr(content: string, commandID: number): string**
    - Parameter Description:

      ```
      content The content that needs to be sent to TUI to display
      commandID matching command ID
      ```

      
    - Description: Generate a return message protocol for common error types

      ```
      BTPTools.createResMessageNormalErr("error message", 1111111)
      
      operation result:
      `COMMANDER BTP/1.0.0
       content-type:applicationjson
       charset:utf-8
       method:commander
      
       {"type":"string","mateCommandID":1111111,"body":{"level":0,"isBlod":false,"isSpace":false,"isBlock":true,"color":"red ","conent":"error message","type":"string"}}
      
       --------end`
      ```

      

- **BTPTools.createResMessageLoading(commandID: number): string**
    - Parameter Description:

      ```
      commandID matching command ID
      ```

      

    - Description: Generate a return message protocol for loading status

      ```
      BTPTools.createResMessageLoading(1111111)
      
      operation result:
      `COMMANDER BTP/1.0.0
       content-type:applicationjson
       charset:utf-8
       method:commander
      
       {"type":"string","mateCommandID":1111111,"body":{"level":0,"isBlod":false,"isSpace":false,"isBlock":false,"color":"white ","conent":"-","type":"string"}}
      
       --------end`
      ```

      

- **BTPTools.createResMessageTable(conent: string[][], commandID: number): string**
    - Parameter Description:

        ```
        content The content that needs to be sent to TUI to display
        commandID matching command ID
        ```

    - Description: Generate the return information protocol of the form type

        ```
        BTPTools.createResMessageTable([["name","age"],["bill",10]],1111111)
        
        operation result:
        `COMMANDER BTP/1.0.0
        content-type:applicationjson
        charset:utf-8
        method:commander
        
        {"type":"string","mateCommandID":1111111,"body":{"level":0,"isBlod":false,"isSpace":false,"isBlock":true,"color":"white ","conent":[["name","age"],["bill",10]],"type":"arrayT"}}
        
        --------end`
        ```

- **BTPTools.createEntryInputStateNormalProtocol(commandID: number): string**
    
    - Parameter Description:
    
      ```
      commandID matching command ID
      ```
    
    - Description: Generate a protocol to make TUI enter the normal input mode

        ```
        BTPTools.createEntryInputStateNormalProtocol(1111111)

        operation result :
        `COMMANDER BTP/1.0.0
        content-type:applicationjson
        charset:utf-8
        method:commander

        {"type":"input","mateCommandID":1111111,"tuiState":"INTPUT_SINGLE_LINE"}}

        --------end` 
        ```

- **instruction manual:**
    
     - After sending, TUI will ignore messages with the same commandID sent later 
