# Solana 钱包开发实战

## 一.前置知识

在开始 Solana 钱包的实际开发之前，先明确以下钱包基础知识，这将帮助开发者更深入地理解钱包设计与架构。

### 1.钱包的类别与密钥管理方式

- 钱包按密钥管理方式不同，可以分为以下几类：

- 交易所钱包（中心化钱包）：密钥存储于交易所云端，用户不直接管理。
  - 存储方案如下：
  - wallet.data 文件（本地存储）
  - KMS + 代码内存储
  - TEE 环境（例如 AWS Nitro + KMS + S3 数据库存储）
  - CloudHSM（硬件安全模块，私钥不离开硬件环境）
- 安全性：CloudHSM > TEE > KMS > wallet.data

- 去中心化钱包（HD钱包）：用户自己管理密钥，钱包私钥或助记词存储在用户的本地设备上，例如：

  - 手机端：TokenPocket、imToken
  - 电脑端：MetaMask、Phantom
  - 主要标准：BIP39（助记词生成）、BIP32（密钥推导）、BIP44（推导路径）

- 半去中心化钱包（MPC钱包）：通过多方计算（MPC）技术将密钥分片存储，各方共同签名。
  - 2 片密钥 (2-1)：其中任意1个密钥即可签名。
  - 3 片密钥 (3-2)：需要2个密钥片进行签名交易才有效。

- MPC托管钱包（大户钱包）：
- 例如 (10-7) 模式：共10个密钥片，至少7片密钥签名有效。
- 适用于大户或托管方，私钥片由多方节点共同管理。

- 多签钱包：

  - Ethereum、Solana 可通过合约实现。
  - Bitcoin 通过部分签名（PSBT）机制实现。
 

### 2.HD钱包相关知识

- BIP39：
  - 提供助记词协议，维护2048个英文单词，支持多语言。
  - 助记词长度：12、15、18、21、24个单词。
  - 从助记词生成种子用于派生密钥。
- BIP32：从助记词生成 masterKey（主密钥），再根据路径生成子密钥（childKey）以及公钥、地址。
- BIP44：定义了HD钱包的标准推导路径：

```
m / purpose' / coin_type' / account' / change / address_index
```

参考文章：https://github.com/the-web3/blockchain-wallet/tree/master/biphd

## 二.Solana钱包开发实战指南

下面将从开发者角度，全面介绍如何调研并实际开发支持 Solana 的钱包。

### 1.调研链的重要方面

作为钱包开发者，需要关注以下几个维度：

  - 离线地址生成方式
  - 离线签名机制
  - 交易记录、账户余额获取方式
  - 交易确认机制（确认位、Finalized）
  - Staking 质押机制
  - 链基本信息（共识算法、性能、代币精度等）


### 2.Solana 区块链全面解析
Solana 是一种高性能单链区块链，专注于速度和低成本的区块链平台，适用于 DeFi、NFT、高频交易等应用场景。

基本信息调研
- 共识机制：
  - 采用 POH（Proof of History）+ DPOS（权益证明）+ BFT 混合机制。
  - Why：POH 为链上的事件提供可信的时间顺序，DPOS 提高安全性和经济激励，BFT 提升网络抗攻击能力。

- 性能与出块频率：
  - 每秒约 3 个区块，每个区块约 0.4 秒，整体约 1秒出块。

- 代币精度：
  - 精度为 9，即最小单位 1 SOL = 10^9 Lamports。

- 签名算法：EdDSA（Ed25519 算法）。
- 地址编码格式：base58。
- 账户模型：Solana 使用账户模型（Account-based），与以太坊类似，而非比特币的UTXO 模型。


### 3. Solana 网络环境与节点服务

一般公司节点由专业团队搭建，调研人员只需要写清楚需要什么机器配置，怎么搭建即可

#### 3.1.开放节点

- 主网(Mainnet-beta)：
  - 生产环境，正式交易需真实 SOL。
  - RPC: https://api.mainnet-beta.solana.com

- 开发网(Devnet)：
  - 免费提供 SOL 空投用于开发测试。
  - RPC: https://api.devnet.solana.com

- 测试网(Testnet)：
  - 主要用于验证器和压力测试，可能不稳定。
  - RPC: https://api.testnet.solana.com

- 第三方节点（备用方案）：GetBlock Solana 节点等


### 4. Solana 核心功能解析与实现方法

