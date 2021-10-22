## BFS-TUI Data Transfer Protocol [BTP]

### 1. Basic description

#### The role of the agreement
It is used by BFS to tell TUI how to display content, and TUI to tell BFS what instructions to execute.

#### Message description

##### character set:
- **utf8**

##### Basic format
```javascript
dataHeader-dataBody-dataEnd
```
- `dataHeader` header information
- `dataBody` data body
- `dataEnd` data end information
- Use `\r\n\r\n` to separate

    
###### `dataHeader` Description:
- Used to describe the meta data of the message
- The first line `COMMANDER BTP/1.0.0` is used to define that the current message is a `commander` command; the message adopts the `BTP/1.0.0` standard;
- Follow other header information
    - Other header information:
        - content-type data body format; possible values ​​`application/json` | `text/plain` | `text/html` [application/json supported in the initial stage]
        - charset The character set used by the data body; possible value `utf8`
        - method Operation method Possible values ​​`commander` | `post`
            - `commander` command call
            - `post` to pass data
            - Method needs to be used when sending a message from the render/render is not necessary when receiving a message

###### `dataBody` description:
- The main part of the data used to transmit
- The data subject is in JSON format as the standard
```javascript
{
    // Types of
    type: "input" | "res",

    // associated commandID
    mateCommandID:number

    // tui current status description
    // WATI_RES waiting for the command message to return
    // INTPUT_SINGLE_LINE enter the normal input mode
    // INPUT_MULTI_LINE enters multi-line input mode
    // INPUT_EDIT_FILE enter the edit file mode
    tuiState: "WATI_RES" | "INTPUT_SINGLE_LINE" | "INPUT_MULTI_LINE" | "INPUT_EDIT_FILE",

    // Menu list
    // tuiState === INPUT_MULTI_LINE requires menu attribute
    menu?: string[],

    // The content of the text that needs to be edited
    // tuiState === INPUT_EDIT_file requires body attribute;
    body: string;

    // message body
    body: {
        // Level-reference value of left margin
        level: number;
        
        // Whether it is bold
        isBlod: boolean;
        
        // Whether it is a space placeholder, such as true ===, the content of content will be ignored;
        isSpace: boolean;
        
        // Whether the block-level element true === occupies a line by itself false === will occupy the same line as the flying block-level elements before and after;
        isBlock: boolean;
        
        // text color
        color:'red' |'blue' |'grey' |'yellow' |'white';
        
        // text content
        content: string | Array<Array<string>>,

        // Define the type of data passed
        type:'string' |'arrayT';
    }
}

```

###### `dataEnd` The end information of the data:
- Used to indicate whether the current message is an independent and complete message or just a partial data; possible values ​​`--------end` | `--------inc`
- `--------end` === The current `dataBody` is a complete data;
- `--------inc` === The current `dataBody` is a partial data;



### 2. Example description
#### Message example:
- Original format: `COMMANDER\0BTP/1.0.0\r\ncontent-type:application/json\r\ncharset:utf8\r\n\r\n{\r\n"type": "res",\ r\n"body":{\r\n"level":0,"isBlod":true\r\n}\r\n}\r\n\r\r\n--------end`


- format:
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

- Description:
```
// dataHeader
// Message protocol
COMMANDER BTP/1.0.0
// dataBody format
content-type:application/json
// Character set used by dataBody
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

        operation result:
        `COMMANDER BTP/1.0.0
        content-type:applicationjson
        charset:utf-8
        method:commander

        {"type":"input","mateCommandID":1111111,"tuiState":"INTPUT_SINGLE_LINE"}}

        --------end`
        ```

- **instruction manual:**
    
    - After sending, TUI will ignore messages with the same commandID sent later