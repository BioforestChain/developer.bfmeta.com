# Node management interface
## 1.Safely shut down the node
- The full name of the interface：`safetyClose`
- The abbreviation of the interface：`sfc`
- Callable method：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`Address：`/api/system/safetyClose`
- Request parameters：
```typescript
interface SafetyClose {
    /**Verification type: 001 Node owner verification 002 Administrator verification*/
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    /**Whether the machine needs to be shut down，true means shutting down false means not shutting down */
    isShutdown?: boolean;
    isRestart?: boolean;
}

```
- Return parameters：
```typescript
interface SafetyClose {
    /**success or not*/
    success: boolean;
    result: {
        /**Node status，See node status for details */
        machineStatus: number;
    };
}

```
## 2.Set node password
- The full name of the interface：`setSystemKey`
- The abbreviation of the interface：`ssk`
- Callable method：`Http,Websocket,command line`
- Calling method：`post`
- interface`url`address：`/api/system/setSystemKey`
- Request parameters：
```typescript
interface SetSystemKey {
    /**Node's old password */
    systemKeyOld: string;
    /**Node's new password */
    systemKeyNew: string;
    /**Whether to decrypt the new password in asymmetrical way(true: use asymmetrical way for decryption, false: do not use asymmetrical way for decryption, clear text transmission) */
    newKeyDecryptEnable?: boolean;
}

```
- Return parameters：
```typescript
interface SetSystemKey {
    /**Success or not */
    success: boolean;
    result: boolean;
}

```
## 3.Set administrator password
- The full name of the interface：`setManagerKey`
- The abbreviation of the interface：`smk`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/setManagerKey`
- Request parameter：
```typescript
interface SetManagerKey {
    /**Node password */
    systemKey: string;
    /**Administrator password */
    managerKey: string;
}

```
- Return parameter：
```typescript
interface SetSystemKey {
    /**Success or not */
    success: boolean;
    result: boolean;
}

```
## 4.Add new available node
- The full name of the interface：`importPeerTable`
- The abbreviation of the interface：`ipt`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/importPeerTable`
- Request parameters：
```typescript
interface ImportPeerTable {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    peerTable: {
        mac: string;
        ip: string;
    }[];
}

```
- Return parameters：
```typescript
interface ImportPeerTable {
    /**Success or not */
    success: boolean;
    result: boolean;
}

```
## 5.Remove node list
- The full name of the interface：`removePeerList`
- The abbreviation of the interface：`rpl`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/removePeerList`
- Request parameters：
```typescript
interface RemovePeerList {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    ipList: string[];
}

```
- Return parameter：
```typescript
interface RemovePeerList {
    /**Success or not */
    success: boolean;
    result: boolean;
}

```
## 6.Get node list
- The full name of the interface：`getPeersList`
- The abbreviation of the interface：`gpl`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/getPeersList`
- Request parameter：none
- Return parameter：none
## 7.Used to determine whether the local ip exists
- The full name of the interface：`verifyIpExist`
- The abbreviation of the interface：`vipe`
- Callable methods：`Http,Websocket`
- Calling method：`post`
- Interface`url`address：`/api/system/verifyIpExist`
- Request parameter：none
- Return parameter：none
## 8.Send the attestation event on the ip chain
- The full name of the interface：`sendIpMarkTr`
- The abbreviation of the interface：`sipmt`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/sendIpMarkTr`
- Request parameter：
```typescript
interface SendIpMarkTransaction {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    ip: string;
    dappid: string;
}

```
- Return parameter：none
## 9.Verify node password
- The full name of the interface：`verifySystemKey`
- The abbreviation of the interface：`vfs`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/verifySystemKey`
- Request parameter：
```typescript
interface VerifySystemKey {
    /**Check password value */
    verifyKey: string;
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
}

```
- Return parameter：
```typescript
interface VerifySystemKey {
    /**Success or not */
    success: boolean;
    result: boolean;
}

