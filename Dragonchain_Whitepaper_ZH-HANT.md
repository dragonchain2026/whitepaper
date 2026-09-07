# Dragonchain 白皮書

## 極速交易、後量子安全的去中心化點對點電子現金系統

**版本**: 1.1
**日期**: 2026年9月
**官網**: https://www.dragonchain.cc
**GitHub**: https://github.com/dragonchain2026

---

## 摘要

Dragonchain 是一個基於工作量證明（PoW）的去中心化點對點電子現金系統，融合了 Sugarchain 的極速交易能力和 Tidecoin 的後量子安全特性。Dragonchain 採用 5 秒出塊時間實現全球最快的 PoW 交易確認速度，使用 Falcon-512 後量子簽名演算法抵禦未來量子電腦的攻擊，並透過 Yespower CPU 友好挖礦演算法實現真正的「一 CPU 一票」去中心化。Dragonchain 從未進行 ICO、預挖或私募，屬於完全公平啟動的社群驅動專案。

---

## 1. 引言

比特幣 [1] 作為第一個成功的去中心化數位貨幣，證明了基於密碼學證明的電子支付系統的可行性。然而，隨著區塊鏈技術的發展，比特幣面臨兩個核心挑戰：

1. **交易確認速度慢**：比特幣 10 分鐘的出塊時間無法滿足日常支付需求
2. **量子計算威脅**：比特幣使用的 ECDSA 簽名演算法在量子電腦面前不具備安全性 [2]

Dragonchain 針對這兩個問題提出了系統性解決方案：
- 5 秒出塊時間，比比特幣快 120 倍
- Falcon-512 後量子簽名演算法，抵禦量子計算攻擊

Dragonchain 嚴格遵循中本聰提出的「一 CPU 一票」理念，透過 Yespower 演算法確保挖礦權力分散在廣泛的 CPU 設備上，而非集中在 GPU 礦場或 ASIC 礦機手中。

---

## 2. 核心參數

| 參數 | 值 | 說明 |
|------|------|------|
| 出塊時間 | 5 秒 | 全球最快 PoW 區塊鏈之一 |
| 初始區塊獎勵 | 42.94967296 DRAGON | 僅創世塊 |
| 前期區塊獎勵 | 3.32988658 DRAGON | 前半年（0-3,153,600 塊） |
| 常規區塊獎勵 | 0.20807311 DRAGON | 半年後，每 4 年減半 |
| 減半間隔 | 25,228,800 塊（約 4 年） | |
| 總發行量 | 21,000,000 DRAGON | 約比特幣的總量 |
| 創世時間 | 2026-05-04 21:20:00 CST | Unix timestamp: 1777900800 |
| PoW 演算法 | Yespower 1.0.1 | CPU 友好型 |
| 簽名演算法 | Falcon-512 | 格基後量子密碼學 |
| 位址格式 | Bech32m (dragon1...) | 原生隔離見證 |
| 難度調整 | SugarShield-N510 | 510 塊移動視窗（約 42.5 分鐘） |
| P2P 埠 | 29000 | |
| RPC 埠 | 29001 | |
| 預挖/IEO/ICO | 無 | 完全公平啟動 |

---

## 3. 後量子安全

### 3.1 量子計算威脅

量子電腦對傳統公鑰密碼學構成根本性威脅。Shor 演算法可以在多項式時間內破解 RSA 和橢圓曲線離散對數問題（ECDLP），這意味著比特幣使用的 ECDSA（secp256k1）簽名演算法在足夠強大的量子電腦面前將完全失效。

2020 年 12 月，光子量子電腦展示了相對於傳統超級電腦的「量子優越性」[3]。儘管目前量子電腦的規模尚不足以破解比特幣簽名，但加密貨幣的設計週期應以數十年為單位——當量子電腦成熟時，未做防護的區塊鏈將面臨災難性安全危機。

### 3.2 Falcon-512 簽名演算法

Dragonchain 採用 **Falcon-512**（Fast-Fourier Lattice-based Compact Signatures over NTRU）作為數位簽名演算法。Falcon 是由 Gentry, Peikert 和 Vaikuntanathan 於 2008 年提出的格基簽名框架的實作，已被 NIST 後量子密碼學標準化程序選為最終入圍演算法 [4]。

**核心原理**：