- 离线地址与签名
  - 生成密钥对（Ed25519）。
  - 地址由公钥经 base58 编码获得。
  - 签名通过本地的 EdDSA 算法实现，无需联网。

- 交易记录与余额查询：通过 RPC 接口调用：
  - 查询交易记录：getTransaction
  - 查询余额：getBalance

- 确认位与最终确认（Finalized）：Solana 交易状态有多级确认，Finalized 状态表示完全确定无法更改。
- 相关RPC方法：getSignatureStatuses

- Staking：Solana支持 PoS Staking机制，用户可将 SOL 委托给验证节点以获得收益，相关RPC方法：getStakeActivation

- NFT与代币（SPL-Token）： 代币与 NFT 使用统一标准 SPL-Token，创建、转账、管理使用 Solana 的 SPL-Token Program。
  - RPC 接口：获取 Token 信息：getTokenAccountsByOwner

- 手续费机制（CU机制）Solana使用“Compute Units”（计算单元）计费， 手续费由交易复杂性决定，CU 越多，费用越高。


## 三. 离线地址代码

```javascript
export function createSolAddress (seedHex: string, addressIndex: string) {
  const { key } = derivePath("m/44'/501'/1'/" + addressIndex + "'", seedHex);
  const publicKey = getPublicKey(new Uint8Array(key), false).toString('hex');
  const buffer = Buffer.from(getPublicKey(new Uint8Array(key), false).toString('hex'), 'hex');
  const address = bs58.encode(buffer);
  const hdWallet = {
    privateKey: key.toString('hex') + publicKey,
    publicKey,
    address
  };
  return JSON.stringify(hdWallet);
}
```

Solana 的 BIP 44 编号是 501；并且将 BIP44 协议中的是否找零项目去掉了。



## 四.离线签名代码

```javascript
export async function signSolTransaction (params) {
  const { from, amount, to, mintAddress, nonce, decimal, privateKey } = params;
  const fromAccount = Keypair.fromSecretKey(new Uint8Array(Buffer.from(privateKey, 'hex')), { skipValidation: true });
  const calcAmount = new BigNumber(amount).times(new BigNumber(10).pow(decimal)).toString();
  if (calcAmount.indexOf('.') !== -1) throw new Error('decimal 无效');
  const tx = new Transaction();
  const toPubkey = new PublicKey(to);
  const fromPubkey = new PublicKey(from);
  tx.recentBlockhash = nonce;
  if (mintAddress) {
    const mint = new PublicKey(mintAddress);
    const fromTokenAccount = await SPLToken.Token.getAssociatedTokenAddress(
      SPLToken.ASSOCIATED_TOKEN_PROGRAM_ID,
      SPLToken.TOKEN_PROGRAM_ID,
      mint,
      fromPubkey
    );
    const toTokenAccount = await SPLToken.Token.getAssociatedTokenAddress(
      SPLToken.ASSOCIATED_TOKEN_PROGRAM_ID,
      SPLToken.TOKEN_PROGRAM_ID,
      mint,
      toPubkey
    );
    tx.add(
      SPLToken.Token.createTransferInstruction(
        SPLToken.TOKEN_PROGRAM_ID,
        fromTokenAccount,
        toTokenAccount,
        fromPubkey,
        [fromAccount],
        calcAmount
      )
    );
  } else {
    tx.add(
      SystemProgram.transfer({
        fromPubkey: fromAccount.publicKey,
        toPubkey: new PublicKey(to),
        lamports: calcAmount
      })
    );
  }
  tx.sign(fromAccount);
  return tx.serialize().toString('base64');
}
```

本代码中有一点值得注意的是，solana 交易签名中的 nonce 是使用 recentBlockHash 做为签名的 nonce, 而且这个 recentBlockHash 签名的交易在一定时间内交易发出去有效，过了这个时间之后再发送交易会报 Block Not Found 的错误。

要解决交易 recentBlockHash 失效问题，需要授权一个新的地址做为获取签名的 Nonce 的地址，这样签名的消息才能很长时间内都有效。

代码如下：

