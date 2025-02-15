作为一名区块链钱包开发，经常需要接触不同的链，这个项目是为了收集接入链之前所做的调研工作的资料，避免以后的重复劳动。

## 研究一条链需要搞清楚的东西有哪些？

### 1. 基础信息
- **RPC Base Url**
  - 通过官方文档，第三方节点提供获取或者公链官网获取
  - Ethereum 系列的可以访问 Chainlist

- **账户模型和 UTXO 模型**
  - 从区块浏览器数据分析
  - 调用 RPC getTransaction(类 getTransaction) 接口
    - 如果数据包含 vin(或者 Input) 和 vout(或者 output) → UTXO 模型（如 BTC、LTC、DOGE，SiaCoin）
    - 如果数据包含 from、to、value(类 from(如 source), 类 to(如 destination), 类 value(如 amount, lamports)) → 账户模型（如 ETH、BSC、SOL）

### 2. 加密，签名与地址编码
- **签名算法**：使用 ECDSA（如 BTC, ETH）、EdDSA（如 Solana）、Schnorr（如 Taproot, BCH）和 RSA。
- **是否有特定签名方案**（如 Algorand 的多重签名、Cosmos 的 bech32 地址编码）
- **地址格式与生成方式**
  - 支持的地址格式（如 ETH 0x 开头，BTC bc1，Solana base58）
  - 地址编码流程（如 Bech32, Base58, Checksummed）
  - 离线地址生成方式
    - 是否支持 HD Wallet（助记词 → 派生路径），如 EOS 不支持 HD wallet 派生
    - 是否支持离线私钥生成

### 3. 代币 & NFT 兼容性
- **代币精度**：代币最小单位，是否支持小数转换（精度计算方式）
- **是否支持代币（Token）和 NFT**
  - 代币标准：
    - ERC-20（以太坊）
    - BEP-20（BSC）
    - SPL（Solana）
    - CW-20（Cosmos）
  - NFT 标准：
    - ERC-721, ERC-1155（ETH 及兼容链）
    - SPL NFT（Solana）
    - CosmosWasm NFT（Cosmos）

### 4. 共识机制与交易确认
- **共识机制**
  - PoW、PoS、DPoS、PBFT、PoH 等
- **确认机制**
  - 需要多少个确认数？（BTC 6 确认，ETH 12 秒 1 块）
  - 是否支持质押（PoS 链通常支持）
- **质押（Staking）方式**
  - 质押是否需要锁定？（如 ETH 2.0 需要锁定，Cosmos 允许流动性质押）
  - 是否支持委托质押（如 Cosmos, Polkadot）

### 5. 交易细节
- **是否支持 Tag/Memo**：部分公链（如 XRP、Cosmos）交易时需要 Memo/Tag 作为交易标识
- **是否为多链结构**
  - 单链（如 BTC, ETH）
  - 多链生态（如 Cosmos, Polkadot, Avalanche Subnets）
  - 是否支持跨链交易（如 Cosmos IBC, Polkadot XCM）
- **钱包的跨链能力**
  - 是否支持跨链转账
  - 是否支持桥接（Bridge）

### 6. 开发离线签名 SDK
- **官方 SDK 是否支持当前开发语言**
  - 幸运：找到了适配 SDK（如 ethers.js、web3.py、solana-web3.js）
  - 不幸：没有适配 SDK，需要研究公链钱包模块，自行实现 SDK

### 7. 区块扫描
- **中心钱包扫链（区块同步）所需的 RPC 接口与 HD 钱包所需要的接口**

### 8. 总结
一个钱包要适配一条新的链，需要搞清楚：
- ✅ 基本信息（账户模型、RPC）
- ✅ 加密方式（签名算法、地址格式）
- ✅ 代币 & NFT 支持
- ✅ 共识机制（交易确认、质押）
- ✅ 交易结构（是否有 Memo、跨链）
- ✅ 开发支持（SDK、离线签名）
- ✅ 扫链 RPC 接口（查询余额、交易、监听区块）