Falcon 的安全性基於 NTRU 格上的短整數解問題（SIS）。其工作流程如下：

1. **金鑰生成**：生成一個 q-ary 格的短基（私鑰）和長基（公鑰）
2. **簽名**：
   a. 生成隨機鹽值 salt
   b. 計算目標 c = H(msg||salt)，其中 H 是將輸入映射到格上點的雜湊函數
   c. 利用短基計算靠近目標 c 的格點 v
   d. 輸出 (salt, s)，其中 s = c − v
3. **驗證**：驗證者接受簽名當且僅當：
   a. 向量 s 足夠短
   b. H(msg||salt) − s 是公鑰生成格上的有效點

**Falcon-512 特性**：

| 特性 | 數值 | 說明 |
|------|------|------|
| 簽名大小 | 690 位元組 | 遠小於同安全性的 RSA 簽名 |
| 公鑰大小 | 897 位元組 | 緊湊，適合嵌入式設備 |
| 經典安全性 | ~128 位 | 等效於 RSA-2048 |
| 簽名速度 | 數千次/秒 | 普通電腦上 |
| 驗證速度 | 比簽名快 5-10 倍 | |
| RAM 使用 | <30 KB | 適合記憶體受限設備 |

**抗量子特性**：Falcon 基於格密碼學，目前沒有已知的高效求解演算法——即使在量子電腦的輔助下也是如此。這與基於大數分解或離散對數的傳統密碼學形成鮮明對比。

### 3.3 Dragonchain 中的 Falcon-512 實作

Dragonchain 將 Falcon-512 完全整合到交易簽名和驗證流程中：

- **金鑰生成**：`PQCLEAN_FALCON512_CLEAN_crypto_sign_keypair()` 生成公鑰/私鑰對
- **交易簽名**：使用 `PQCLEAN_FALCON512_CLEAN_crypto_sign_signature()` 對交易雜湊進行簽名
- **簽名驗證**：使用 `PQCLEAN_FALCON512_CLEAN_crypto_sign_verify()` 驗證簽名有效性

```cpp
// 簽名大小: 690 位元組 (src/key.h)
#define PQCLEAN_FALCON512_CLEAN_CRYPTO_BYTES_ 690

// 簽名驗證 (src/pubkey.cpp)
int r = PQCLEAN_FALCON512_CLEAN_crypto_sign_verify(
    vchSig.data(), vchSig.size(),
    hash.begin(), 32,
    pch + 1
);
```

由於 Falcon-512 使用 1281 位元組的非標準金鑰結構，它**不相容 BIP32 分層確定性（HD）錢包派生**。Dragonchain 將此視為對安全性的合理權衡：後量子安全優先於 HD 便利性。

---

## 4. 工作量證明與共識

### 4.1 Yespower PoW 演算法

Dragonchain 採用 **Yespower 1.0.1** 作為工作量證明演算法 [5]。Yespower 基於 scrypt，具有以下特性：

- **CPU 友好**：在目前 CPU 上計算效率較高
- **GPU 不友好**：在目前 GPU 上的效率較低
- **FPGA/ASIC 中性**：不特別適合任何專用硬體

這種設計確保了挖礦權力分散在廣泛的通用計算設備上，真正實現了「一 CPU 一票」的去中心化理念。

**PoW 驗證流程**：

```
H(Nonce) = Hash(Hash(nVersion, hashPrevBlock, hashMerkleRoot, nTime, nBits, Nonce))

若 H(Nonce) < PoW_Target:
    工作量證明有效 → 廣播新區塊 → 獲得區塊獎勵
```

**最低難度設置**（powLimit）：
```
0x003fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
```
較低的 powLimit 使得：
1. 5 秒極速出塊成為可能
2. 低配置 CPU 也能參與挖礦

### 4.2 難度調整演算法：SugarShield-N510

Dragonchain 採用 **SugarShield-N510** 難度調整演算法（DAA），基於 Zcash 對 Digishield 的改進，使用 **510 個區塊**（約 42.5 分鐘）的移動平均視窗。

**核心公式**：

```
target_{h+1} = avg_N_targets * (1 + (t_N − t₀) / (4 × T × N) − 1/4)
```

其中 T = 5（目標出塊時間），N = 510（視窗大小）

**參數配置**：