```
## 10.Bind node account
- The full name of the interface：`bindingAccount`
- The abbreviation of the interface：`ba`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/bindingAccount`
- Request parameter：
```typescript
interface BindingAccount {
    /**Node password */
    systemKey: string;
    /**Encrypted trustee private key */
    cryptoSecretKey: string;
    /**Encrypted trustee public key */
    cryptoPublicKey: string;
    /**Encrypted trustee security private key */
    cryptoSecondSecretKey?: string;
    /**Encrypted trustee security public key */
    cryptoSecondPublicKey?: string;
}

```
- Return parameter：
```typescript
interface BindingAccount {
    /**Success or not */
    success: boolean;
    result: {
        /**Address */
        address: string;
        /**Public key */
        publicKey: string;
        /**Security key */
        secondPublicKey?: string;
        /**Add time */
        delegateAddTime: number;
    };
}

```
## 11.Get the node trustee
- The full name of the interface：`getSystemDelegate`
- The abbreviation of the interface：`gsd`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/getSystemDelegate`
- Request parameters：
```typescript
interface GetSystemDelegate {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
}

```
- Return parameter：
```typescript
interface GetSystemDelegate {
    /**Success or not */
    success: boolean;
    result?: {
        /**Node trustee public key */
        publicKey: string;
        /**The secure password public key injected by the node trustee */
        secondPublicKey?: string;
        /**Current secondary public key of the node trustee */
        currentSecondPublicKey?: string;
        /**Node trustee address */
        address: string;
        /**The timestamp generated from the time when the node is set successfully */
        addTime: number;
        /**Trustee */
        name: string | undefined;
    };
}

```
## 12.Query all forgers injected by this node
- The full name of the interface：`getInjectGenerators`
- The abbreviation of the interface：`none`
- Callable methods：`Http,Websocket`
- Calling method：`post`
- Interface`url`address：`/api/system/getInjectGenerators`
- Request parameter：
```typescript
interface GetInjectGenerators {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
}

```
- Return parameter：
```typescript
interface GetInjectGenerators {
    /**Success or not */
    success: boolean;
    result: {
        /**Array of forgers  */
        injectGenerators: {
            /**Forger address */
            address: string;
            /**Forger username */
            username?: string;
        }[];
    };
}

```
## 13.Query the details of the forger injected by the node
- The full name of the interject：`getSystemDelegateDetail`
- The abbreviation of the interject：`none`
- Callable methods：`Http,Websocket`
- Calling method：`post`
- Interject`url`address：`/api/system/getSystemDelegateDetail`
- Request parameters：
```typescript
interface GetSystemDelegateDetail {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    /**Trustee address */
    address: string;
}

```
- Return parameters：
```typescript
interface GetSystemDelegateDetail {
    /**Success or not */
    success: boolean;
    result: {
        /**The number of main equity held by the account */
        balance: string;
        /**Blocking revenue of the account */
        reward: string;
        /**Accumulated number of blocks forged by the account */
        forgerBlocks: number;
        /**Account address */
        address: string;
        /**Whether it is a trustee */
        isDelegate: boolean;
        /**Account alias */
        username?: string;
    };
}

```
## 14.Get node details
- The full name of the interface：`getSystemNodeInfo`
- The abbreviation of the interface：`none`
- Callable methods：`Http,Websocket`
- Calling method：`post`
- Interface`url`address：`/api/system/getSystemNodeInfo`
- Request parameter：
```typescript
interface GetSystemNodeInfo {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
}

```
- Return parameters：
```typescript
interface GetSystemNodeInfo {
    /**Success or not */
    success: boolean;
    result: {
        nodeInfo: {
            /**Node binding address list */
            addressList: {
                address: string;
                isDelegate: boolean;
                username?: string;
            }[];
            /**The cumulative income of the address bound to the node */
            accumulativeReward: string;
            /**Current height */
            height: number;
            /**Node version */
            version: string;
            /**Chain network identifier */
            magic: string;
            /**Chain name */
            chainName: string;
            /**Node operating system type */
            osType: string;
            /**Signature of Genesis block */
            gensisBlockSignature: string;
        };
    };
}

