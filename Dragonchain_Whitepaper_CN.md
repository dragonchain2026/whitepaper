# Dragonchain 白皮书

## 极速交易、后量子安全的去中心化点对点电子现金系统

**版本**: 1.1  
**日期**: 2026年9月  
**官网**: https://www.dragonchain.cc  
**GitHub**: https://github.com/dragonchain2026  

---

## 摘要

Dragonchain 是一个基于工作量证明（PoW）的去中心化点对点电子现金系统，融合了 Sugarchain 的极速交易能力和 Tidecoin 的后量子安全特性。Dragonchain 采用 5 秒出块时间实现全球最快的 PoW 交易确认速度，使用 Falcon-512 后量子签名算法抵御未来量子计算机的攻击，并通过 Yespower CPU 友好挖矿算法实现真正的"一 CPU 一票"去中心化。Dragonchain 从未进行 ICO、预挖或私募，属于完全公平启动的社区驱动项目。

---

## 1. 引言

比特币 [1] 作为第一个成功的去中心化数字货币，证明了基于密码学证明的电子支付系统的可行性。然而，随着区块链技术的发展，比特币面临两个核心挑战：

1. **交易确认速度慢**：比特币 10 分钟的出块时间无法满足日常支付需求
2. **量子计算威胁**：比特币使用的 ECDSA 签名算法在量子计算机面前不具备安全性 [2]

Dragonchain 针对这两个问题提出了系统性解决方案：
- 5 秒出块时间，比比特币快 120 倍
- Falcon-512 后量子签名算法，抵御量子计算攻击

Dragonchain 严格遵循中本聪提出的"一 CPU 一票"理念，通过 Yespower 算法确保挖矿权力分散在广泛的 CPU 设备上，而非集中在 GPU 矿场或 ASIC 矿机手中。

---

## 2. 核心参数

| 参数 | 值 | 说明 |
|------|------|------|
| 出块时间 | 5 秒 | 全球最快 PoW 区块链之一 |
| 初始区块奖励 | 42.94967296 DRAGON | 仅创世块 |
| 前期区块奖励 | 3.32988658 DRAGON | 前半年（0-3,153,600 块） |
| 常规区块奖励 | 0.20807311 DRAGON | 半年后，每 4 年减半 |
| 减半间隔 | 25,228,800 块（约 4 年） | |
| 总发行量 | 21,000,000 DRAGON | 约比特币的总量 |
| 创世时间 | 2026-05-04 21:20:00 CST | Unix timestamp: 1777900800 |
| PoW 算法 | Yespower 1.0.1 | CPU 友好型 |
| 签名算法 | Falcon-512 | 格基后量子密码学 |
| 地址格式 | Bech32m (dragon1...) | 原生隔离见证 |
| 难度调整 | SugarShield-N510 | 510 块移动窗口（约 42.5 分钟） |
| P2P 端口 | 29000 | |
| RPC 端口 | 29001 | |
| 预挖/IEO/ICO | 无 | 完全公平启动 |

---

## 3. 后量子安全

### 3.1 量子计算威胁

量子计算机对传统公钥密码学构成根本性威胁。Shor 算法可以在多项式时间内破解 RSA 和椭圆曲线离散对数问题（ECDLP），这意味着比特币使用的 ECDSA（secp256k1）签名算法在足够强大的量子计算机面前将完全失效。

2020 年 12 月，光子量子计算机展示了相对于传统超级计算机的"量子优越性"[3]。尽管目前量子计算机的规模尚不足以破解比特币签名，但加密货币的设计周期应以数十年为单位——当量子计算机成熟时，未做防护的区块链将面临灾难性安全危机。

### 3.2 Falcon-512 签名算法

Dragonchain 采用 **Falcon-512**（Fast-Fourier Lattice-based Compact Signatures over NTRU）作为数字签名算法。Falcon 是由 Gentry, Peikert 和 Vaikuntanathan 于 2008 年提出的格基签名框架的实现，已被 NIST 后量子密码学标准化进程选为最终入围算法 [4]。

**核心原理**：

Falcon 的安全性基于 NTRU 格上的短整数解问题（SIS）。其工作流程如下：

