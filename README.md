# Pascal Coin: P2P Cryptocurrency without need of historical operations.  
  
Copyright (c) 2016 Albert Molina  
  
THIS IS EXPERIMENTAL SOFTWARE. Use it for educational purposes only.  
  
This software is a Node of the Pascal Coin P2P Cryptocurrency.  
It can be used to Mine and Explore blocks and operations.  
  
Distributed under the MIT software license, see the accompanying file  
LICENSE  or visit http://www.opensource.org/licenses/mit-license.php.  

This product includes software developed by the OpenSSL Project and Denis  
Grinyuk (https://github.com/Arvur/OpenSSL-Delphi), and some  
cryptographic functions inspirated in code written by Ladar Levison and   
Marco Ferrante.  
Original source code is written in Pascal Language and is available at   
https://github.com/PascalCoin/PascalCoin  
  
  
## HOW TO COMPILE:  
  
See instructions at GitHub Wiki: https://github.com/PascalCoin/PascalCoin/wiki
  
  
Enjoy Pascal Coin!
  
## Donations  
  
If you like it, consider a donation using BitCoin:
16K3HCZRhFUtM8GdWRcfKeaa6KsuyxZaYk

Also, consider a donation at PascalCoin development account: "0-10"

## History:  

### Build 1.5.4.0 - 2017-03-14

- Added Network Timestamp Adjustment (NAT) to calc valid timestamps
  - Minimum 4 active connections to calc median used for NAT, otherwise use local timestamp
  - Based on IP's (to prevent a malicious IP timejacking, each IP is only used once)
  - Removed IP's and recalculated after disconnecting (to prevent malicious node connecting/disconnecting for timejacking)
- New blocks will not be accepted if using future timestamp greater than NAT timestamp + 15 seconds (also, mantaining current protocol rule >= lastBlock.timestamp)
- Network protocol fixed to 5-5 (Nodes with version prior to 1.5 will not be allowed)
- Added protection for non included operations on a block, to prevent continuous sending. Only will resend operations once.
- Bug #27 fixed: Invalid timestamp on FPC (https://github.com/PascalCoin/PascalCoin/issues/27)
- JSON-RPC
  - Method "getconnections" added "timediff" to know timestamp diff of node
  - Method "payloaddecrypt" added "unenc_hexpayload" result value with HEXASTRING of unencrypted payload
- Fixed some "Random Memory access violation errors" bugs found caused by multithreading and disconnected nodes

### Build 1.5.3.0 - 2017-03-06

- Fixed issue #23: RPC findoperation fails to find operation by opHash
- Miners best practices: Sending new job with new timestamp every 30 seconds
- Buffering last 10 sent jobs to miners
- Small delay prior to destroy a connection to prevent exception handling
- Minor logs changes


### Build 1.5.2.0 - 2017-03-03

- Added a jobs buffer for miners. This will allow to submit old job solutions (limited buffer). (Fix the "tx" issue)
- Miner jobs will not be sent every time a transaction is received, thet will be buffered and sent every few seconds (Fix the "tx" issue)
- Better network performance, allowing more operations and nodes thanks to buffering before relaying
- Daemon: Allow select on ini file how many connections can handle
- Fixed a locking when deleting connections

### Build 1.5.1.0 - 2017-02-20

- Memory leak fixed on RPC-JSON commands
- Memory leak fixed on node connections
- Improved network speed processing new blocks/operations
- Some minor bugs

### Build 1.5.0.0 - 2017-02-15

- Net protocol upgrade to 4-5
- Introducing "more work" with high priority than "more high". Work is calculated based on target. Higher target (more work) is more important than higher length
- Solved locking/crash bug on high connections (caused by bad thread locking)
- Improved network connection
- Added JSON-RPC port 4003 protection Whitelist (only allowed IP's can use JSON-RPC)


### Build 1.4.3.0 - 2017-02-02

- Adding "maturation" param to "JSON Operation object", return null when operation is not included on a blockchain yet, 0 means that is included in highest block and so on...
- Fixing miner timestamp value to prevent invalid time

### Build 1.4.2.0 - 2017-01-23

- Max JSON-RPC miner connections is now 1000 (before, 10)
- JSON-RPC miner enabled at pascalcoin_daemon (linux daemon)
- Screen messages for daemon
- pascalcoin_daemon.ini file for daemon

### Build 1.4.1.0 - 2017-01-18

- Improved JSON communications with Miner client (Port 4009 by default)
- Deleted adding numeration to JSON miner clients (only sends miner name)
- Minor changes

### Build 1.4.0.0 - 2016-12-30

- JSON-RPC changes:
  - Added method "signsendto" to allow a off-line wallet sign transaction operations without being syncrhonized
  - Added method "signchangekey" to allow a off-line wallet sign change key operations without being syncrhonized
  - Added method "executeoperations" to allow a on-line wallet execute operations signed off-line
  - Added method "operationsinfo" that will decode operations signed off-line
  - Added param "max" at JSON-RPC method "getblocks"
  - Changed param name "deep" for "depht" in "getaccountoperations". Deep will be available for compatibility.
  - Changed result for "changekeys". Now returns a "JSON operation object" array with each change key operation result
  - Changes on "JSON Operation object": May return a "valid" param and "errors" param
- Corrected a memory leak when processing JSON-RPC calls
- Better performance in connection protocol
- Updated protocol available to 1
- Updated net protocol to 3-4
- Important bug corrected

### Build 1.3.0.0 - 2016-11-24

- JSON-RPC modifications:
  - New param "b58_pubkey", can be used instead of "enc_pubkey": b58_pubkey is a Base58 encoded public key with checksum, is the value that Wallet exports/imports public key
  - New JSON object type Public Key: "name","can_use","enc_pubkey","b58_pubkey","ec_nid","x" and "y" for each public key
  - Added params "start" and "max" to "getaccountoperations". By default deep=100, max=100 and start=0, so will return last 100 operations made to an account
  - Added params "start" and "max" to "getblockoperations". By default max=100 and start=0, so will return last 100 operations made to a block
  - Added params "start" and "max" to "getwalletaccounts". By default max=100 and start=0, so will return first 100 accounts of the wallet
  - Added params "start" and "max" to "getwalletpubkeys". By default max=100 and start=0, so will return first 100 public keys of the wallet
    - Will return a Public key JSON object
  - Method "decodepubkey" allows params "enc_pubkey" or "b58_pubkey" (if used together, returns error if not match)
    - Will return a Public key JSON object
  - Method "changekey" allows params "new_enc_pubkey" or "new_b58_pubkey" (if used together, returns error if not match)
  - Method "getwalletaccounts" allows params "enc_pubkey" or "b58_pubkey" (if used together, returns error if not match)
  - Method "getwalletaccountscount" allows params "enc_pubkey" or "b58_pubkey" (if used together, returns error if not match)
  - Method "getwalletcoins" allows params "enc_pubkey" or "b58_pubkey" (if used together, returns error if not match)  
  - Method "payloadencrypt"  allows params "enc_pubkey" or "b58_pubkey" when payload_method="pubkey" (if used together, returns error if not match)  
  - Method "addnewkey" changed. Does not return an HEXASTRING with enc_pubkey, now returns a public key JSON object
  - New methods:
    - Method "changekeys". Similar to "changekey" but for multiple accounts at param "accounts" as a coma separated string (ex: "accounts"="1248,1753,85056"). Returns a JSON object with result information
    - Method "lock" to lock wallet. Returns true if locked, returns false if wallet has no password (an empty string, must put a new password prior to lock)
    - New method "getwalletpubkey". Search for "enc_pubkey" or "b58_pubkey" and returns a Public key JSON object
- Corrected a issue saving last 5 .bank files
- Improved protections for .bank corruption files
- Added "open data folder" button on options form
- Other minor changes

### Build 1.2.0.0 - 2016-11-16

- Account checksum values modified to be more easy and more distributed: Checksum = ((N * 101) MOD 89)+10
- Allow find operations by "ophash"
- Show Operation "ophash" in operation payload decoder
- Added param "enc_pubkey" to "getwalletaccounts" JSON-RPC method to return only accounts from this public key
- Added params "pow" and "sbh" to "nodestatus" JSON-RPC method
- Added method "getwalletaccountscount" returning accounts count of the entire wallet or for a single "enc_pubkey"
- Added method "getwalletcoins" returning coins of the entire wallet or for a signle "enc_pubkey"
- Modified seed nodes distribution to send only checked IP nodes
- Corrected invalid operation block index when showing account operations


### Build 1.1.0.0 - 2016-11-03

- JSON-RPC Server included
- Minor changes


### Build 1.0.9.0 - 2016-10-21

- Corrected a BUG (BUG-101) that causes blocking connections when received more than 100 connections, causing "alone in the world" after a cert period time.
- It's necessary to update because new version will refuse old versions to make network stable.


### Build 1.0.8.0 - 2016-10-20

- Cross compatible
- Can compile with Delphi or Lazarus (Free Pascal)
- New storage system. No more access database
- Network hashrate calculation


### Build 1.0.7.0 - 2016-10-10

- Introducing basic JSON-RPC to allow GPU miners development (Third party).
- See file "HOWTO_DEVELOP_GPU_MINER_FOR_PASCALCOIN.txt"
- No more CPU mining due exists GPU mining


### Build 1.0.6.0 - 2016-10-04

- Memory leaks corrections
- Introducing net protocol 2-3
- Source code modified, next build will be compiled with Lazarus and FPC


### Build 1.0.5.0 - 2016-09-21

- Massive operations, selecting multiple accounts
- Filter accounts by balance
- Correct operations explorer order of operations for each block (descending order)
- Minor changes


### Build 1.0.4.0 - 2016-09-16

- IMPORTANT: Introducing net protocol changes: Must update!
- More and more and more stable
- Prevents "Alone in the world" if everybody is updated ;-)
- Invalid local time detector with corrections
- IP nodes configurator


### Build 1.0.3.0 - 2016-09-08

- Important changes to database 
- Peer cache
- Issues with Connections
- More stable
- Miner key selector
- Invalid local time detector


### Build 1.0.2.0 - 2016-08-31

- Improved hashing speed
- Allow mining without opening external ports
- Choose how many CPU's want to mine
- Show real-time pending operations waiting to be included in the block
- More stable
- Some miner modifications

### Build 1.0.1.0 - 2016-08-12

- Included an option to Import/Export Wallet keys file
- Some miner modifications


### Build 1.0.0.0 - 2016-08-11

- First stable version.
- Created with Genesis block hardcoded
- Published at same time than Genesis block. NO PREMINE