```
## 15.Node information query
- The full name of the interface：`miningMachineInfo`
- The abbreviation of the interface：`mmi`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/miningMachineInfo`
- ：
```typescript
interface MiningMachineInfo {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
}

```
- Return parameters：
```typescript
interface MiningMachineInfo {
    /**Success or not */
    success: boolean;
    result: {
        data: {
            /**Host name */
            hostname: string;
            /**Update function port */
            updateServicePort: number;
            /**Node management function port */
            systemInfoPort: number;
            /**Service market port */
            serviceMarketPort: number;
            /**Blockchain network port */
            port: number;
            /**Operating system */
            platform: string;
            /**Number of CPU cores */
            cpuCount: number;
            /**CPU model information */
            cpuModel: string;
            /**CPU speed */
            cpuSpeed: string;
            /**Memory speed unit: byte */
            totalMem: number;
            /**Memory model */
            memModel: string;
            /**Memory hertz */
            memHz: number;
            /**Disk size unit: byte */
            totalDisk: number;
            /**Disk type */
            diskModel: string;
            /**Whether the node has been set trustee true: the trustee has been set，false: the trustee has not been set */
            delegateStatus: boolean;
        };
    };
}

```
## 16.Set node configuration information
- The full name of the interface：`setSystemConfig`
- The abbreviation of the interface：`ssc`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/setSystemConfig`
- Request parameters：
```typescript
interface SetSystemConfig {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    /**Configuration information, The parameters here can be null */
    config: AllPartial<Config.ConfigRevisable>;
}

```
- Return parameters：
```typescript
interface SetSystemConfig {
    /**Success or not */
    success: boolean;
    result: boolean;
}

```
## 17.Get node configuration information
- The full name of the interface：`getSystemConfigInfoDetail`
- The abbreviation of the interface：`gsci`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/getSystemConfigInfoDetail`
- Request parameters：
```typescript
interface GetSystemConfigInfoDetail {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
}

```
- Return parameters：
```typescript
interface GetSystemConfigInfoDetail {
    /**Success or not */
    success: boolean;
    result: BFMetaPC.Config.ConfigReadable;
}

```
## 18.Get node status（real-time information）
- The full name of the interface：`getRuntimeState`
- The abbreviation of the interface：`grs`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/getRuntimeState`
- Request parameters：
```typescript
interface GetRuntimeState {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
}

```
- Return parameters：
```typescript
interface GetRuntimeState {
    /**Success or not */
    success: boolean;
    result: {
        /**Memory information，JSON object */
        memory: {
            /**Resident set size，how much physical memory is allocated to this process */
            rss: number;
            /**堆的总大小，包含 3 个部分：1、已分配的内存，用于对象的创建和存储，对应于 heapUsed 2、未分配的但可用于分配的内存 3、未分配的但不能分配的内存 The total size of the heap, including 3 parts: 1. Allocated memory, used for object creation and storage, corresponding to heapUsed 2. Memory that is unallocated but available for allocation 3. Memory that is unallocated but unavailable for allocation */
            heapTotal: number;
            /**Allocated memory, that is, the total size of all objects in the heap */
            heapUsed: number;
            /**Memory occupied by the system link library used by the process */
            external: number;
        };
        /**Operating system */
        platform: "aix" | "android" | "darwin" | "freebsd" | "linux" | "openbsd" | "sunos" | "win32" | "cygwin" | "netbsd";
        /**Number of CPU cores */
        cpuCount: number;
        /**Available memory size */
        freemem: number;
        /**Total memory size */
        totalmen: number;
        /**CPU usage，unit：% */
        cpuUsage: number;
        /**CPU average load */
        cpuUsageV2: number[];
        /**CPU model information */
        cpuModel: string;
        /**Process information，JSON object */
        process: {
            /**Process ID */
            pid: number;
            /**Process name */
            name?: string;
        };
    };
}