1. **密钥生成**：生成一个 q-ary 格的短基（私钥）和长基（公钥）
2. **签名**：
   a. 生成随机盐值 salt
   b. 计算目标 c = H(msg||salt)，其中 H 是将输入映射到格上点的哈希函数
   c. 利用短基计算靠近目标 c 的格点 v
   d. 输出 (salt, s)，其中 s = c − v
3. **验证**：验证者接受签名当且仅当：
   a. 向量 s 足够短
   b. H(msg||salt) − s 是公钥生成格上的有效点

**Falcon-512 特性**：

| 特性 | 数值 | 说明 |
|------|------|------|
| 签名大小 | 690 字节 | 远小于同安全性的 RSA 签名 |
| 公钥大小 | 897 字节 | 紧凑，适合嵌入式设备 |
| 经典安全性 | ~128 位 | 等效于 RSA-2048 |
| 签名速度 | 数千次/秒 | 普通计算机上 |
| 验证速度 | 比签名快 5-10 倍 | |
| RAM 使用 | <30 KB | 适合内存受限设备 |

**抗量子特性**：Falcon 基于格密码学，目前没有已知的高效求解算法——即使在量子计算机的辅助下也是如此。这与基于大数分解或离散对数的传统密码学形成鲜明对比。

### 3.3 Dragonchain 中的 Falcon-512 实现

Dragonchain 将 Falcon-512 完全集成到交易签名和验证流程中：

- **密钥生成**：`PQCLEAN_FALCON512_CLEAN_crypto_sign_keypair()` 生成公钥/私钥对
- **交易签名**：使用 `PQCLEAN_FALCON512_CLEAN_crypto_sign_signature()` 对交易哈希进行签名
- **签名验证**：使用 `PQCLEAN_FALCON512_CLEAN_crypto_sign_verify()` 验证签名有效性

```cpp
// 签名大小: 690 字节 (src/key.h)
#define PQCLEAN_FALCON512_CLEAN_CRYPTO_BYTES_ 690

// 签名验证 (src/pubkey.cpp)
int r = PQCLEAN_FALCON512_CLEAN_crypto_sign_verify(
    vchSig.data(), vchSig.size(),
    hash.begin(), 32,
    pch + 1
);
```

由于 Falcon-512 使用 1281 字节的非标准密钥结构，它**不兼容 BIP32 分层确定性（HD）钱包派生**。Dragonchain 将此视为对安全性的合理权衡：后量子安全优先于 HD 便利性。

---

## 4. 工作量证明与共识

### 4.1 Yespower PoW 算法

Dragonchain 采用 **Yespower 1.0.1** 作为工作量证明算法 [5]。Yespower 基于 scrypt，具有以下特性：

- **CPU 友好**：在当前 CPU 上计算效率较高
- **GPU 不友好**：在当前 GPU 上的效率较低
- **FPGA/ASIC 中性**：不特别适合任何专用硬件

这种设计确保了挖矿权力分散在广泛的通用计算设备上，真正实现了"一 CPU 一票"的去中心化理念。

**PoW 验证流程**：

```
H(Nonce) = Hash(Hash(nVersion, hashPrevBlock, hashMerkleRoot, nTime, nBits, Nonce))

若 H(Nonce) < PoW_Target:
    工作量证明有效 → 广播新区块 → 获得区块奖励
```

**最低难度设置**（powLimit）：
```
0x003fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
```
较低的 powLimit 使得：
1. 5 秒极速出块成为可能
2. 低配置 CPU 也能参与挖矿

### 4.2 难度调整算法：SugarShield-N510

Dragonchain 采用 **SugarShield-N510** 难度调整算法（DAA），基于 Zcash 对 Digishield 的改进，使用 **510 个区块**（约 42.5 分钟）的移动平均窗口。

**核心公式**：

```
target_{h+1} = avg_N_targets * (1 + (t_N − t₀) / (4 × T × N) − 1/4)
```

其中 T = 5（目标出块时间），N = 510（窗口大小）

**参数配置**：