| 參數 | 值 | 說明 |
|------|------|------|
| 目標出塊時間 | 5 秒 | |
| 難度週期 | 510 塊（42.5 分鐘） | |
| 最大下調 | 32% | 每週期 |
| 最大上調 | 16% | 每週期 |
| 難度調整啟動塊 | 第 511 塊 | |
| 難度穩定開始塊 | 第 512 塊 | |

**抗攻擊特性**：
- 使用未來時間限制（FTL）防範時間戳攻擊：區塊時間與目前主鏈頭部偏差超過 60 秒將被拒絕
- 節點時間與標準網路時間偏差不得超過 70 秒，否則將被網路封禁

### 4.3 最長鏈共識

Dragonchain 遵循 Nakamoto 共識的最長鏈規則：節點始終將累積工作量最大的鏈視為正確鏈，並在其基礎上繼續工作。當兩個節點同時廣播不同版本的下一個區塊時，節點處理第一個收到的區塊但保存另一個分支。下一次出塊時，工作量最大的鏈將成為最長分支，網路上大多數節點將切換到該分支。

---

## 5. 經濟模型

### 5.1 供給模型

Dragonchain 總發行量嚴格限定為 **21,000,000 DRAGON**，與經濟意義顯著的比特幣總量一致。

**區塊獎勵結構**：

```
階段 1（創世塊, 高度 0）:
  獎勵 = 42.94967296 DRAGON（= 2³² / 10⁸）

階段 2（前期, 高度 1 ~ 3,153,600，約半年）:
  獎勵 = 3.32988658 DRAGON/塊
  總量 = 10,500,000 DRAGON

階段 3（常規, 高度 3,153,601 起）:
  獎勵 = 0.20807311 DRAGON/塊
  每 25,228,800 塊（約 4 年）減半
  共 64 次減半 → 最終獎勵歸零
```

**總供給計算**：
```
總供給 = 42.94967296 + 10,500,000 + Σ(n=0→63) 25,228,800 × 0.20807311 / 2^n
       = 42.94967296 + 10,500,000 + 10,499,957.05032704
       ≈ 21,000,000 DRAGON
```

### 5.2 發行曲線

Dragonchain 採用**前置供給**策略：
- **前半年**釋放總量的一半（10,500,000 DRAGON），快速建立流通
- **半年後**進入標準的 4 年減半週期，逐步釋放剩餘部分
- **約 256 年後**，所有 DRAGON 被完全挖掘

此設計的優勢：
1. 早期礦工獲得充足的激勵參與網路建設
2. 快速建立的流動性有助於交易所和支付應用整合
3. 長期減半機制持續保持稀缺性

### 5.3 與比特幣經濟模型對比

| 參數 | Bitcoin | Dragonchain |
|------|---------|-------------|
| 總供應量 | 21,000,000 BTC | 21,000,000 DRAGON |
| 出塊時間 | 10 分鐘 | 5 秒 |
| 初始獎勵 | 50 BTC | 3.33 DRAGON |
| 減半週期 | 210,000 塊（~4 年） | 25,228,800 塊（~4 年） |
| 減半次數 | 33 次 | 64 次 |
| 全部挖完 | ~2140 年 | ~2282 年 |
| 最小單位 | 1 聰 (0.00000001) | 1 聰 (0.00000001) |

---

## 6. 位址與交易

### 6.1 Bech32m 位址格式

Dragonchain 預設使用 **Bech32m 編碼**（BIP350）的原生隔離見證（Native SegWit）位址，格式為：

```
dragon1qvazfa2ssu47wes89390sl0jz6g05h0267u8g
```

**位址解析**：
- `dragon`：人類可讀前綴（HRP），標識 Dragonchain 網路
- `1`：分隔符
- `q`：見證版本（v0）
- `vazfa2ssu47wes89390sl0jz6g05h0`：見證程式（Base32 編碼）
- `267u8g`：校驗和（6 位 BCH 碼）

**與傳統位址的對比**：

| 特性 | Legacy (Base58) | Bech32m (Native SegWit) |
|------|-----------------|------------------------|
| 範例位址 | S... | dragon1... |
| 大小寫 | 大小寫敏感 | 不區分大小寫 |
| QR 碼效率 | 低 | 高 |
| 差錯檢測 | 一般 | 極強（檢錯 + 糾錯） |
| 交易手續費 | 較高 | 較低 |

傳統位址前綴：public key 以 `S` 開頭，script hash 以 `s` 開頭。