```
## 19.Get node access statistics
- The full name of the interface：`getSystemMonitor`
- The abbreviation of the interface：`gsm`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/getSystemMonitor`
- Request parameters：
```typescript
interface GetSystemMonitor {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    /**Specify the access type，including the traffic accessing IP，times，the times of accessing interface，number of blocks，event date, etc. */
    monitorType?: string;
    /**The number of queries, for example, limit=10 means that 10 pieces of data can be queried. */
    limit?: number;
    /**The starting position of the query, for example, offset = 0 means that the query starts from the first line. */
    offset?: number;
}

```
- Return parameters：
```typescript
interface GetSystemMonitor {
    /**Success or not */
    success: boolean;
    result: {
        systemMonitor: {
            /**IP statistical information */
            requestIpMonitorInfo?: {
                /**IP information object，JSON object */
                [ip: string]: {
                    /**Number of visits */
                    requestCount: number;
                    /**Number of access errors */
                    requestErrorCount: number;
                    /**Number of access denials */
                    requestRejectCount: number;
                    /**Download traffic*/
                    downloadFlow: number;
                    /**Upstream traffic */
                    uploadFlow: number;
                };
            };
            /**Access method statistics，JSON object */
            requestMethodInfo?: {
                /**Access method statistics，JSON object */
                [method: string]: {
                    /**Number of visits */
                    count: number;
                };
            };
            /**Access path statistics，JSON object */
            requestPathInfo?: {
                /**Access path statistics，JSON object */
                [path: string]: {
                    /**Number of visits */
                    count: number;
                };
            };
            /**Access url statistical information，JSON object */
            requestApiUrlInfo?: {
                /**Access url statistics，JSON object */
                [apiUrl: string]: {
                    /**Number of visits */
                    count: number;
                };
            };
            /**Access interface name statistics，JSON object */
            requestMethodNameInfo?: {
                /**Access interface name statistics，JSON object */
                [methodName: string]: {
                    /**Number of visits */
                    count: number;
                };
            };
            /**Access account statistics，JSON object */
            requestAccountInfo?: {
                /**Access account statistics，JSON object */
                [account: string]: number;
            };
            /**Node event statistic，JSON object */
            transaction?: {
                /**Number of node event equity */
                amount: string;
                /**Number of node event fees */
                fee: string;
            };
            /**Statistics of various node events，JSON object */
            trsCount?: {
                /**Statistics of various node events，JSON object */
                [key: string]: number;
            };
            /**Block information，JSON object */
            blockCount?: {
                /**Block height */
                height: number;
                /**Block generation timestamp */
                time: number;
                /**Number of block rewards  */
                reward: string;
                /**Block size */
                blockSize: number;
                /**Number of block events */
                numberOfTransactions: number;
            }[];
        };
    };
}

```
## 20.Get the type of node running log
- The full name of the interface：`getSystemLoggerType`
- The abbreviation of the interface：`glt`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/getSystemLoggerType`
- Request parameters：
```typescript
interface GetSystemLoggerType {
    /**Verification type: 001 node owner verification 002 administration verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
}

```
- Return parameters：
```typescript
interface GetSystemLoggerType {
    /**Success or not */
    success: boolean;
    result: {
        /**Log type，array */
        loggerType: {
            /**Log type */
            loggerType: string;
            /**Log file name */
            loggerName: string;
            /**Number of logs of this type */
            loggerAmount: number;
            /**Total size of log of this type，unit：byte */
            loggerSize: number;
            /**Last time when the log of this type was created */
            loggerUpdateTime: number;
        }[];
    };
}