| 参数 | 值 | 说明 |
|------|------|------|
| 目标出块时间 | 5 秒 | |
| 难度周期 | 510 块（42.5 分钟） | |
| 最大下调 | 32% | 每周期 |
| 最大上调 | 16% | 每周期 |
| 难度调整启动块 | 第 511 块 | |
| 难度稳定开始块 | 第 512 块 | |

**抗攻击特性**：
- 使用未来时间限制（FTL）防范时间戳攻击：区块时间与当前主链头部偏差超过 60 秒将被拒绝
- 节点时间与标准网络时间偏差不得超过 70 秒，否则将被网络封禁

### 4.3 最长链共识

Dragonchain 遵循 Nakamoto 共识的最长链规则：节点始终将累积工作量最大的链视为正确链，并在其基础上继续工作。当两个节点同时广播不同版本的下一个区块时，节点处理第一个收到的区块但保存另一个分支。下一次出块时，工作量最大的链将成为最长分支，网络上大多数节点将切换到该分支。

---

## 5. 经济模型

### 5.1 供给模型

Dragonchain 总发行量严格限定为 **21,000,000 DRAGON**，与经济意义显著的比特币总量一致。

**区块奖励结构**：

```
阶段 1（创世块, 高度 0）:
  奖励 = 42.94967296 DRAGON（= 2³² / 10⁸）

阶段 2（前期, 高度 1 ~ 3,153,600，约半年）:
  奖励 = 3.32988658 DRAGON/块
  总量 = 10,500,000 DRAGON

阶段 3（常规, 高度 3,153,601 起）:
  奖励 = 0.20807311 DRAGON/块
  每 25,228,800 块（约 4 年）减半
  共 64 次减半 → 最终奖励归零
```

**总供给计算**：
```
总供给 = 42.94967296 + 10,500,000 + Σ(n=0→63) 25,228,800 × 0.20807311 / 2^n
       = 42.94967296 + 10,500,000 + 10,499,957.05032704
       ≈ 21,000,000 DRAGON
```

### 5.2 发行曲线

Dragonchain 采用**前置供给**策略：
- **前半年**释放总量的一半（10,500,000 DRAGON），快速建立流通
- **半年后**进入标准的 4 年减半周期，逐步释放剩余部分
- **约 256 年后**，所有 DRAGON 被完全挖掘

这一设计的优势：
1. 早期矿工获得充足的激励参与网络建设
2. 快速建立的流动性有助于交易所和支付应用集成
3. 长期减半机制持续保持稀缺性

### 5.3 与比特币经济模型对比

| 参数 | Bitcoin | Dragonchain |
|------|---------|-------------|
| 总供应量 | 21,000,000 BTC | 21,000,000 DRAGON |
| 出块时间 | 10 分钟 | 5 秒 |
| 初始奖励 | 50 BTC | 3.33 DRAGON |
| 减半周期 | 210,000 块（~4 年） | 25,228,800 块（~4 年） |
| 减半次数 | 33 次 | 64 次 |
| 全部挖完 | ~2140 年 | ~2282 年 |
| 最小单位 | 1 聪 (0.00000001) | 1 聪 (0.00000001) |

---

## 6. 地址与交易

### 6.1 Bech32m 地址格式

Dragonchain 默认使用 **Bech32m 编码**（BIP350）的原生隔离见证（Native SegWit）地址，格式为：

```
dragon1qvazfa2ssu47wes89390sl0jz6g05h0267u8g
```

**地址解析**：
- `dragon`：人类可读前缀（HRP），标识 Dragonchain 网络
- `1`：分隔符
- `q`：见证版本（v0）
- `vazfa2ssu47wes89390sl0jz6g05h0`：见证程序（Base32 编码）
- `267u8g`：校验和（6 位 BCH 码）

**与传统地址的对比**：

| 特性 | Legacy (Base58) | Bech32m (Native SegWit) |
|------|-----------------|------------------------|
| 示例地址 | S... | dragon1... |
| 大小写 | 大小写敏感 | 不区分大小写 |
| 二维码效率 | 低 | 高 |
| 差错检测 | 一般 | 极强（检错 + 纠错） |
| 交易手续费 | 较高 | 较低 |

传统地址前缀：public key 以 `S` 开头，script hash 以 `s` 开头。

