## BFS-TUI 数据传输协议[BTP]

### 1. 基础说明

#### 协议作用
用于BFS告知TUI以何种方式显示内容，以及TUI告知BFS执行何种指令。

#### 报文说明

##### 字符集:
- **utf8**

##### 基础格式
```javascript
dataHeader - dataBody - dataEnd
```
- `dataHeader` 头部信息
- `dataBody` 数据主体
- `dataEnd` 数据尾部信息
- 之间采用 `\r\n\r\n` 分隔

    
###### `dataHeader` 说明:
- 用来说明报文的 meta 数据
- 首行 `COMMANDER BTP/1.0.0` 用来定义当前报文是一个 `commander` 指令; 报文采用 `BTP/1.0.0` 标准;
- 后面跟随其他头部信息
    - 其他头部信息：
        - content-type  数据主体格式； 可能的值 `application/json` |  `text/plain` | `text/html` [初始阶段支持 application/json]
        - charset 数据主体采用的字符集； 可能的值 `utf8`
        - method 操作方法 可能的值 `commander` | `post`
            - `commander` 指令调用
            - `post` 传递数据
            - method 从render 发送消息的时候需要使用/ render 接受消息的时候可以不需要

###### `dataBody` 说明：
- 用来传递的数据主体部分
- 数据主体 以 JSON 格式为标准
```javascript
{
    // 类型
    type: "input" | "res",

    // 关联的commandID
    mateCommandID:number

    // tui  当前状态说明
    // WATI_RES 等待指令消息返回
    // INTPUT_SINGLE_LINE 进入普通输入模式
    // INPUT_MULTI_LINE 进入多行输入模式
    // INPUT_EDIT_FILE 进入编辑文件模式 
    tuiState:  "WATI_RES" | "INTPUT_SINGLE_LINE" | "INPUT_MULTI_LINE" | "INPUT_EDIT_FILE",

    // 菜单列表 
    // tuiState === INPUT_MULTI_LINE 需要 menu 属性
    menu?: string[],

    // 需要编辑的文本内容
    // tuiState === INPUT_EDIT_file 需要body属性;
    body: string;

    // 消息主体
    body: {
        // 层级 - 左边距参考值
        level: number; 
        
        // 是否粗体字 
        isBlod: boolean;
        
        // 是否空格占位符 如 true === 会忽略 content 的的内容；
        isSpace: boolean;
        
        // 是否块级元素 true === 独占一行 false === 会和前后的飞块级元素同占一行;
        isBlock: boolean;
        
        // 文本颜色
        color: 'red' | 'blue' | 'grey' | 'yellow' | 'white' ;
        
        // 文本内容
        content: string | Array<Array<string>> ,

        // 定义传递过来的数据类型
        type: 'string' | 'arrayT' ;
    }
}

```

###### `dataEnd` 数据的尾部信息：
- 用来说明当前报文是一个独立完整，还是只是一个局部数据; 可能的值 `--------end` | `--------inc`
- `--------end` === 当前 `dataBody` 是一个完整的数据；
- `--------inc` === 当前 `dataBody` 是一个局部数据；



### 2. 实例说明
#### 报文实例：
-原始格式：  `COMMANDER\0BTP/1.0.0\r\ncontent-type:application/json\r\ncharset:utf8\r\n\r\n{\r\n"type": "res",\r\n"body":{\r\n"level":0,"isBlod":true\r\n}\r\n}\r\n\r\r\n--------end` 


-格式化：
```
COMMANDER BTP/1.0.0
content-type:application/json
charset:utf8
method:commander

{
    "type": "res",
    "body": {
        "level":0,
        "isBlod:true
    }
}

--------end

```

- 说明：
```
// dataHeader
// 报文协议
COMMANDER BTP/1.0.0   
// dataBody格式
content-type:application/json
// dataBody 使用的字符集
charset:utf8

// dataBody
{
    "type": "res",
    "body": {
        "level":0,
        "isBlod:true
    }
}

// dataEnd
--------end

```

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