```
## 21.Get the list of node running log
- The full name of the interface：`getSystemLoggerList`
- The abbreviation of the interface：`gll`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/getSystemLoggerList`
- Request parameters：
```typescript
interface GetSystemLoggerList {
    /**Verification type: 001 node owner verification 002 administration verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    /**Node type */
    loggerType: string;
}

```
- Return parameters：
```typescript
interface GetSystemLoggerList {
    /**Success or not */
    success: boolean;
    result: {
        /**Node running log list（a certain type of log），JSON object */
        loggerList: {
            /**Log file name */
            logName: string;
            /**Time when log was last modified */
            logUpdateTime: number;
            /**Log file size */
            logSize: number;
            /**Log file path */
            logPath: string;
        }[];
    };
}

```
## 22.Get the content of node running log
- The full name of the interface：`getSystemLoggerDetail`
- The abbreviation of the interface：`gld`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/getSystemLoggerDetail`
- Request parameters：
```typescript
interface GetSystemLoggerDetail {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    /**Log file name */
    loggerName: string;
    /**Number of queries, for example, limit=10 means that 10 pieces of data can be queried. */
    limit?: number;
    /**The starting position of the query, for example, offset = 0 means that the query starts from the first line. */
    offset?: number;
    /**Search string */
    searchString?: string;
    /**Method reading files */
    readFileType?: {
        readFileAsync = 0,
        createReadStream = 1,
    };
}

```
- Return parameters：
```typescript
interface GetSystemLoggerDetail {
    /**Success or not */
    success: boolean;
    result: {
        /**The content of Node running log, array format, the content of all detail lines */
        loggerDetail: string[];
        /**Total lines of content */
        linesTotal: number;
    };
}

```
## 23.Delete node running log
- The full name of the interface：`delSystemLogger`
- the abbreviation of the interface：`none`
- Callable methods：`Http,Websocket`
- Calling method：`post`
- Interface`url`address：`/api/system/delSystemLogger`
- Request parameters：
```typescript
interface DelSystemLogger {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    /**Log file name */
    loggerName: string;
}

```
- Return parameters：
```typescript
interface DelSystemLogger {
    /**Success or not */
    success: boolean;
    result: boolean;
}

```
## 24.Get the node email address
- The full name of the interface：`getEmailAddress`
- The abbreviation of the interface：`gea`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/getEmailAddress`
- Request parameters：
```typescript
interface GetEmailAddress {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    /**The email address to be queried */
    emailAddress?: string;
}

```
- Return parameters：
```typescript
interface GetEmailAddress {
    /**Success or not */
    success: boolean;
    result: {
        /**Email receiving address */
        emailToAddress: string;
        /**Email sending address */
        emailFromAddress: string;
        /**Email configuration */
        emailConfig: {
            /**Email configuration type，POP3/SMTP/IMAP */
            type: string;
            /**Email configuration host */
            host?: string;
            /**Email configuration port */
            port?: number;
            /**Whether to enable email security control */
            secureConnection?: boolean;
            /**Whether to enable ssl */
            ssl?: boolean;
            /**Whether to enable tls */
            tls?: boolean;
            /**Transfer email configuration information */
            auth?: {
                /**Transfer email configuration user name */
                user: string;
                /**Transfer email configuration password */
                pass: string;
            };
        };
    };
}

```
## 25.Set the node email address
- The full name of the interface：`setEmailAddress`
- The abbreviation of the interface：`sea`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/setEmailAddress`
- Request parameters：
```typescript
interface SetEmailAddress {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    /**Email receiving address */
    emailToAddress: string;
    /**Email sending address */
    emailFromAddress: string;
    /**Email configuration */
    emailConfig: {
        /**Email configuration type，POP3/SMTP/IMAP */
        type: string;
        /**Email configuration host */
        host?: string;
        /**Email configuration port */
        port?: number;
        /**Whether to enable email security control */
        secureConnection?: boolean;
        /**Whether to enable ssl */
        ssl?: boolean;
        /**Whether to enable tls */
        tls?: boolean;
        /**Transfer email configuration information */
        auth?: {
            /**Transfer email configuration user name */
            user: string;
            /**Transfer email configuration password */
            pass: string;
        };
    };
}

```
- Return parameters：
```typescript
interface SetEmailAddress {
    /**Success or not */
    success: boolean;
    result: boolean;
}