```javascript
export function prepareAccount(params){
  const {
    authorAddress, from, recentBlockhash, minBalanceForRentExemption, privs,
  } = params;

  const authorPrivateKey = (privs?.find(ele=>ele.address===authorAddress))?.key;
  if(!authorPrivateKey) throw new Error("authorPrivateKey 为空");
  const nonceAcctPrivateKey = (privs?.find(ele=>ele.address===from))?.key;
  if(!nonceAcctPrivateKey) throw new Error("nonceAcctPrivateKey 为空");

  const author = Keypair.fromSecretKey(new Uint8Array(Buffer.from(authorPrivateKey, "hex")));
  const nonceAccount = Keypair.fromSecretKey(new Uint8Array(Buffer.from(nonceAcctPrivateKey, "hex")));


  let tx = new Transaction();
  tx.add(
      SystemProgram.createAccount({
        fromPubkey: author.publicKey,
        newAccountPubkey: nonceAccount.publicKey,
        lamports: minBalanceForRentExemption,
        space: NONCE_ACCOUNT_LENGTH,
        programId: SystemProgram.programId,
      }),
  
      SystemProgram.nonceInitialize({
        noncePubkey: nonceAccount.publicKey,  
        authorizedPubkey: author.publicKey, 
      })
  );
  tx.recentBlockhash = recentBlockhash;


  tx.sign(author, nonceAccount);
  return tx.serialize().toString("base64");
}
```



## 五.钱包开发相关的 RPC 接口

### 1.获取账户信息

- 接口作用：本接口的作用是判断账户是否可用，如果 value 为 null, 说明不可用，否则可用
- 接口名称：getAccountInfo
- 接口参数：地址
- 请求示范

```bash
curl --location 'https://sly-yolo-dinghy.solana-mainnet.quiknode.pro/2ac2af5b8c2e5e9e74c7906e949f1976314aa996' \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getAccountInfo",
    "params": [
      "4wHd9tf4x4FkQ3JtgsMKyiEofEHSaZH5rYzfFKLvtESD",
      {
        "encoding": "base58"
      }
    ]
  }'
```

- 返回值

```json
{
    "jsonrpc": "2.0",
    "result": {
        "context": {
            "apiVersion": "1.17.34",
            "slot": 268835746
        },
        "value": {
            "data": [
                "",
                "base58"
            ],
            "executable": false,
            "lamports": 289995950,
            "owner": "11111111111111111111111111111111",
            "rentEpoch": 18446744073709551615,
            "space": 0
        }
    },
    "id": 1
}
```

- value 不为 null, 说明该地址能用

### 2.获取 recentBlochHash,  直接签名的话 recentBlochHash 相当于 nonce

- 接口作用：获取最近的区块的 blockHash, 做为交易签名的 Nonce
- 接口名称：getRecentBlockhash
- 接口参数：无
- 请求示范

```bash
curl --location 'https://sly-yolo-dinghy.solana-mainnet.quiknode.pro/2ac2af5b8c2e5e9e74c7906e949f1976314aa996' \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc":"2.0",
    "id":1, 
    "method":"getRecentBlockhash"
}'
```

- 返回值

```json
{
    "jsonrpc": "2.0",
    "result": {
        "context": {
            "apiVersion": "1.17.34",
            "slot": 268809612
        },
        "value": {
            "blockhash": "5k5Hh3gfRX4hJv9dqxfdMa6wSFkkZ631sf71kPqtCFv7",
            "feeCalculator": {
                "lamportsPerSignature": 5000
            }
        }
    },
    "id": 1
}
```

- blockhash 为要使用的值

### 3. 获取准备 nonce 账户的 Minimum Balance For Rent 数据

- 接口作用：获取 rent 账户的最小 Rent
- 接口名称：getMinimumBalanceForRentExemption
- 接口参数：账户的数据长度
- 请求示范

```bash
curl --location 'https://sly-yolo-dinghy.solana-mainnet.quiknode.pro/2ac2af5b8c2e5e9e74c7906e949f1976314aa996' \
--header 'Content-Type: application/json' \
--data ' {
    "jsonrpc": "2.0", "id": 1,
    "method": "getMinimumBalanceForRentExemption",
    "params": [50]
  }'
```

- 返回值

```json
{
    "jsonrpc": "2.0",
    "result": 1238880,
    "id": 1
}
```

- result 的结果为 *prepareAccount*  的 minBalanceForRentExemption 值

### 4. 获取最新块高 （Slot）

- 接口作用：获取最新的 slot
- 接口名称：getSlot
- 接口参数：无
- 请求示范

