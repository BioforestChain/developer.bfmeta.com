# Glossary

## Event Type

| **Event Type ID** | **Event Type Name** |
| -------------------- | ---------- |
| `BFM-BFMeta-BSE-01` | Set security password |
| `BFM-BFMeta-BSE-02` | Registered Forger |
| `BFM-BFMeta-BSE-03` | Governance voting |
| `BFM-BFMeta-BSE-04` | Set address alias |
| `BFM-BFMeta-BSE-05` | Receive votes |
| `BFM-BFMeta-BSE-06` | Decline to vote |
| `BFM-BFMeta-WOD-00` | Create DAPPID |
| `BFM-BFMeta-WOD-01` | DAPPID payment |
| `BFM-BFMeta-EXT-00` | Data Attestation |
| `BFM-BFMeta-AST-00` | Issuing rights |
| `BFM-BFMeta-AST-01` | Equity Transfer |
| `BFM-BFMeta-AST-02` | Destruction of rights and interests |
| `BFM-BFMeta-AST-03` | Initiate a gift of rights and interests |
| `BFM-BFMeta-AST-04` | Accept the gift of rights and interests |
| `BFM-BFMeta-AST-05` | Initiate equity entrustment |
| `BFM-BFMeta-AST-06` | Sign for entrusted rights |
| `BFM-BFMeta-AST-07` | Transfer of main interest |
| `BFM-BFMeta-AST-08` | Main interests moved in |
| `BFM-BFMeta-AST-09` | Initiate equity exchange |
| `BFM-BFMeta-AST-10` | Accept equity exchange |
| `BFM-BFMeta-AST-11` | Initiate an asset exchange |
| `BFM-BFMeta-AST-12` | Accept asset exchange |
| `BFM-BFMeta-LNS-00` | Register/Cancel seat name |
| `BFM-BFMeta-LNS-01` | Set the analytic value of the bit name |
| `BFM-BFMeta-LNS-02` | Set the name of the administrator |
| `BFM-BFMeta-CUS-00` | Individual Event | 

## Node status

| **Node Status** | **Node Status Description** |
| -------- | ------------ |
| `0` | Offline: Not available |
| `1` | Free state, free resources are available |
| `2` | Rebuilding the blockchain |
| `3` | Node consensus |
| `4` | Replay block |
| `5` | Forging block |
| `6` | Rollback block |

## Participation

A value calculated by a specific algorithm based on the amount of events the account participated in in each block and the balance of the previous round. The participation of the account will determine the participation of the block, and will be converted into the number of votes that can participate in the voting. These votes can be sent to the designated account (or yourself) in the form of voting in a new round. In the last block of this round, the total number of votes obtained by all the voting accounts of the current round will be calculated and selected Block accounts can be forged in the next round. It should be noted that the number of votes will be recalculated every round.

## payloadHash

The value formed by concatenating the hash values ​​of all events processed by the current block is used to verify whether the block is generated correctly and whether the block has been tampered with by others.

## Node Administrator

The owner of the current node can assign the management authority of the current node to others. When other people hold the assigned authority, they can become the administrator of the current node and can perform management operations on the current node. The method of distribution management is to authorize the designated blockchain address. As long as you hold the private key of the address, you are the administrator of the node. A node owner can appoint multiple node administrators.

## Name System

Location name system, users can locate the dynamically changing latitude and longitude, IP address or blockchain address through a specific name on the Biological Chain Forest blockchain.

## Log file name and content description

| **Log file name** | **Log file name content description** |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `logs-calcEquities-testnet.log` | Equity calculation log file (used to store the relevant logs of equity calculation, if the equity calculation is wrong, it is very likely to affect the normal operation of the node) |
| `logs-dealTransaction-testnet.log` | Processing event log file (used to store related logs for processing events. During event processing, the processing failure information of the business scenario will be output. This is because the event is legal when it is received. However, the processing is illegal, which is a very normal situation. For example, an account with only 10 rights and interests made two transfers, each of which was transferred out of 10 rights and interests, which are both legal events for the two events themselves. However, due to the correlation between the pre- and post-processing when processing the event, the latter event will fail to be successfully processed due to insufficient equity, and an error will occur. Therefore, the error in the processing of the event is usually allowed) |
| `logs-externalCommunicate-testnet.log` | External connection communication file (used to store the log of the external communication process, this process is a bridge for command line tools or other process access, and does not affect the operation of the main process) |
| `logs-forger-testnet.log` | Forging block log file (used to store all important data and logs of important processes during the operation of the node. It is the most important log file for the node. If only the warning type log appears, then It can run normally. If there is an error, it may affect the running of the program. Search for errors as much as possible) So when an error of the error level occurs, it means that the node has encountered some failures. Under normal circumstances, the node will try to repair these problems by itself, such as distribution Fork, block verification failed, synchronization failure, etc. When the node is in a stagnant state for a long time, it means that the node may have an unrepairable fault. At this time, the log should be collected and sent to the BCF team of the Biological Chain Forest |
| `logs-memForTemp-testnet.log` | The log used to store the data generated by the event, generally without any output, if an error occurs, it will affect the normal operation of the node |
| `logs-memInfo-testnet.log` | Accounting log file (used to store accounting related logs, each block shows the number of updated accounts. Normally, there will be no errors, if errors occur, it will affect The node is running normally) |
| `logs-peerScan-testnet.log` | Node scan log file (used to store the logs generated by the node scan during node operation, there will be related logs whenever a node is connected or disconnected) |
| `logs-sharedInfos-testnet.log` | Disk monitoring and shared memory process log file (used to store disk monitoring and shared memory related information logs) |
| `logs-testnet.log` | Main program log file (node ​​is run by multiple processes extended by the main program, the main program will monitor all processes, if any process exits abnormally, there will be related logs in the main program Prompt) |
| `logs-untreatedTrs-testnet.log` | Untreated event log file (At the end of each block, the number of untreated events stored in the node will be displayed) |
| `commander.log` | Command-line tool log file (used to store the log printed after the command-line tool is running, so that it is difficult to view the used command when the command line execution returns too many results)                                                                                                                                                                |