```
## 26.Get the network-related information of the node process
- The full name of the interface：`getProcessNetwork`
- The abbreviation of the interface：`gpn`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/getProcessNetwork`
- Request parameters：
```typescript
interface GetProcessNetwork {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    /**Number of queries, for example, limit=10 means that 10 pieces of data can be queried. */
    limit?: number;
    /**The starting position of the query, for example, offset = 0 means that the query starts from the first line */
    offset?: number;
    /**Process type */
    processType?: string;
}

```
- Return parameters：
```typescript
interface GetProcessNetwork {
    /**Success or not */
    success: boolean;
    result?: {
        /**Process network information object，JSON object */
        processNetwork: {
            /**Process type */
            processType: string;
            /**Process name */
            name: string;
            /**Process sending network traffic */
            send: number;
            /**Process receiving network traffic */
            receive: number;
            /**Total network sent and received by the process */
            sum: number;
        }[];
        count: number;
    };
}

```
## 27.Get node process CPU information
- The full name of the interface：`getProcessCPU`
- The abbreviation of the interface：`gpc`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/getProcessCPU`
- Request parameters：
```typescript
interface GetProcessCPU {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    /**Number of queries, for example, limit=10 means that 10 pieces of data can be queried. */
    limit?: number;
    /**The starting position of the query, for example, offset = 0 means that the query starts from the first line */
    offset?: number;
    /**Process type */
    processType?: string;
}

```
- Return parameters：
```typescript
interface GetProcessCPU {
    /**Success or not */
    success: boolean;
    result?: {
        /**Process CPU status，JSON object */
        processCPU: {
            /**Process type */
            processType: string;
            /**Process name */
            name: string;
            /**Process CPU usage */
            percent: number;
            /**Process CPU status */
            state: string;
        }[];
        count: number;
    };
}

```
## 28.Get node process memory information
- The full name of the interface：`getProcessMemory`
- The abbreviation of the interface：`gpm`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/getProcessMemory`
- Request parameters：
```typescript
interface GetProcessMemory {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
    /**Number of queries, for example, limit=10 means that 10 pieces of data can be queried. */
    limit?: number;
    /**The starting position of the query, for example, offset = 0 means that the query starts from the first line */
    offset?: number;
    /**Process type */
    processType?: string;
}

```
- Return parameters：
```typescript
interface GetProcessMemory {
    /**Success or not */
    success: boolean;
    result?: {
        /**Process memory status，JSON object */
        processMemory: {
            /**Process type */
            processType: string;
            /**Process name */
            name: string;
            /**Process shared memory size */
            share: number;
            /**Process dedicated memory size */
            rss: number;
        }[];
        count: number;
    };
}

```
## 29.Send node status regularly
- The full name of the interface：`systemStatus`
- The abbreviation of the interface：`ess`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/systemStatus`
- Request parameters：
```typescript
interface SystemStatus {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
}

```
- Return parameters：
```typescript
interface SystemStatus {
    /**Success or not */
    success: boolean;
    result: {
        systemStatus: {
            /**CPU usage，array */
            cpusStatus: {
                /**Which generation of code name does the CPU belong to in its series */
                model: string;
                /**CPU main frequency（clock frequency） */
                speed: number;
                /**Time-consuming statistics */
                times: {
                    /**The number of milliseconds the CPU spends in user mode */
                    user: number;
                    /**The number of milliseconds the CPU spends in good mode */
                    nice: number;
                    /**The number of milliseconds the CPU spends in system mode */
                    sys: number;
                    /**The number of milliseconds the CPU spends in idle mode */
                    idle: number;
                    /**The number of milliseconds the CPU spends in interrupt request mode */
                    irq: number;
                };
            }[];
            /**Memory usage */
            memStatus: {
                /**Available memory size */
                freeMem: number;
                /**Total memory size */
                totalmem: number;
            };
            /**System bandwidth */
            bandwithStatus: {
                /**Total bandwidth */
                total: number;
                /**Utilized bandwidth */
                usage: number;
            };
            /**Disk usage */
            diskSpace: {
                /**Available disk size */
                free: number;
                /**Total */
                size: number;
            };
            /**Miner running status：0 initializing，unavailable 1 restarting，2 closing，3 running */
            machineStatus: number;
            /**Node status：1 rebuild blockchain，2 synchronizing，3 idle state，4 forge blocks，5 rollback blocks */
            peerStatus: number;
            /**Trustee information */
            deletegateInfo: {
                /**Voting yield */
                reward: string;
                /**Number of votes in the current round */
                voteCount: string;
                /**Number of blocks forged by trustee */
                forgerBlocks: number;
            };
            /**Miner version name */
            miningMachineVersionName: string;
            /**CPU usage */
            cpuUsage: number;
            /**Current block height */
            currentHeight: number;
            /**Synchronize to the highest height in the node list */
            maxHeight: number;
        };
    };
}

