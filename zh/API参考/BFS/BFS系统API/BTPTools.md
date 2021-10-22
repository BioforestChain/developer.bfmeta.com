## BTPTools

### 1. 描述
BFS-TUI通信协议生成与解析工具[bfsprocess.import('bfs').BTPTools]

### 2. 方法

- **BTPTools.parseHead(commandMessage: string): { [key: string]: string}**
    - 参数说明：
        ```
        commandMessage  Tui发送过来的协议
        ```
    
    - 描述： 解析指令消息的头部信息
        ```
        const commandMessage = `COMMANDER BTP/1.0.0
        content-type:applicationjson
        charset:utf-8
        method:commander
        
        {"type":"string","commandID":161173507822,"body":"core help"}
        
        --------end` 
        const head = BTPTools.parseHead(commandMessage)
        
        运行结果：
        {
            protocol: "COMMANDERBTP/1.0.0 ", 
            content-type: "application/json", 
            charset: "utf8", 
            method: "commander"
        }
        
        ```
    
    
- **BTPTools.parseBodyByBTPString(commandMessage: string): { [key: string]: string}**
    - 参数说明：
        ```
        commandMessage  Tui发送过来的协议
        ```
    
    - 描述： 解析指令消息的主体信息
        ```
        const commandMessage = `COMMANDER BTP/1.0.0
        content-type:applicationjson
        charset:utf-8
        method:commander

        {"type":"string","commandID":161173507822,"body":"core help"}

        --------end` 
        const body = BTPTools.parseBodyByBTPString(commandMessage)
        
        运行结果：
        {
            body: "core help"
            commandID: 1611735078633
            type: "string"
        }
        ```
    
- **BTPTools.parseCommandIDByCommandMessage(commandMessage: string): number**
    - 参数说明：
        ```
        commandMessage  Tui发送过来的协议
        ```
    - 描述: 从指令消息中解析出commandID
        ```
        const commandMessage = `COMMANDER BTP/1.0.0
        content-type:applicationjson
        charset:utf-8
        method:commander

        {"type":"string","commandID":161173507822,"body":"core help"}

        --------end` 
        const commandID = BTPTools.parseCommandIDByCommandMessage(commandMessage)
        
        运行结果：
        161173507822
        ```
    
- **BTPTools.createResMessageNormal(content: string, commandID: number, level: number): string**

    - 参数说明：

      ```
      content       需要发送给TUI 显示的内容
      commandID     匹配的指令ID
      level         内容所在的层级，根据层级决定左侧的空白
      ```

    - 描述：生成普通返回消息的协议

      ```
      BTPTools.createResMessageNormal("测试文本", 1111111, 0)

      运行结果：
      `COMMANDER BTP/1.0.0
       content-type:applicationjson
       charset:utf-8
       method:commander
      
       {"type":"string","mateCommandID":1111111,"body":{"level":0,"isBlod":false,"isSpace":false,"isBlock":true,"color":"white","conent":"测试文本","type":"string"}}
      
       --------end` 
      ```

      

- **BTPTools.createResMessageNormalErr(content: string, commandID: number): string**
    - 参数说明：

      ```
      content     需要发送给TUI 显示的内容
      commandID   匹配的指令ID
      ```

      
    - 描述：生成普通错误类型的返回信息协议

      ```
      BTPTools.createResMessageNormalErr("错误信息", 1111111)
      
      运行结果：
      `COMMANDER BTP/1.0.0
       content-type:applicationjson
       charset:utf-8
       method:commander
      
       {"type":"string","mateCommandID":1111111,"body":{"level":0,"isBlod":false,"isSpace":false,"isBlock":true,"color":"red","conent":"错误信息","type":"string"}}
      
       --------end` 
      ```

      

- **BTPTools.createResMessageLoading(commandID: number): string**
    - 参数说明：

      ```
      commandID  匹配的指令ID
      ```

      

    - 描述: 生成 loading 状态的返回信息协议

      ```
      BTPTools.createResMessageLoading(1111111)
      
      运行结果：
      `COMMANDER BTP/1.0.0
       content-type:applicationjson
       charset:utf-8
       method:commander
      
       {"type":"string","mateCommandID":1111111,"body":{"level":0,"isBlod":false,"isSpace":false,"isBlock":false,"color":"white","conent":"-","type":"string"}}
      
       --------end` 
      ```

      

- **BTPTools.createResMessageTable(conent: string[][], commandID: number): string**
    - 参数说明：

        ```
        content      需要发送给TUI 显示的内容
        commandID    匹配的指令ID
        ```

    - 描述：生成 表格类型的返回信息协议

        ```
        BTPTools.createResMessageTable([["name","age"],["bill",10]],1111111)
        
        运行结果:
        `COMMANDER BTP/1.0.0
        content-type:applicationjson
        charset:utf-8
        method:commander
        
        {"type":"string","mateCommandID":1111111,"body":{"level":0,"isBlod":false,"isSpace":false,"isBlock":true,"color":"white","conent":[["name","age"],["bill",10]],"type":"arrayT"}}
        
        --------end` 
        ```

- **BTPTools.createEntryInputStateNormalProtocol(commandID: number): string**
    
    - 参数说明：
    
      ```
      commandID   匹配的指令ID
      ```
    
    - 描述：生成 使TUI进入普通输入模式的协议

        ```
        BTPTools.createEntryInputStateNormalProtocol(1111111)

        运行结果:
        `COMMANDER BTP/1.0.0
        content-type:applicationjson
        charset:utf-8
        method:commander

        {"type":"input","mateCommandID":1111111,"tuiState":"INTPUT_SINGLE_LINE"}}

        --------end` 
        ```

- **备注说明：**
    
    - 在发送后TUI 会忽略之后发送的相同commandID的消息