### 6.2 原生隔離見證（Native SegWit）

Dragonchain 自創始區塊即**預設啟用隔離見證**（BIP141/BIP143/BIP147），所有交易預設使用隔離見證格式。相比傳統交易格式：

- 交易資料更緊湊，手續費更低
- 消除交易展延性（transaction malleability）
- 為二層網路擴展（如閃電網路）提供基礎

### 6.3 交易簽名

每筆 Dragonchain 交易均使用 **Falcon-512** 演算法簽名。簽名大小為 690 位元組，公鑰為 897 位元組。與 Bitcoin 的 ECDSA（~71 位元組簽名，33 位元組公鑰）相比體積更大，但換來了對量子計算攻擊的長期安全性。

---

## 7. 網路架構

### 7.1 P2P 網路

Dragonchain 採用全分散式點對點網路拓撲，不需要超級節點。每個全節點執行相同的配置和程式碼，透過廣播機制構建自組織網路。

**節點執行流程**：
1. 新交易廣播到所有節點
2. 每個節點收集新交易並打包為區塊
3. 每個節點執行 PoW 計算，若找到有效證明則廣播到全網
4. 節點收集工作量證明並生成有效的區塊
5. 僅當 PoW 和所有交易有效時，節點接受該區塊
6. 節點透過創建包含前一區塊雜湊的新區塊來表示對目前鏈的接受

### 7.2 磁碟與頻寬需求

| 指標 | 目前值 | 日增長 |
|------|--------|--------|
| 區塊數 | 635,000+ | ~21,000 塊/天 |
| 區塊鏈大小 | ~234 MB | ~8 MB/天 |
| 年增長 | — | ~2.9 GB/年 |

---

## 8. 安全性分析

### 8.1 量子安全評估

Dragonchain 的抗量子安全等級如下：

| 攻擊場景 | ECDSA (Bitcoin) | Falcon-512 (Dragonchain) |
|----------|:---:|:---:|
| 經典電腦攻擊 | 安全（128 位） | 安全（~128 位等效） |
| Shor 演算法（量子） | **完全破解** | **安全**（格問題無已知高效量子解法） |
| Grover 演算法（量子） | 安全性減半（64 位） | N/A（格問題不適用） |
| 側信道攻擊 | 取決於實作 | 取決於實作（同等水平） |

---

## 9. 路線圖

| 階段 | 時間 | 目標 |
|------|------|------|
| 第一階段 | 2026 Q2 | 主網上線、全節點發布、挖礦啟動 ✅ |
| 第二階段 | 2026 Q2 | 區塊瀏覽器上線、官方網站部署 ✅ |
| 第三階段 | 2026 Q3-2027+ | Android 手機錢包（SPV）、智慧合約等 |

---

## 10. 結論

Dragonchain 融合了 Sugarchain 的極速交易能力和 Tidecoin 的後量子安全特性，構建了一個兼具速度與安全性的下一代去中心化電子現金系統。

透過 5 秒出塊時間、Falcon-512 後量子簽名、Yespower CPU 友好挖礦和 SugarShield-N510 難度調整演算法，Dragonchain 在交易速度、量子安全、去中心化程度和經濟模型可持續性方面實現了系統性創新。

作為完全由社群驅動、無預挖、無 ICO 的公平啟動專案，Dragonchain 致力於成為全球使用者可信任的去中心化支付基礎設施。

---

## 參考文獻

[1] S. Nakamoto. Bitcoin: A Peer-to-Peer Electronic Cash System, 2008.
[2] P. W. Shor. Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer. SIAM Journal on Computing, 1997.
[3] IEEE Spectrum. Photonic Quantum Computer Displays 'Supremacy' Over Supercomputers, Dec 2020.
[4] Falcon: Fast-Fourier Lattice-based Compact Signatures over NTRU. https://falcon-sign.info/falcon.pdf
[5] Yespower. https://www.openwall.com/yespower/
[6] Zenny Kim. Sugarchain: A PoW Blockchain with Fastest Transactions and Halving without Rounding Errors.
[7] EverettX. Tidecoin: A Post-Quantum Security Peer-to-Peer Crypto Cash, 2020.

---

> Dragonchain 是一個開源專案，在 MIT 許可下發布。任何人均可自由使用、修改和分發本專案程式碼。