```
## 30.Send node CPU, memory and network information regularly
- The full name of the interface：`systemProcess`
- The abbreviation of the interface：`esp`
- Callable methods：`Http,Websocket,command line`
- Calling method：`post`
- Interface`url`address：`/api/system/systemProcess`
- Request parameters：
```typescript
interface SystemProcess {
    /**Verification type: 001 node owner verification 002 administrator verification */
    verifyType: string;
    /**Check value，according to the different permissions of the node visitor, the node owner checks the password, and the administrator checks the address */
    verifyKey: string;
}

```
- Return parameters：
```typescript
interface SystemProcess {
    /**Success or not */
    success: boolean;
    result: {
        /**Node CPU，memory，network information，JSON object */
        systemProcess: {
            /**CPU usage，array */
            usageCPU: {
                // Number of processes
                processCount: number;
                // Speed
                speed: number;
                // Uptime
                runtime: number;
                // Number of threads
                threadCount: number;
                // Number of handles
                handleCount: number;
                // CPU usage
                CPUusage: number;
            };
            /**Memory usage */
            usageMemory: {
                // Physical memory
                physcial: number;
                // Physical memory usage 
                physcialUsed: number;
                // Free physical memory 
                physcialWait: number;
                // Virtual memory
                virtual: number;
                // Virtual memory usage
                virtualUsed: number;
                // Free virtual memory
                virtualWait: number;
            };
            /**Network usage */
            usageNetwork: {
                // Receiving network traffic
                receive: number;
                // Sending network traffic
                send: number;
                // Total network traffic
                sum: number;
            };
            /**Process usage */
            processData: {
                /**CPU usage percentage */
                CPUPercent: number;
                /**Memory usage percentage */
                memoryPercent: number;
                /**Send per second */
                send: number;
                /**Receive per second */
                receive: number;
            };
        };
    };
}

```
## 31.Get node information, and different forms of node information will be returned according to the different parameters input 
- The full name of the interface：`getSystemInfo`
- The abbreviation of the interface：`none`
- Callable methods：`Http,Websocket`
- Calling method：`get`
- Interface`url`address：`/api/system/getSystemInfo`
- Request parameter：none
- Return parameters：
```typescript
interface GetSystemInfo {
    /**Success or not */
    success: boolean;
    result: {
        data: {
            /**System ip */
            localIp: string;
            /**System trustee */
            delegateInfo: {
                /**Node trustee public key */
                publicKey: string;
                /**The security password public key injected by the node trustee */
                secondPublicKey?: string;
                /**Current secondary public key of the node trustee */
                currentSecondPublicKey?: string;
                /**Node trustee address */
                address: string;
                /**The timestamp when the node trustee is successfully set */
                addTime: number;
                /**Trustee */
                name: string | undefined;
            } | undefined;
        };
    };
}

```
## 32.Get node status
- The full name of the interface：`getMachineStatus`
- The abbreviation of the interface：`none`
- Callable methods：`Http,Websocket`
- Calling methods：`get`
- Interface`url`address：`/api/system/getMachineStatus`
- Request parameter：none
- Return parameters：
```typescript
interface GetMachineStatus {
    /**Success or not */
    success: boolean;
    result: {
        /**Node status */
        machineStatus: number;
        /**The latest block height of the node */
        height?: number;
    };
}

```