### 6.2 原生隔离见证（Native SegWit）

Dragonchain 自创始区块即**默认启用隔离见证**（BIP141/BIP143/BIP147），所有交易默认使用隔离见证格式。相比传统交易格式：

- 交易数据更紧凑，手续费更低
- 消除交易延展性（transaction malleability）
- 为二层网络扩展（如闪电网络）提供基础

### 6.3 交易签名

每笔 Dragonchain 交易均使用 **Falcon-512** 算法签名。签名大小为 690 字节，公钥为 897 字节。与 Bitcoin 的 ECDSA（~71 字节签名，33 字节公钥）相比体积更大，但换来了对量子计算攻击的长期安全性。

---

## 7. 网络架构

### 7.1 P2P 网络

Dragonchain 采用全分布式点对点网络拓扑，不需要超级节点。每个全节点运行相同的配置和代码，通过广播机制构建自组织网络。

**节点运行流程**：
1. 新交易广播到所有节点
2. 每个节点收集新交易并打包为区块
3. 每个节点执行 PoW 计算，若找到有效证明则广播到全网
4. 节点收集工作量证明并生成有效的区块
5. 仅当 PoW 和所有交易有效时，节点接受该区块
6. 节点通过创建包含前一区块哈希的新区块来表示对当前链的接受

### 7.2 磁盘与带宽需求

| 指标 | 当前值 | 日增长 |
|------|--------|--------|
| 区块数 | 635,000+ | ~21,000 块/天 |
| 区块链大小 | ~234 MB | ~8 MB/天 |
| 年增长 | — | ~2.9 GB/年 |

---

## 8. 安全性分析

### 8.1 量子安全评估

Dragonchain 的抗量子安全等级如下：

| 攻击场景 | ECDSA (Bitcoin) | Falcon-512 (Dragonchain) |
|----------|:---:|:---:|
| 经典计算机攻击 | 安全（128 位） | 安全（~128 位等效） |
| Shor 算法（量子） | **完全破解** | **安全**（格问题无已知高效量子解法） |
| Grover 算法（量子） | 安全性减半（64 位） | N/A（格问题不适用） |
| 侧信道攻击 | 取决于实现 | 取决于实现（同等水平） |

---

## 9. 路线图

| 阶段 | 时间 | 目标 |
|------|------|------|
| 第一阶段 | 2026 Q2 | 主网上线、全节点发布、挖矿启动 ✅ |
| 第二阶段 | 2026 Q2 | 区块浏览器上线、官方网站部署 ✅ |
| 第三阶段 | 2026 Q3-2027+ | Android 手机钱包（SPV）、智能合约等 |

---

## 10. 结论

Dragonchain 融合了 Sugarchain 的极速交易能力和 Tidecoin 的后量子安全特性，构建了一个兼具速度与安全性的下一代去中心化电子现金系统。

通过 5 秒出块时间、Falcon-512 后量子签名、Yespower CPU 友好挖矿和 SugarShield-N510 难度调整算法，Dragonchain 在交易速度、量子安全、去中心化程度和经济模型可持续性方面实现了系统性创新。

作为完全由社区驱动、无预挖、无 ICO 的公平启动项目，Dragonchain 致力于成为全球用户可信任的去中心化支付基础设施。

---

## 参考文献

[1] S. Nakamoto. Bitcoin: A Peer-to-Peer Electronic Cash System, 2008.  
[2] P. W. Shor. Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer. SIAM Journal on Computing, 1997.  
[3] IEEE Spectrum. Photonic Quantum Computer Displays 'Supremacy' Over Supercomputers, Dec 2020.  
[4] Falcon: Fast-Fourier Lattice-based Compact Signatures over NTRU. https://falcon-sign.info/falcon.pdf  
[5] Yespower. https://www.openwall.com/yespower/  
[6] Zenny Kim. Sugarchain: A PoW Blockchain with Fastest Transactions and Halving without Rounding Errors.  
[7] EverettX. Tidecoin: A Post-Quantum Security Peer-to-Peer Crypto Cash, 2020.  

---

> Dragonchain 是一个开源项目，在 MIT 许可下发布。任何人均可自由使用、修改和分发本项目代码。