```bash
curl --location 'https://sly-yolo-dinghy.solana-mainnet.quiknode.pro/2ac2af5b8c2e5e9e74c7906e949f1976314aa996' \
--header 'Content-Type: application/json' \
--data '{"jsonrpc":"2.0","id":1, "method":"getSlot"}'
```

- 返回值

```json
{
    "jsonrpc": "2.0",
    "result": 268840179,
    "id": 1
}
```

- 返回值为最新的块高（Slot）

### 5. 根据块高获取交易

- 接口作用：根据区块号获取里面的交易
- 接口名称：getConfirmedBlock
- 接口参数：区块高度和编码方式
- 请求示范

```bash
curl --location 'https://api.devnet.solana.com' \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0", "id": 1,
    "method": "getConfirmedBlock",
    "params": [
        268992938, 
         {
          "encoding":"base64",
           "maxSupportedTransactionVersion": 0
      }
    ]
  }'
```

- 返回值

```json
{
  "jsonrpc": "2.0",
  "result": {
    "blockTime": null,
    "blockhash": "3Eq21vXNB5s86c62bVuUfTeaMif1N2kUqRPBmGRJhyTA",
    "parentSlot": 429,
    "previousBlockhash": "mfcyqEXB3DnHXki6KjjmZck6YjmZLvpAByy2fj4nh6B",
    "rewards": [],
    "transactions": [
      {
        "meta": {
          "err": null,
          "fee": 5000,
          "innerInstructions": [],
          "logMessages": [],
          "postBalances": [499998932500, 26858640, 1, 1, 1],
          "postTokenBalances": [],
          "preBalances": [499998937500, 26858640, 1, 1, 1],
          "preTokenBalances": [],
          "status": {
            "Ok": null
          }
        },
        "transaction": [
          "AVj7dxHlQ9IrvdYVIjuiRFs1jLaDMHixgrv+qtHBwz51L4/ImLZhszwiyEJDIp7xeBSpm/TX5B7mYzxa+fPOMw0BAAMFJMJVqLw+hJYheizSoYlLm53KzgT82cDVmazarqQKG2GQsLgiqktA+a+FDR4/7xnDX7rsusMwryYVUdixfz1B1Qan1RcZLwqvxvJl4/t3zHragsUp0L47E24tAFUgAAAABqfVFxjHdMkoVmOYaR1etoteuKObS21cc1VbIQAAAAAHYUgdNXR0u3xNdiTr072z2DVec9EQQ/wNo1OAAAAAAAtxOUhPBp2WSjUNJEgfvy70BbxI00fZyEPvFHNfxrtEAQQEAQIDADUCAAAAAQAAAAAAAACtAQAAAAAAAAdUE18R96XTJCe+YfRfUp6WP+YKCy/72ucOL8AoBFSpAA==",
          "base64"
        ]
      }
    ]
  },
  "id": 1
}
```

或者内部包含

```json
{
   "parsed":{
      "info":{
         "amount":"12500",
         "authority":"CyZuD7RPDcrqCGbNvLCyqk6Py9cEZTKmNKujfPi3ynDd",
         "destination":"Ezts8ufHLpKmJsvXzPmguvvdtC18G8aQngQ9EyUAJrHf",
         "source":"FLffM77tZpRP7eRqQGfH7UA1j1j31csZ4f9CwQ4qbVjp"
      },
      "type":"transfer"
   },
   "program":"spl-token",
   "programId":"TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
   "stackHeight":2
}
```

- source 是转出地址
- destination 转入地址
- amount 是转账金额
- type：转账类型

### 6. 根据交易 Hash 获取交易详情

- 接口作用：根据交易 Hash 获取交易详情
- 接口名称：getConfirmedTransaction
- 接口参数：区块高度和编码方式
- 请求示范

```bash
curl --location 'https://sly-yolo-dinghy.solana-mainnet.quiknode.pro/2ac2af5b8c2e5e9e74c7906e949f1976314aa996' \
--header 'Content-Type: application/json' \
--data ' {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getConfirmedTransaction",
    "params": [
      "5X6y7aVzC93nfHbPbcy1yU8jHeFj996ZrVzMfNFYqC59jJrVMPMj3zzCtfwnoJB8H7PcgZRhyMbz1znVt8CSoT35",
      {
          "encoding":"jsonParsed",
           "maxSupportedTransactionVersion": 0
      }
    ]
  }'
```

- 返回值

```json
{
   "jsonrpc":"2.0",
   "result":{
      "blockTime":1717073604,
      "transaction":{
         "message":{
            "accountKeys":[
               
            ],
            "instructions":[
               {
                  "parsed":{
                     "info":{
                        "amount":"5000000000",
                        "authority":"7JnucyofTX4Zk74jJ18HRhfKoBc8GgPM6ofQZ1EQLSpZ",
                        "destination":"EgScKCKKWX1d7yEp7SBpErdvYsBXzYYgxtMWiSdbZL9S",
                        "source":"FSkKxc5WBa7g6obBmXMavNmpuNPs25nukJz32DyHXuTM"
                     },
                     "type":"transfer"
                  },
                  "program":"spl-token",
                  "programId":"TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
                  "stackHeight":null
               },
               {
                  "accounts":[
                     
                  ],
                  "data":"3MuSQQNShkST",
                  "programId":"ComputeBudget111111111111111111111111111111",
                  "stackHeight":null
               },
               {
                  "accounts":[
                     
                  ],
                  "data":"FtDxo9",
                  "programId":"ComputeBudget111111111111111111111111111111",
                  "stackHeight":null
               }
            ],
            "recentBlockhash":"HMzYzdmwMez5ueZA9yrWHT6V5ZT4VtD3CC4hsaN7a9m4"
         },
         "signatures":[
            "5X6y7aVzC93nfHbPbcy1yU8jHeFj996ZrVzMfNFYqC59jJrVMPMj3zzCtfwnoJB8H7PcgZRhyMbz1znVt8CSoT35"
         ]
      }
   },
   "id":1
}
```

- source 是转出地址
- destination 转入地址
- amount 是转账金额
- type：转账类型

### 7.发送交易到区块链网络

- 接口作用：发送交易到区块链网络
- 接口名称：sendTransaction
- 接口参数：区块高度和编码方式
- 请求示范

```bash
curl --location 'https://sly-yolo-dinghy.solana-mainnet.quiknode.pro/2ac2af5b8c2e5e9e74c7906e949f1976314aa996' \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "sendTransaction",
    "params": [
      "ASG4jVQEsZhgMGnkXIYEvyhvGMYBfwL8lFd40mPz4AjpeWqZlVM122Vx8iCZIUNmbGdqDcdplm0xMUGri4WiCAIBAAEDOns4dLpGe+a4HqNh49dFOvi4HIiu3SS1Ax/doLxxrTJa8yfsHKLzR/zaYPv8xS5VGKfAp4PkO8pQN4Jn396IRgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKy6/vKD5x9Dmbz1bBJ85fYZK94CfRhjYwMBlQyb4mz8BAgIAAQwCAAAAQEIPAAAAAAA=",
      {
        "encoding":"base64"
      }
     
    ]
  }'
```

- 返回值

```json
{
    "jsonrpc": "2.0",
    "result": "g6yMJUd16jAcPiQfi7QK1M5tN7yheeKAC7NbUh3BQCSfav3pgv74ovCSGuhgApzma1s6ew8WEmX1Bzfk6sCfg9P",
    "id": 1
}
```

- 成功返回交易 Hash,  失败返回各种错误


## 六.HD 钱包开发

## **1.离线地生成和离线签名**

参考上面的代码

## **2.和链上交互的接口**

- 获取账户余额
- 根据地址获取交易记录
- 获取预估手续费

对应代码库：https://github.com/the-web3/wallet-chain-node

## 八. 总结

HD 钱包和交易所钱包不同之处有以下几点

## **1.密钥管理方式不同**

- HD 钱包私钥在本地设备，私钥用户自己控制
- 交易所钱包中心化服务器(CloadHSM, TEE 等)，私钥项目方控制

## **2.资金存在方式不同**

- HD 资金在用户钱包地址
- 交易所钱包资金在交易所热钱包或者冷钱包里面, 用户提现的时候从交易所热钱包提取

## **3.业务逻辑不一致**

- 中心化钱包：实时不断扫链更新交易数据和状态
- HD 钱包：根据用户的操作通过请求接口实现业务逻辑


# 九. 附录

- RPC 文档：https://solana.com/docs/rpc
- github: https://github.com/solana-labs
- 浏览器 1：https://explorer.solana.com/
- 浏览器 2: https://solscan.io/
- 钱包相关的资料：https://solana.com/developers/cookbook
- solana web3js: https://github.com/solana-labs/solana-web3.js
    
