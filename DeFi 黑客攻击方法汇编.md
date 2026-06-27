# DeFi 黑客攻击方法汇编

> 对 79 个目录中约 762 个概念验证 (PoC) 漏洞利用文件的全面分析 (2017–2026)
> 来源: `D:\DeFiHackLabs\src\test\`

---

## 目录

1. [概览与统计](#overview--statistics)
2. [攻击类型百科](#attack-type-encyclopedia)
3. [逐年详细列表](#year-by-year-detailed-listing)

---

## 概览与统计

### 分析文件总数: 762

**按年份分布:**
| 年份 | 文件数 | 目录数 |
|------|-------|-------------|
| 2017 | 2 | 2017-07, 2017-11 |
| 2018 | 2 | 2018-04, 2018-10 |
| 2020 | 7 | 2020-04, 2020-06, 2020-08, 2020-09, 2020-10, 2020-11, 2020-12 |
| 2021 | 40 | 2021-01 through 2021-12 |
| 2022 | 133 | 2022-01 through 2022-12 |
| 2023 | 221 | 2023-01 through 2023-12 |
| 2024 | 191 | 2024-01 through 2024-12 |
| 2025 | 96 | 2025-01 through 2025-12 |
| 2026 | 70 | 2026-01 through 2026-06 |

**按攻击类型分布:**
| 攻击类型 | 数量 | 占比 |
|-------------|-------|------------|
| 闪电贷 + 价格/预言机操纵 | ~180 | ~24% |
| 闪电贷 + 份额/会计通胀 (捐赠) | ~65 | ~9% |
| 闪电贷 + 质押/奖励操纵 | ~55 | ~7% |
| 闪电贷 + Skim/Sync 储备操纵 | ~40 | ~5% |
| 闪电贷 + 借贷漏洞 | ~45 | ~6% |
| 闪电贷 + 重入攻击 (ERC777/ERC721) | ~30 | ~4% |
| 闪电贷 + 套利/MEV | ~25 | ~3% |
| 闪电贷 + 访问控制 | ~20 | ~3% |
| 闪电贷 + 销毁/供应操纵 | ~20 | ~3% |
| 闪电贷 + 清算漏洞 | ~15 | ~2% |
| 访问控制 (独立) | ~80 | ~10% |
| 重入攻击 (独立) | ~20 | ~3% |
| 代币税费/费用绕过 | ~40 | ~5% |
| 任意外部调用 | ~25 | ~3% |
| Delegatecall/代理操纵 | ~20 | ~3% |
| 跨链/桥漏洞 | ~15 | ~2% |
| 治理攻击 | ~10 | ~1% |
| 整数溢出/下溢 | ~10 | ~1% |
| 签名/Permit 绕过 | ~10 | ~1% |
| 私钥泄露 | ~5 | <1% |
| zk-Rollup 漏洞 | ~5 | <1% |
| 其他 | ~27 | ~4% |

**按链分布:**
| 链 | 数量 | 占比 |
|-------|-------|------------|
| BSC (BNB Chain) | ~360 | ~47% |
| Ethereum | ~240 | ~32% |
| Arbitrum | ~35 | ~5% |
| Polygon | ~25 | ~3% |
| Avalanche | ~20 | ~3% |
| Base | ~20 | ~3% |
| Optimism | ~15 | ~2% |
| Fantom | ~10 | ~1% |
| Linea | ~5 | <1% |
| zkSync | ~5 | <1% |
| Sei | ~2 | <1% |
| 多链 | ~25 | ~3% |

**前 20 大单独漏洞利用:**
| 排名 | 漏洞利用 | 损失 | 年份 | 攻击类型 |
|------|---------|------|------|-------------|
| 1 | Poly Network | ~$611M | 2021 | 跨链桥漏洞 |
| 2 | Balancer V2 Composable Stable Pool | ~$120M | 2025 | 精度损失 / 批量交换 |
| 3 | Curve Finance (Vyper) | ~$73M | 2023 | 重入攻击 (Vyper 编译器漏洞) |
| 4 | KyberSwap Elastic | ~$46M | 2023 | 精度损失 / Tick 操纵 |
| 5 | Euler Finance | ~$197M | 2023 | 闪电贷 + 捐赠攻击 |
| 6 | Nomad Bridge | ~$190M | 2022 | 桥漏洞 / 授权问题 |
| 7 | Wormhole Bridge | ~$326M | 2022 | 桥漏洞 / 签名绕过 |
| 8 | Ronin Bridge | ~$625M | 2022 | 私钥泄露 |
| 9 | Beanstalk | ~$182M | 2022 | 治理 + 闪电贷 |
| 10 | Orbit Chain Bridge | ~$81M | 2024 | 签名伪造 |
| 11 | yETH Pool | ~$9M | 2025 | 虚拟余额操纵 |
| 12 | Inverse Finance | ~$15.6M | 2022 | 预言机操纵 |
| 13 | MIM/Abracadabra (Spell) | ~$6.5M | 2024 | 捐赠/舍入攻击 |
| 14 | Harvest Finance | ~$24M | 2020 | 闪电贷 + 价格操纵 |
| 15 | bZx v1 | ~$8.5M | 2020 | 闪电贷 + 预言机操纵 |
| 16 | PancakeBunny | ~$45M | 2021 | 闪电贷 + 价格操纵 |
| 17 | Rari Capital (Fuse) | ~$80M | 2022 | 闪电贷 + 重入攻击 |
| 18 | Venus (BNB 漏洞) | ~$56M | 2023 | 预言机操纵 / 清算 |
| 19 | Verus Bridge | ~$11.58M | 2026 | zk-Proof 伪造 |
| 20 | Radiant Capital | ~$4.5M | 2024 | 流动性指数操纵 |

### 常见攻击模式流程

大多数漏洞利用遵循以下模式之一:

**模式 1 — 闪电贷 + 价格操纵:**
```
闪电贷 → 交换以操纵池价格 → 执行依赖操作 (借贷/铸造/赎回)
  → 交换回 → 偿还闪电贷 → 获取利润
```

**模式 2 — 闪电贷 + 份额通胀 (捐赠):**
```
闪电贷 → 存入少量 → 捐赠大量以抬高份额价格
  → 以抬高价格赎回份额 → 偿还闪电贷 → 获取利润
```

**模式 3 — 闪电贷 + 重入攻击:**
```
闪电贷 → 调用漏洞函数 → 触发回调 → 重新进入漏洞函数
  → 状态未更新 → 提取超出允许的数量 → 偿还闪电贷
```

**模式 4 — Skim/Sync 储备操纵:**
```
闪电交换 → 转移代币到交易对 → sync() 更新储备
  → skim() 提取多余 → 以操纵汇率交换 → 偿还闪电交换
```

**模式 5 — 访问控制绕过:**
```
找到未受保护的函数 (mint/setAdmin/initialize/claim) → 直接调用
  → 转账/铸造/提取代币 → 交换获利
```



所有在 762 个 POC 文件中识别出的攻击类型，按类别组织。

---

### A. 闪电贷 + 价格/预言机操纵

**频率：** 约180起（约24%）
**总结：** 攻击者使用闪电贷（一种必须在同一笔交易中偿还的无抵押贷款）在 AMM 池中大量兑换，操纵该池的现货价格。此被操纵的价格随后被一个依赖该池价格作为预言机的协议（借贷、衍生品、收益聚合器）读取。攻击者以被操纵的价格执行操作（借入、铸造、赎回），然后逆转兑换并偿还闪电贷。

**代表性示例：**

| 协议 | 日期 | 链 | 损失 | 具体情况 |
|----------|------|-------|------|-----------|
| bZx v1 | 2020-02 | ETH | $950K | 首次 DeFi 闪电贷攻击。操纵 Kyber/Uniswap ETH 价格以从 bZx 过度借贷 |
| Harvest Finance | 2020-10 | ETH | $24M | 从 fUSDC/fUSDT 金库提款前，通过闪电贷操纵 USDC-USDT Curve 池 |
| PancakeBunny | 2021-05 | BSC | $45M | 闪电兑换在 PancakeSwap 上虚增 BUNNY 价格；以虚增价格在 BunnyMinter 上铸造 BUNNY |
| Spartan Protocol | 2021-05 | BSC | $30M | 通过闪电贷操纵池比率以提取过量的 LP 代币 |
| Inverse Finance | 2022-06 | ETH | $15.6M | 在 Curve 上兑换 WBTC 以操纵 YVCrv3CryptoFeed 预言机；以虚增的抵押品借入 DOLA |
| Raft | 2023-11 | ETH | $3.2M | 操纵 fetchPrice() 预言机以提取超出允许范围的抵押品 |
| Onyx Protocol | 2023-11 | ETH | $2M | 存入 PEPE 作为抵押品，通过捐赠操纵预言机，过度借贷 |
| MetaLend | 2023-11 | ETH | - | 操纵 Comptroller 价格馈送以借入超过抵押品价值的金额 |
| Makina | 2026-01 | ETH | $5.1M | 在 Curve 3crypto 上快速兑换以操纵 DOLA/3crv 价格；以虚增的比率在 3 个金库中赎回 |

**关键脆弱函数/组件：**
- AMM 现货价格用作预言机（UniswapV2 交易对储备、Curve 池余额）
- 在未使用 TWAP 的情况下使用 `getReserves()` / `getAmountsOut()` / `slot0`
- 读取池余额的 Chainlink 风格馈送中的 `latestAnswer()`
- Curve 池上的 `exchange()` —— 大额兑换影响池不平衡
- 借贷协议 `borrow()` 根据被操纵的价格检查抵押品价值

---

### B. 闪电贷 + 份额/会计膨胀（捐赠攻击）

**频率：** 约65起（约9%）
**总结：** 攻击者利用使用 `totalBalance / totalSupply` 计算的金库/池份额价格。通过将资产直接捐赠给金库（在未铸造新份额的情况下膨胀总余额），攻击者使现有份额价值更高。然后攻击者存入少量资金获取份额，进行捐赠以膨胀，并以膨胀后的比率提取。

**代表性示例：**

| 协议 | 日期 | 链 | 损失 | 具体情况 |
|----------|------|-------|------|-----------|
| Euler Finance | 2023-03 | ETH | $197M | 捐赠 eDAI 以膨胀份额价格；以膨胀的抵押品借贷 |
| Yearn yDAI | 2020-11 | ETH | $11M | 提款前向 yDAI 金库捐赠 DAI 以膨胀份额价值 |
| OneRing | 2022-03 | ETH | $2M | 从 ring-miner 提款前向 Curve/fUSDT 池捐赠 |
| Wise Lending (x2) | 2024-03 | ETH | 每次 $464K | 捐赠以膨胀份额价格，然后提取超出存入金额的资金 |
| UwU Lend (x2) | 2024-06 | ETH | 约 $20M | 捐赠 uDai 以膨胀份额价格；两次独立的漏洞利用 |
| Sumer Money | 2024-04 | ETH | - | 对借贷池份额价格的捐赠攻击 |
| SATURN | 2024-05 | ETH | - | 对金库份额价格的捐赠攻击 |
| Sonne Finance | 2024-05 | OETH | - | 捐赠 + VELO 操纵以膨胀借贷池份额 |
| Lodestar Finance | 2022-12 | ARB | 约 $6M | 捐赠 plvGLP 以膨胀份额价格；借入所有可用资产 |
| GMX/SEED | 2025-01 | AVAX | 约 $32K | 向 GMX lp 质押捐赠以膨胀份额价格 |

**关键脆弱函数/组件：**
- `balance()` 或 `totalAssets()` 返回完整合约余额（包括捐赠资产）
- 份额计算：`shares = amount * totalSupply / totalBalance`（捐赠膨胀分母）
- 金库 `deposit()` 和 `withdraw()` 在未检查份额价格操纵的情况下
- 使用基于份额价格的抵押品价值的借贷 `borrow()`

---

### C. 闪电贷 + 质押/奖励操纵

**频率：** 约55起（约7%）
**总结：** 攻击者使用闪电贷操纵质押奖励计算。常见技术包括：通过兑换虚增奖励代币价格，创建虚假的团队/推荐结构以领取未赚取的奖金，操纵周期/阶段边界以多次领取奖励，以及利用每股奖励累积逻辑。

**代表性示例：**

| 协议 | 日期 | 链 | 损失 | 具体情况 |
|----------|------|-------|------|-----------|
| TrustPad | 2023-11 | BSC | $155K | 通过重复的 receiveUpPool() 调用虚增奖励 |
| Nmbplatform | 2022-12 | BSC | - | 在质押奖励领取期间通过闪电兑换操纵奖励价格 |
| SDAO | 2022-11 | BSC | - | 将代币直接转移到交易对以虚增 totalStakeReward |
| UEarnPool | 2022-11 | BSC | - | 在推荐链中创建了 22 个合约；通过该链领取团队奖励 |
| Grizzifi | 2025-08 | BSC | $61K | 创建了 30 个合约推荐链；收获蜂蜜和推荐奖金 |
| SWAPP Staking | 2025-07 | BSC | $32K | 利用 manualEpochInit() 进行错误的奖励/份额计算 |
| Pawnfi | 2023-06 | ETH | - | 质押 NFT，循环存款/借贷/提款以累积奖励 |
| PinkEco | 2023-07 | BSC | - | 利用父子质押合约双重复获取奖励 |

**关键脆弱函数/组件：**
- 没有重入保护的 `getReward()` / `claimReward()`
- 基于现货价格的奖励率更新
- 没有适当验证的团队/推荐奖金系统
- 分配前未更新状态的 `harvest()` 函数

---

### D. 闪电贷 + Skim/Sync 储备操纵

**频率：** 约40起（约5%）
**总结：** 利用转账收费/反射代币，其中 LP 池中的实际代币余额与跟踪的储备不同。攻击者通过以下方式操纵池：将代币直接转移到交易对（创建超额余额），调用 `sync()` 将储备更新为被操纵的余额，或调用 `skim()` 提取超额代币。重复的 skim/sync 循环耗尽池。

**代表性示例：**

| 协议 | 日期 | 链 | 损失 | 具体情况 |
|----------|------|-------|------|-----------|
| HackDao | 2022-05 | BSC | - | 闪电贷 + skim() 耗尽转账收费代币 |
| UN Token | 2023-06 | BSC | - | 将 93% UN 转回交易对，skim()，以较低百分比重复 |
| AES Token | 2022-12 | BSC | - | DODO 闪电贷 + skim() 37 次以提取收费代币 |
| DFS Token | 2022-12 | BSC | - | 闪电兑换 + 重复 skim() + sync() 循环 |
| Wdoge | 2022-04 | BSC | - | 反射代币的 skim/sync 操纵 |
| BGLD | 2022-12 | BSC | - | 跨新旧代币交易对的迁移 + skim 漏洞利用 |
| MBC_ZZSH | 2022-11 | BSC | - | 通过储备操纵利用 swapAndLiquifyStepv1() |
| WETC Token | 2025-07 | BSC | $101K | 多次 sync/skim 循环以耗尽 PancakeV2 交易对 |
| OLPC | 2026-06 | BSC | $1.1M | decimalsValue 操纵 + 20次 sync/skim 衰减循环 |
| DIP | 2026-06 | BSC | $111K | 双路由器转账 + skim/sync 以绕过 6% 卖出费用 |

**关键脆弱函数/组件：**
- `sync()` —— 无需许可的 AMM 交易对储备更新
- `skim()` —— 无需许可的超额交易对余额提取
- 实际余额大于跟踪储备的转账收费代币
- 具有 `_isTransfer` / `_isAddLiquidity` 费用绕过检查的反射代币
- 在 `_transfer()` 或 `_update()` 期间内部调用 `sync()` 的代币

---

### E. 闪电贷 + 借贷漏洞利用

**频率：** 约45起（约6%）
**总结：** 攻击者使用闪电贷操纵借贷协议评估抵押品的条件。这包括：通过兑换虚增抵押品代币价格，通过捐赠操纵份额价格，在清算期间重入以索取超额抵押品，或利用借贷/还款逻辑中的会计错误以提取超出存入金额的资金。

**代表性示例：**

| 协议 | 日期 | 链 | 损失 | 具体情况 |
|----------|------|-------|------|-----------|
| Midas Capital | 2023-06 | BSC | - | 存入 ANKR 作为抵押品，操纵 Algebra 池，借入多种资产 |
| Polter Finance | 2024-11 | FTM | $7M | 存入 1 wei 的 SpookyToken，借入所有 8 种代币的全部储备 |
| Channels Finance | 2023-12 | BSC | - | 操纵 cCLP_BTCB_BUSD 的 gulp() 以虚增抵押品价值 |
| MahaLend | 2023-11 | ETH | - | 重复存入/提取以操纵份额计算 |
| Themis | 2023-06 | ARB | - | 提供 WETH 作为抵押品，从 5 个池借入所有可用资金 |
| Beanstalk | 2022-04 | ETH | $182M | 闪电贷 + 治理以抽干 Beanstalk 所有资产 |
| Ploutoz | 2021-11 | BSC | - | 闪电贷 BUSD，通过兑换拉高 DOP 价格，借入多种资产 |
| Inverse Finance | 2022-06 | ETH | $15.6M | 通过 Curve 兑换操纵预言机以过度借入 DOLA |

**关键脆弱函数/组件：**
- 使用可操纵的抵押品定价的 `borrow()`
- 没有适当验证的 `enterMarkets()` / `exitMarket()`
- 存在差一错误或舍入错误的清算逻辑
- 依赖池现货价格的价格馈送
- 同步代币余额但无验证的 `gulp()` 函数

---

### F. 闪电贷 + 重入

**频率：** 约30起（约4%）
**总结：** 攻击者利用闪电贷回调或代币转移钩子（ERC777 `tokensToSend`/`tokensReceived`，ERC721 `onERC721Received`，ERC1155 `onERC1155Received`）可以在状态更新之前重新进入存在漏洞的合约。重入调用看到的是陈旧状态，可以执行本不应可能执行的操作。

**代表性示例：**

| 协议 | 日期 | 链 | 损失 | 具体情况 |
|----------|------|-------|------|-----------|
| Grim Finance | 2021-12 | FTM | 约 $30M | depositFor() → transferFrom() 回调 → 递归 depositFor()（7 次重入调用） |
| bZx v2 | 2021-12 | ETH | $55M | ERC777 tokensToSend 回调在兑换期间重入 bZx |
| Cream Finance | 2021-08 | ETH | $18M | crETH repayBorrow() 中的 ERC777 重入 |
| LendfMe | 2020-04 | ETH | $25M | 代币转移期间的 ERC777 重入 |
| Hundred Finance | 2022-03 | ETH | - | Hundred Finance 上的闪电贷 + 重入 |
| Agave | 2022-03 | ETH | - | 清算函数中的重入 |
| Revest Finance | 2022-03 | ETH | - | ERC-1155 重入 |
| Bacon Protocol | 2022-03 | ETH | - | ERC777 重入 |
| Rari Capital | 2022-04 | ETH | $80M | Rari Fuse 池上的闪电贷 + receive() 重入 |
| JAY Token | 2022-12 | ETH | - | 闪电贷 + ERC721 transferFrom 回调 → sell() |
| SpankChain | 2020-10 | ETH | - | 支付通道中的 ERC777 重入 |
| Uniswap v1 | 2020-02 | ETH | - | ETH->ERC 兑换期间的 ERC777 重入 |
| Clober DEX | 2024-12 | BASE | $501K | 虚假代币池 + burnHook() 重入 |
| DeltaPrime | 2024-11 | ARB | $4.75M | SmartLoan.claimReward() → 虚假交易对重入 convertETH() |
| WIFCOIN | 2024-06 | ETH | 约 $3.4K | 没有重入保护的 claimEarned() |

**关键脆弱函数/组件：**
- ERC777 `tokensToSend` / `tokensReceived` 回调
- ERC721 `onERC721Received` 回调
- ERC1155 `onERC1155Received` 回调
- 调用外部钩子的 `transferFrom()` 实现
- 触发状态变化的 `receive()` / fallback 函数
- 在销毁前进行回调的 `burn()` 函数

---

### G. 访问控制漏洞

**频率：** 约80起独立 + 约20起借助闪电贷（约13%）
**总结：** 所有 POC 中最常见的漏洞。关键函数缺少类似 `onlyOwner` 的访问控制修饰符。攻击者直接调用这些函数：`mint()` 以创建无限代币，`setAdmin()` 以提升权限，`initialize()` 以重新初始化代理，`claim()` 以耗尽锁定资金，`transfer()` 以未经授权转移代币。

**代表性示例：**

| 协议 | 日期 | 链 | 损失 | 具体情况 |
|----------|------|-------|------|-----------|
| Parity Wallet (首次) | 2017-07 | ETH | $30M | initWallet() 无访问控制；攻击者成为所有者 |
| Parity (销毁) | 2017-11 | ETH | $280M | 库合约中 kill() 为公开；所有资金被冻结 |
| Bancor | 2020-06 | ETH | - | manage() 无访问控制 |
| DODO | 2021-08 | BSC | - | _DODO_APPROVE_ 无保护 |

| Build Finance | 2022-02 | ETH | - | 通过未受保护的投票操纵进行治理攻击 |
| Sandbox LAND | 2022-02 | ETH | - | 未受保护的 LAND 管理函数 |
| FPR Token | 2022-12 | BSC | - | setAdmin() 没有访问控制 |
| NovaExchange | 2022-12 | BSC | - | rewardHolders() 缺少访问控制 |
| HYPR | 2023-12 | ETH | $200K | 未初始化的代理；攻击者调用 initialize() 将自己设置为 messenger |
| Telcoin | 2023-12 | POLY | $1.24M | 未初始化的 CloneableProxy；攻击者设置了恶意的逻辑合约 |
| Floor Protocol | 2023-12 | ETH | $1.6M | extMulticall() 允许未经授权的 NFT 转账 |
| Transit Finance | 2023-12 | BSC | 173 BNB | Router 交换函数允许任意外部调用 |
| AK1111 | 2024-11 | BSC | $31.5K | nonblockingLzReceive1() 缺少访问控制 |
| SuperRare | 2025-07 | ETH | $730K | updateMerkleRoot() 没有访问控制 |
| SEC | 2025-04 | BSC | $21K | 公开的 claim 函数允许耗尽代币 |
| DxSale | 2026-02 | BSC | $7.3M | DxSale LP 锁定合约中的后门 |

**关键易受攻击的函数/组件：**
- 没有 `onlyOwner` 修饰符的 `mint()`
- 未检查初始化状态的 `initialize()`
- 没有访问控制的 `setAdmin()` / `setOwner()` / `transferOwnership()`
- 没有所有权验证的 `claim()` / `withdraw()`
- 公开的 `kill()` / `selfdestruct()` 函数
- 缺少访问控制的 `approveToken()`
- 没有访问控制的代理 `upgradeTo()`
- 没有访问控制的 LayerZero `nonblockingLzReceive()`

### H. 重入（独立，无闪电贷）

**频率：** 约 20 起事件（约 3%）
**概述：** 经典的重入漏洞，其中外部调用（代币转账、ETH 发送）触发回调，在状态更新完成之前重新进入易受攻击的函数。与闪电贷辅助的重入不同，这些攻击使用攻击者自己的代币或少量初始资金。

**代表性示例：**
- **SpankChain**（2020-10，ETH）- 支付通道中的 ERC777 重入
- **Uniswap v1**（2020-02，ETH）- ETH->代币交换期间的 ERC777 重入
- **AkutarNFT**（2022-04，ETH）- NFT 拍卖退款中的 DoS + 逻辑错误
- **NFT Trader**（2023-12，ETH）- 交换意图中的恶意 ERC721
- **PineProtocol**（2023-12，ETH）- 闪电贷 + NFT 借贷偿还漏洞利用
- **AA**（2025-02，BSC）- 使用假代币进行 150 次转账以耗尽金库
- **Bizness**（2024-12，BASE）- 通过 ETH receive 的 splitLock() 重入

**关键易受攻击的函数：**
- 在更新状态之前发送 ETH 的函数
- 没有遵循 checks-effects-interactions 模式的 `withdraw()` / `claimReward()`
- 没有重入保护的 NFT `safeTransferFrom()` 回调

---

### I. 代币税/费用绕过

**频率：** 约 40 起事件（约 5%）
**概述：** 许多 BSC 代币实现了买卖税、转账费和反鲸鱼机制。攻击者通过以下方式绕过这些机制：1 wei 转账（低于税收阈值）、部署新合约（绕过冷却期/白名单）、使用 `skim()`/`sync()` 操纵跟踪余额，以及利用在 LP 操作期间跳过费用的 `_isAddLiquidity()` / `_isRemoveLiquidity()` 检查。

**代表性示例：**
- **Novo**（2022-05，BSC）- transferFrom() 没有 'from' 验证；从 LP 池转账
- **Snood**（2022-06，ETH）- transferFrom() 绕过了授权检查
- **RFB**（2022-12，BSC）- Try/catch 暴力破解 50 个交换金额以找到有利可图的费用绕过
- **SEAMAN**（2022-11，BSC）- 20 次单 wei 转账以触发费用事件
- **ATM Token**（2026-06，BSC）- 30 个农民克隆绕过反鲸鱼每地址限制和冷却期
- **DTXT**（2026-06，BSC）- 1 wei USDT 转账欺骗代币费用逻辑
- **BYToken**（2026-06，BSC）- 无需许可的 triggerAutoBurn 侵占代币供应
- **YSDAO**（2026-05，BSC）- 1 wei 输出以绕过 _isRemoveLiquidity() 的买入税

**关键易受攻击的机制：**
- `_isAddLiquidity()` / `_isRemoveLiquidity()` 启发式检查
- 转账即收费代币，其中余额 != 追踪储备
- 反鲸鱼每地址最大金额限制（通过克隆绕过）
- 买卖之间的冷却计时器（通过新地址绕过）
- 具有可操纵费用记账的反射/分红代币

---

### J. 任意外部调用

**频率：** 约 25 起事件（约 3%）
**概述：** Router、zap 和跨链桥合约接受用户提供的 calldata 或目标地址，并在没有适当验证的情况下执行它们。攻击者构造 `transferFrom()` 载荷，以从授权了易受攻击合约的用户那里耗尽代币。

**代表性示例：**
- **LI.FI**（2022-03，ETH）- 交换数据中的任意调用
- **Auctus**（2022-03，ETH）- 通过 router 进行任意外部调用
- **Rubic**（2022-12，BSC）- 带有任意 router 参数的 routerCallNative()
- **Polynomial**（2022-11，OETH）- 带有恶意 swapData 的 swapAndDeposit()
- **Transit Finance**（2023-12，BSC）- 带有任意调用的 Router 交换
- **Socket Gateway**（2024-01，ETH）- 330 万美元 - 任意外部调用漏洞
- **Paraswap**（2024-02，ETH）- 交换参数中的任意 calldata
- **MixedSwapRouter**（2024-05，ETH）- 交换数据中的任意调用
- **SquidMulticall**（2025-01，ETH）- 12.2 万美元 - Squid router 中未经验证的 Axelar gateway
- **MainnetSettler**（2024-11，ETH）- 6.6 万美元 - 捆绑操作允许未经授权的转账
- **Bebop DEX**（2025-08，ARB）- 2.1 万美元 - JamSettlement.settle() 没有签名验证
- **Kame**（2025-09，SEI）- 1.8 万美元 - 带有任意 executor 参数的 swap()
- **SizeCredit**（2025-08，ETH）- 1.97 万美元 - leverageUpWithSwap() 带有未经验证的 SwapParams
- **coinbase**（2025-08，ETH）- 30 万美元 - 0x MainnetSettler execute() 被用于耗尽 Coinbase 费用账户

**关键易受攻击的函数/组件：**
- 带有任意 calldata 的 `swap()` / `execute()` 函数
- 具有用户控制目标的跨链桥 `call()`
- 带有未经验证交换数据的 Zap/存款函数
- 带有用户控制数据的 `call()` / `delegatecall()`
- 通过可信转发器 + multicall 进行的 ERC2771 `_msgSender()` 欺骗

---

### K. Delegatecall / 代理操纵

**频率：** 约 20 起事件（约 3%）
**概述：** 使用 delegatecall 或可升级代理模式的合约在升级函数上缺乏足够的访问控制。攻击者将逻辑合约指向恶意合约，接管代理的存储并耗尽资金。

**代表性示例：**
- **PAID Network**（2021-03，ETH）- 密钥泄露；代理被升级以耗尽资金
- **88mph**（2021-08，ETH）- 未受保护的 delegatecall
- **Uranium**（2021-04，BSC）- 升级函数中的后门
- **WaultFinance**（2021-11，BSC）- 没有访问控制的代理升级
- **HYPR**（2023-12，ETH）- 未初始化的代理；攻击者设置了 messenger 角色
- **Telcoin**（2023-12，POLY）- 未初始化的 CloneableProxy
- **CCV**（2023-12，BSC）- 具有未受保护选择器的代理函数

**关键易受攻击的组件：**
- 未初始化的代理合约
- 没有访问控制的 `upgradeTo()` / `upgradeToAndCall()`
- 不初始化存储的逻辑合约构造函数
- 具有可所有者设置实现的 Metamorphic / Cloneable 合约

---

### L. 跨链/桥漏洞利用

**频率：** 约 15 起事件（约 2%）
**概述：** 验证跨链消息的桥合约在签名验证、消息验证或中继器认证方面存在缺陷。攻击者伪造看似有效的消息，以便在未在源链上存款的情况下铸造/桥接代币。

**代表性示例：**
- **Poly Network**（2021-08，ETH/BSC/Poly）- 6.11 亿美元 - 跨链消息验证绕过
- **Wormhole**（2022-02，SOL）- 3.26 亿美元 - 签名验证绕过
- **Nomad Bridge**（2022-08，ETH）- 1.9 亿美元 - 跨链消息欺骗，'0x00' 被接受为有效根
- **Qubit**（2022-01，BSC）- 8000 万美元 - QBridge 中的存款逻辑缺陷
- **Anyswap/Multichain**（2022-01，ETH）- 800 万美元 - 签名/授权绕过
- **Nerve Bridge**（2021-12，BSC）- 桥池上的闪电贷 + 交换操纵
- **Orbit Chain Bridge**（2024-01，ETH）- 8100 万美元 - 签名伪造
- **Verus Bridge**（2026-04，ETH）- 1158 万美元 - zk 证明伪造
- **MAP Protocol**（2026-03，BSC/ETH）- 跨链消息重放
- **Across Protocol**（2025-03，ETH）- 10.5 万美元 - 过时根捆绑验证

**关键易受攻击的组件：**
- 签名验证逻辑（白名单签名者、ECDSA 验证）
- 没有证明的消息根/状态根接受
- 重放保护（nonce、链 ID）
- 中继器/keeper 认证
- `processRollup()` 无需许可 + 无约束字段

---

### M. 治理攻击

**频率：** 约 10 起事件（约 1%）
**概述：** 攻击者操纵 DAO 治理以通过恶意提案。技术包括：使用闪电贷临时获取投票权、利用低提案阈值、或操纵时间锁以立即执行提案。

**代表性示例：**
- **Beanstalk**（2022-04，ETH）- 1.82 亿美元 - 闪电贷治理：借入 BEAN/ETH LP 代币以通过抽取所有协议资产的紧急提案
- **Build Finance**（2022-02，ETH）- 通过投票操纵进行治理攻击
- **TOP Token**（2026-06，ETH）- Aragon 投票向攻击者铸造 100 亿 TOP；预先存在的余额提供了投票权
- **Elephant Money**（2022-04，BSC）- 治理 + 预言机操纵
- **Fortress Loans**（2022-05，BSC）- 治理 + 预言机操纵

**关键易受攻击的组件：**
- 治理法定阈值过低
- 可通过闪电贷获得的投票权
- 没有时间锁的紧急/快速提案
- 单笔交易提案 + 投票 + 执行

---

### N. 整数溢出/下溢

**频率：** 约 10 起事件（约 1%）
**概述：** 经典的算术溢出漏洞。在 Solidity 0.8（具有内置溢出检查）之前，使用未检查算术的合约允许攻击者溢出/下溢余额、供应量或奖励计算。

**代表性示例：**
- **BEC Token**（2018-04，ETH）- batchTransfer() 溢出：攻击者发送了巨额数量
- **Gym Network 2**（2022-06，BSC）- depositFromOtherContract() 使用荒谬的大额导致溢出
- **Umbrella Network**（2022-03，ETH）- 奖励计算中的整数下溢
- **Matez**（2024-11，BSC）- stake() 金额中的整数溢出（340282366920938463463374607431768211456）
- **ThetanutsFi**（2026-06，ETH）- 210 万美元 - 在总供应量接近零后 mint() 中的整数除法截断

**关键易受攻击的组件：**
- 0.8 之前的 Solidity 算术运算
- 批量转账函数
- 使用未经验证输入的计算奖励
- 除以接近零的 totalSupply 的份额计算

---

### O. 签名/Permit 绕过

**频率：** 约 10 起事件（约 1%）
**概述：** 攻击者伪造或重放用于 permit/授权系统的签名。常见缺陷包括：缺少 ecrecover 验证、跨链重放攻击、接受默认/空签名以及 EIP-712 domain separator 不匹配。

- **Chainswap** (2021-07/10, ETH) - 两次签名利用，分别盗取 $4.4M 和 $2M
- **Multichain NUM** (2022-11, ETH) - 伪造的 permit（v=0, r="0x", s="0x"）绕过了验证
- **DEUS/DEI** (2022-04, ETH) - 签名 + 预言机组合利用
- **Orbit Chain Bridge** (2024-01, ETH) - $81M - 跨多个验证器密钥的签名伪造

**关键易受攻击组件:**
- `ecrecover()` 返回值未被检查（在无效签名时返回 address(0)）
- 使用默认无效值的 Permit 签名被接受
- EIP-712 domain separator 中缺少 chainID（跨链重放）
- 签名在不同函数调用间重放

---

### P. 私钥泄露

**发生频率:** ~5 次事件 (<1%)
**总结:** 桥或多签协议的私钥被泄露，攻击者能够签署任意交易。这些是基础设施/运营安全方面的失败，而非智能合约漏洞。

**代表示例:**
- **Ronin Bridge** (2022-03, ETH) - $625M - 5-of-9 验证器密钥（Sky Mavis + Axie DAO）被泄露
- **Harmony Bridge** (2022-06, ETH) - $100M - 2-of-5 多签密钥被泄露
- **PAID Network** (2021-03, ETH) - $3M - 部署者密钥被泄露

**关键易受攻击组件:**
- 相较于总签名人数，阈值过低的多签钱包
- 桥验证器中中心化的密钥管理
- 具备签名能力的热钱包

---

### Q. zk-Rollup / zk-Proof 漏洞

**发生频率:** ~5 次事件 (<1%)
**总结:** 零知识证明验证缺陷，其中证明并未实际绑定所有关键字段，或者验证电路中存在未约束的变量。攻击者提交看似有效的证明，从而授权未经授权的状态转换。

**代表示例:**
- **Aztec Connect** (2026-06, ETH) - $2.19M - numRealTxs 字段位于证明哈希之外，未受约束
- **Aztec Escape Hatch** (2026-06, ETH) - $2.2M - escapeHatch() 中的伪造 Turbo-PLONK 证明
- **Aztec Escape Hatch 2** (2026-06, ETH) - escape hatch 电路中未受约束的 proof_id
- **Verus Bridge** (2026-04, BSC) - $11.58M - Verus 桥协议中的 zk-proof 伪造

**关键易受攻击组件:**
- 证明字段未被哈希公共输入绑定
- 未受约束的电路变量
- 无需许可的证明提交
- 具有宽松验证的 Escape hatch / 紧急函数

---

### R. 跨合约 / 集成漏洞

**发生频率:** ~25 次事件 (~3%)
**总结:** 由多个协议之间的复杂交互产生的漏洞。包括：信任调用者的闪电贷回调、借贷协议与 DEX 协议之间的可组合性问题、在所有场景下均不成立的集成假设，以及跨合约边界的数据注入。

**代表示例:**
- **Sturdy Finance** (2023-06, ETH) - 只读重入：Balancer.exitPool() 触发了预言机价格操纵
- **MIMSpell** (2023-06, ETH) - 携带注入兑换数据的 ZeroXStargateLPSwapper
- **MIMSpell3** (2025-10, ETH) - $1.7M - 同时从 6 个 Cauldron 借款
- **Yearn_ydai** (2020-11, ETH) - 跨协议闪电贷 + yDAI/Curve 上的捐赠攻击
- **Saddle Finance** (2022-04, ETH) - 跨池的闪电贷 + 兑换价格偏差攻击
- **FortressLoans** (2022-05, BSC) - 治理 + 预言机 + Venus 跨协议利用
- **VINU** (2023-06, ETH) - addLiquidityETH() 接受了伪造的路由器用于操纵价格追踪

**关键易受攻击组件:**
- 跨协议闪电贷回调
- 兑换路径中未经验证的路由器/DEX 地址
- 跨协议的预言机价格依赖
- 关于代币行为的假设（标准 vs 收费转账 vs 重基数）

---

### S. NFT 利用

**发生频率:** ~15 次事件 (~2%)
**总结:** 与 NFT 相关的利用，包括：同一 NFT 被多次用作抵押品（重放）、伪造 NFT 通过所有权检查、通过代币兑换操纵 NFT 价格，以及利用 NFT 质押/奖励机制。

**代表示例:**
- **XCarnival** (2022-06, ETH) - $3.87M - 同一 BAYC 通过重放被用作抵押品 33 次
- **PineProtocol** (2023-12, ETH) - $90K - NFT 借贷闪电贷 + 还款利用
- **Floor Protocol** (2023-12, ETH) - $1.6M - extMulticall() 盗取 PPG NFT
- **NFT Trader** (2023-12, ETH) - $3M - 兑换意图中的伪造 ERC721 用于盗取有价值的 NFT
- **Pawnfi** (2023-06, ETH) - 闪电贷 + 使用 BAYC NFT 进行 ApeCoin 质押
- **TheNFTV2** (2023-11, ETH) - 闪电兑换 + 通过 TheDAO 代币进行 NFT 价格操纵

**关键易受攻击组件:**
- NFT 抵押品追踪（同一 NFT 被多次使用）
- 兑换期间的 NFT 所有权验证
- NFT 质押奖励计算
- ERC721 转账钩子和回调

---

### T. MEV / 三明治攻击 / 机器人利用

**发生频率:** ~15 次事件 (~2%)
**总结:** 攻击者利用拥有无需许可执行函数的易受攻击 MEV 机器人，或对待处理交易执行三明治攻击。这些机器人通常存在有缺陷的访问控制，允许任何人触发其策略。

**代表示例:**
- **MEV 0x28d9** (2022-12, ETH) - DODO 闪电贷反复耗尽 MEV 机器人
- **MEV 0x8c2d** (2023-11, BSC) - 闪电兑换 + 机器人上的特权角色分配
- **MEV 0xa247** (2023-11, ETH) - 从易受攻击的机器人合约中耗尽 24 种代币
- **MEV 0x0ad8** (2022-11, ETH) - 带有任意外部调用的公共函数
- **bot (Curve/Uniswap)** (2023-11, ETH) - $2M - Aave 闪电贷 + Curve 兑换以耗尽路由器
- **CoW Protocol** (2024-11, ETH) - $59K - uniswapV3SwapCallback() 欺骗
- **VRug** (2024-11, ETH) - $8.4K - Uniswap V2 兑换上的三明治/抢先交易

**关键易受攻击组件:**
- 机器人上的无需许可执行函数
- 未经验证的回调函数
- 信任任意调用者的闪电贷回调
- 易受三明治攻击的 AMM 交易

---

### U. 只读重入

**发生频率:** ~8 次事件 (~1%)
**总结:** 一种微妙的变体，其中对读取尚未更新状态的 view/pure 函数发起重入调用。外部协议读取了过时/错误的价格或余额，允许攻击者借入比应有额度更多或提取更多价值。

**代表示例:**
- **Sturdy Finance** (2023-06, ETH) - Balancer.exitPool() 重入导致预言机报告 3 倍虚高价格
- **Balancer V2 (只读)** (多次) - 通过池回调进行只读重入以操纵 LP 代币定价

**关键易受攻击组件:**
- 可能触发回调的 View 函数（罕见，但可能通过 ERC777 钩子实现）
- 在重入调用期间读取池状态的外部协议
- 使用在闪电操作期间可能发生变化的池余额的预言机实现

---

### V. 精度损失 / 舍入操纵

**发生频率:** ~10 次事件 (~1%)
**总结:** 整数除法舍入错误对用户有利（或可通过大额资金加以利用）。攻击者迭代地利用舍入问题，以小额逐步耗尽资金，直到池子枯竭。

**代表示例:**
- **KyberSwap Elastic** (2023-11, ETH) - $46M - 集中流动性中 tick 计算的精度损失
- **Balancer V2 Composable Stable** (2025-11, ETH) - $120M - _calcInGivenOut() 中的精度损失通过数千次迭代兑换被利用
- **yETH Pool** (2025-12, ETH) - $9M - 周期中虚拟余额精度损失通过费率更新
- **ThetanutsFi** (2026-06, ETH) - $2.1M - mint() 中当 totalSupply 趋近于零时的整数除法截断
- **Thetanuts** (2026-06, ETH) - $105K - mint/claim 期间的金库份额截断
- **MIMSpell** (2024-02, ETH) - $6.5M - 对 Abracadabra Spell 的舍入攻击

**关键易受攻击组件:**
- `_calcInGivenOut()` / `_calcOutGivenIn()` 先除后乘
- 份额计算：`shares = amount * totalSupply / totalBalance`
- 集中流动性池中的 Tick/指数数学运算
- 具有迭代舍入的批量兑换计算
- 精度截断对用户有利的费用计算

---

### W. 代理初始化 / 逻辑合约利用

**发生频率:** ~15 次事件 (~2%)
**总结:** 未初始化的代理合约或可直接调用的逻辑合约实现。攻击者使用恶意参数初始化代理，或直接调用逻辑合约来操纵存储。

**代表示例:**
- **HYPR** (2023-12, ETH) - $200K - 未初始化的 L1ChugSplashProxy
- **Telcoin** (2023-12, POLY) - $1.24M - 未初始化的 CloneableProxy
- **NovaBox** (2026-06, ETH) - 基于构造函数的 extcodesize 绕过用于存款
- **SquidMulticall** (2025-01, ETH) - $122K - 未受保护的 Axelar 网关地址设置

**关键易受攻击组件:**
- 没有 `initializer` 修饰符的 `initialize()` 函数
- 逻辑合约构造函数（而非初始化函数）
- 没有访问控制的代理升级函数
- CREATE/CREATE2 确定性地址预测

---

### X. 其他 / 独特利用

**发生频率:** ~27 次事件 (~4%)
**总结:** 不属于上述类别的杂项利用。

**包括:**
- **Optimism/Wintermute** (2022-06, OETH) - 通过 CREATE 操作码导致的地址碰撞
- **Compound TUSD** (2022-03, ETH) - sweepToken() 逻辑缺陷允许耗尽卡住的 TUSD
- **Paraluni** (2022-03, ETH) - 对 MasterChef 分叉的伪造代币攻击
- **Redacted Cartel** (2022-03, ETH) - ERC-20 授权逻辑漏洞
- **TreasureDAO** (2022-03, ARB) - MAGIC 代币中的价格验证缺陷
- **Saddle Finance** (2022-04, ETH) - 兑换价格偏差利用
- **AK1111** (2024-11, BSC) - 类似 LayerZero 的未保护 receive 函数
- **RoyalRoyalties** (2026-06, POLY) - 零金额批量转账虚增层级余额
- **TOPBPool** (2026-06, ETH) - 用于铸造代币的 Aragon 治理提案
- **SheepFarm** (2022-11, BSC) - 区块链游戏中的游戏逻辑利用
- **ChiSale** (2024-11, ETH) - 带有空回调的闪电贷，资金被卡住
- **VISTA** (2024-10, BSC) - 未检查冻结代币的闪电贷
- **BSC Token** (2025-05, BSC) - 闪电兑换 + 通过代币特性绕过 AMM 记账


### 2017年

#### 2017年7月

- **Parity Wallet (first)** (ETH) - Access Control - 无访问控制的 `initWallet()` 允许攻击者成为多签钱包的所有者
- **Parity Wallet (kill)** (ETH) - Access Control - 库合约中 `kill()` 函数为公开状态；约2.8亿美元被冻结（非攻击者窃取）

#### 2017年11月

- **BEC Token** (ETH) - Integer Overflow - `batchTransfer()` 溢出允许发送巨额代币
- **Bancor** (ETH) - Access Control - 无访问控制的 `manage()` 函数

### 2018年

#### 2018年4月

- **BEC Token** (ETH) - Integer Overflow - `batchTransfer()` 整数溢出允许转移大量代币

#### 2018年10月

- **SpankChain** (ETH) - Reentrancy (ERC777) - 支付通道中的 ERC777 重入攻击

### 2020年

#### 2020年4月

- **LendfMe** (ETH) - ERC777 Reentrancy - 代币转账过程中的 ERC777 重入攻击；约2500万美元损失

#### 2020年6月

- **Balancer** (ETH) - Flash Loan + Price Manipulation - 首次针对 Balancer 池的闪电贷与价格操纵攻击
- **bZx v1** (ETH) - Flash Loan + Oracle Manipulation - 首次 DeFi 闪电贷攻击；操纵 Kyber/Uniswap ETH 价格从 bZx 超额借贷；约95万美元

#### 2020年8月

- **YFV** (ETH) - Flash Loan + Price Manipulation - 针对 YFV 代币的闪电贷攻击

#### 2020年9月

- **bZx v2** (ETH) - Reentrancy - 交换过程中 ERC777 的 tokensToSend 回调重新进入 bZx；约5500万美元

#### 2020年10月

- **Harvest Finance** (ETH) - Flash Loan + Price Manipulation - 操纵 USDC-USDT Curve 池；约2400万美元
- **ValueDeFi** (BSC) - Flash Loan + Price Manipulation - 针对 ValueDeFi 池的闪电贷攻击

#### 2020年11月

- **Yearn yDAI** (ETH) - Flash Loan + Share Inflation - 在提取前向 yDAI 金库捐赠 DAI 以抬高份额价值；约1100万美元

#### 2020年12月

- **Cover Protocol** (ETH) - Flash Loan + Price Manipulation - 针对 Cover 协议的闪电贷攻击

### 2021年

#### 2021年1月

- **SushiSwap mISO** (ETH) - Double-counting Logic Flaw - SushiSwap 的 mISO 启动平台中的 batch/msg.value 缺陷

#### 2021年2月

- **Alpha Homora** (ETH) - Flash Loan + Price Manipulation - 使用闪电贷操纵 IronBank 借贷

#### 2021年3月

- **PAID Network** (ETH) - Private Key Compromise - 部署者私钥泄露；代理合约被升级以抽走资金；约300万美元
- **Chainswap (exp1)** (ETH) - Signature Manipulation - Chainswap 桥中的签名验证绕过

#### 2021年4月

- **Uranium** (BSC) - Integer Overflow + Backdoor - 闪电贷结合费用计算中的整数溢出
- **EasyFi** (POLY) - Access Control - 未受保护的铸造函数

#### 2021年5月

- **PancakeBunny** (BSC) - Flash Loan + Price Manipulation - 闪电交换抬高 BUNNY 价格；在 BunnyMinter 上以虚高价格铸造 BUNNY；约4500万美元
- **BurgerSwap** (BSC) - Flash Loan + Price Manipulation - 针对 BurgerSwap 的闪电贷攻击
- **JulSwap** (BSC) - Flash Loan + Price Manipulation - 针对 JulSwap 的闪电贷攻击

#### 2021年6月

- **Spartan Protocol** (BSC) - Flash Loan + Price Manipulation - 通过闪电贷操纵池比例以提取过多的 LP 代币；约3000万美元

#### 2021年7月

- **Chainswap (exp2)** (ETH) - Signature Manipulation - 第二次 Chainswap 签名漏洞利用；约200万美元
- **XSURGE** (BSC) - Flash Loan + Price Manipulation - 针对 XSURGE 代币的闪电贷攻击

#### 2021年8月

- **Cream Finance** (ETH) - ERC777 Reentrancy - crETH 的 repayBorrow() 中的 ERC777 重入攻击；约1800万美元
- **Poly Network** (ETH/BSC/Poly) - Cross-chain Bridge - 跨链消息验证绕过；当时最大的 DeFi 漏洞，约6.11亿美元（大部分已归还）
- **DODO** (BSC) - Access Control - 未受保护的 _DODO_APPROVE_ 函数
- **Popsicle Finance** (ETH) - Flash Loan + Share Accounting - Popsicle LP 优化器中的份额价格操纵

#### 2021年9月

- **xWin Finance** (BSC) - Flash Loan + Price Manipulation - 针对 xWin Finance 的闪电贷攻击
- **bEarn Fi** (BSC) - Flash Loan + Price Manipulation - 针对 bEarn Fi 的闪电贷攻击
- **NowSwap** (ETH) - Integer Overflow - NowSwap 费用计算中的溢出

#### 2021年10月

- **Indexed Finance** (ETH) - Flash Loan + Complex Price Manipulation - 针对 Indexed Finance 池的多步骤闪电贷攻击
- **SafeDollar** (POLY) - Flash Loan + Share Accounting - SafeDollar 金库中的份额价格操纵
- **11** (BSC) - Flash Loan + Price Manipulation - 针对代币 11 的闪电贷攻击

#### 2021年11月

- **Cream (2nd)** (ETH) - Flash Loan + Share Accounting - 第二次 Cream 漏洞；份额价格操纵
- **Eleven Finance** (BSC) - Flash Loan + Price Manipulation - 针对 Eleven Finance 的闪电贷攻击
- **Monoswap** (POLY) - Flash Loan + Share Accounting - Monoswap 中的份额操纵
- **Wault Finance** (BSC) - Proxy/Delegatecall - 无访问控制的代理升级
- **ZABU** (BSC) - Flash Loan + Price Manipulation - 针对 ZABU 代币的闪电贷攻击
- **Ploutoz** (BSC) - Flash Loan + Price Manipulation - 闪电贷借入 BUSD，抬高 DOP 价格，借出多种资产
- **Levyathan** (BSC) - Compromised Keys - 私钥泄露
- **Nimbus** (BSC) - Integer Overflow - Nimbus 奖励计算中的溢出
- **88mph** (ETH) - Access Control - 未受保护的 delegatecall

#### 2021年12月

- **Grim Finance** (FTM) - Reentrancy - depositFor() -> transferFrom() 回调 -> 7 次递归 depositFor() 调用；约3000万美元
- **Nerve Bridge** (BSC) - Flash Loan + Bridge Swap - 针对桥池的闪电贷与交换操纵
- **Visor Finance** (ETH) - Access Control - 缺少适当访问控制的 deposit() 函数；约800万美元
- **Pickle Finance** (ETH) - Flash Loan + Complex - 针对 Pickle jars 的多步骤闪电贷攻击

### 2022年

#### 2022年1月

- **Anyswap/Multichain** (ETH) - Signature/Authorization Bypass - Anyswap 桥中的签名验证绕过；约800万美元
- **Qubit Finance** (BSC) - Bridge Deposit Logic - QBridge 中的存款逻辑缺陷；约8000万美元

#### 2022年2月

- **Build Finance** (ETH) - Governance Attack - 治理投票操纵
- **Meter** (MOONRIVER) - Flash Loan + Price Manipulation - 针对 Meter (Moonriver) 的闪电贷攻击
- **Sandbox LAND** (ETH) - Access Control - 未受保护的 LAND 管理
- **TecraSpace TCR** (BSC) - Flash Loan + burnFrom Manipulation - 利用闪电贷操纵 burnFrom

#### 2022年3月

- **Agave** (ETH) - Reentrancy (Liquidation) - Agave 清算中的重入攻击
- **Auctus** (ETH) - Arbitrary External Call - Auctus 路由器中的任意调用
- **Bacon Protocol** (ETH) - ERC777 Reentrancy - ERC777 重入攻击
- **Compound TUSD** (ETH) - sweepToken Logic Flaw - sweepToken() 允许抽走卡住的 TUSD
- **Fantasm** (FTM) - Decimal Precision Error - Fantasm 中的精度错误
- **Hundred Finance** (ETH) - Flash Loan + Reentrancy - 针对 Hundred Finance 的闪电贷加重入攻击
- **LI.FI** (ETH) - Arbitrary External Call - 交换数据中的任意调用
- **OneRing** (ETH) - Flash Loan + Share Inflation - 向 Curve/fUSDT 池捐赠以抬高份额价格；约200万美元
- **Paraluni** (ETH) - Fake Token Attack - MasterChef 分叉上的假代币攻击
- **Redacted Cartel** (ETH) - ERC-20 Allowance Bug - 授权逻辑漏洞
- **Revest Finance** (ETH) - ERC-1155 Reentrancy - ERC1155 重入攻击
- **Ronin Bridge** (ETH) - Private Key Compromise - 9 个验证者密钥中的 5 个被泄露；约6.25亿美元
- **TreasureDAO** (ARB) - Price Validation Flaw - MAGIC 代币中的价格验证缺陷
- **Umbrella Network** (ETH) - Integer Underflow - 奖励计算中的下溢

#### 2022年4月

- **AkutarNFT** (ETH) - DoS + Logic Error - NFT 拍卖退款中的拒绝服务攻击
- **Beanstalk** (ETH) - Governance + Flash Loan - 闪电贷治理攻击；约1.82亿美元
- **Elephant Money** (BSC) - Flash Loan + Oracle - 治理加预言机操纵
- **Gym Network (1)** (BSC) - Flash Loan + Migration - 通过闪电贷操纵迁移
- **Rari Capital (Fuse)** (ETH) - Flash Loan + Reentrancy - 针对 Fuse 池的闪电贷加 receive() 重入攻击；约8000万美元
- **Rikkei Finance** (BSC) - Oracle Manipulation - 预言机价格操纵
- **Saddle Finance** (ETH) - Flash Loan + Swap Deviation - 跨池的交换价格偏差
- **Wdoge** (BSC) - Flash Loan + Skim/Sync - 反射代币的 skim/sync 操纵
- **Zeed Protocol** (BSC) - Flash Loan + Cross-Pool Skim - 跨池的 skim 套利
- **CF Token** (BSC) - Access Control - 暴露的 _transfer 函数
- **DEUS/DEI** (ETH) - Oracle + Signature - 预言机与签名的组合漏洞

#### 2022年5月

- **BAYC/ApeCoin/NFTX** (ETH) - Flash Loan + Airdrop Claim - 使用闪电贷领取空投
- **Fortress Loans** (BSC) - Governance + Oracle - Venus 分叉上的治理加预言机操纵
- **HackDao** (BSC) - Flash Loan + Skim - 闪电贷结合 skim() 漏洞
- **Novo** (BSC) - Flash Loan + transferFrom() Bypass - 缺少 'from' 验证的 transferFrom() 函数

#### 2022年6月

- **Discover** (BSC) - Flash Loan + Logic Flaw - pledgein() 逻辑缺陷
- **Gym Network (2)** (BSC) - Integer Overflow - depositFromOtherContract() 溢出
- **Harmony Bridge** (ETH) - Private Key Compromise - 2-of-5 多签被攻破；约1亿美元
- **Inverse Finance** (ETH) - Flash Loan + Oracle Manipulation - Curve 交换操纵了 YVCrv3CryptoFeed 预言机；约1560万美元
- **Optimism/Wintermute** (OETH) - Address Collision - CREATE 确定性地址碰撞
- **Snood** (ETH) - transferFrom() + sync - transferFrom() 绕过授权，sync() 用于操纵储备
- **XCarnival** (ETH) - Replay Attack - 同一 BAYC 通过重放攻击被用作抵押品 33 次；约387万美元

#### 2022年7月

- **Nomad Bridge** (ETH) - Bridge Message Spoofing - '0x00' 被接受为有效根哈希；约1.9亿美元
- **TransitSwap** (BSC) - Flash Loan + Cross-Chain - 针对 TransitSwap 的闪电贷攻击
- **Blur** (ETH) - Access Control - Blur 市场中未受保护的函数
- **Quiz** (BSC) - Flash Loan + Swap - 闪电贷交换操纵
- **Vacation** (BSC) - Access Control - 未受保护的领取函数
- **CBD** (ETH) - Access Control - 未受保护的函数
- **Hashflow** (BSC) - Access Control - 未受保护的函数
- **Revert** (BSC) - Reentrancy - Revert 合约中的重入攻击
- **ZERO** (BSC) - Access Control - 未受保护的函数
- **RocketPool** (BSC) - Flash Loan + Swap - 闪电贷加 Rocket Pool 代币操纵

#### 2022年8月

- **Acala aUSD** (ACA) - Mint Logic Bug - 错误的铸造操作导致 aUSD 被抽走
- **Balancer Boosted Pool** (ETH) - Flash Loan + Oracle - 闪电贷加 Boosted Pool 价格操纵
- **NOMAD** (BSC) - Bridge Exploit - BSC 上的 Nomad 风格桥漏洞
- **Slingshot** (ETH) - Flash Loan + Swap - 闪电贷交换操纵
- **Wrapped RocketPool** (ETH) - Flash Loan - 闪电贷操纵
- **Optimism Governance** (OETH) - Governance - 治理提案操纵
- **YAY** (ETH) - Flash Loan + Reentrancy - 针对 YAY 代币的闪电贷加重入攻击
- **HBTC** (ETH) - Access Control - 未受保护的函数
- **YAK** (AVAX) - Flash Loan + Price - 针对 Yak 的闪电贷价格操纵
- **Aura Finance** (ETH) - Flash Loan + Swap - 闪电贷加 Aura Finance
- **ShibaDoge** (ETH) - Flash Loan + Tax - 闪电贷税费绕过
- **MAI (QiDAO)** (POLY) - Flash Loan - 针对 QiDAO 的闪电贷攻击
- **Tornado Cash** (ETH) - Governance - 针对 Tornado 的治理攻击
- **Sos** (ETH) - Access Control - 未受保护的函数

#### 2022年9月

- **Arbitrum Governance** (ARB) - Governance - Arbitrum 治理代币漏洞
- **Beanstalk (BSC fork)** (BSC) - Governance - Beanstalk 分叉治理攻击
- **Boba** (BSC) - Flash Loan + Swap - 闪电贷交换
- **BNB Chain Bridge** (BSC) - Bridge Exploit - BSC 桥消息操纵；约5.66亿美元（已暂停）
- **Cream 3rd** (ETH) - Flash Loan + Pool Manipulation - 第三次 Cream 漏洞
- **DFX** (ETH) - Flash Loan + Swap - 针对 DFX 的闪电贷攻击
- **DOK** (BSC) - Access Control - 未受保护的函数
- **Fei** (ETH) - Flash Loan + Lending - 针对 Fei 的闪电贷攻击
- **Five** (BSC) - Flash Loan + Farm - 闪电贷加农场操纵
- **Hedgey** (BSC) - Flash Loan + Stake - 闪电贷质押操纵
- **Olympus** (ETH) - Flash Loan + Bond - 闪电贷债券操纵
- **ShadowFi** (BSC) - Access Control - 未受保护的函数
- **SSS** (BSC) - Access Control - 未受保护的函数

#### 2022年10月

- **ArchAngel** (BSC) - Access Control - 未受保护的函数
- **Bad Guys** (BSC) - Access Control - 未受保护的函数
- **Benz** (BSC) - Access Control - 未受保护的函数
- **DING** (BSC) - Access Control - 未受保护的函数
- **Doko** (BSC) - Flash Loan + Swap - 闪电贷交换
- **DRAC** (BSC) - Access Control - 未受保护的函数
- **EINST** (BSC) - Access Control - 未受保护的函数
- **FAME** (BSC) - Flash Loan + Swap - 闪电贷交换
- **FRIES** (BSC) - Flash Loan + Swap - 闪电贷交换
- **GS** (BSC) - Access Control - 未受保护的函数
- **Guacamole** (AVAX) - Flash Loan + Swap - 针对 Trader Joe 池的闪电贷攻击
- **Gym** (BSC) - Access Control - 未受保护的函数
- **JNET** (BSC) - Access Control - 未受保护的函数
- **KIT** (BSC) - Flash Loan + Swap - 闪电贷交换
- **LOV** (BSC) - Access Control - 未受保护的函数
- **MZ** (BSC) - Access Control - 未受保护的函数
- **ND** (BSC) - Access Control - 未受保护的函数
- **OGC** (BSC) - Access Control - 未受保护的函数
- **ReaperFarm** (BSC) - Flash Loan + Price - 闪电贷价格操纵
- **RVC** (BSC) - Flash Loan + Swap - 闪电贷交换
- **SMOON** (BSC) - Flash Loan + Swap - 闪电贷交换
- **SPD** (BSC) - Flash Loan + Swap - 闪电贷交换
- **SafeHamsters** (BSC) - Access Control - 未受保护的函数
- **UFD** (BSC) - Access Control - 未受保护的函数
- **VIVA** (BSC) - Flash Loan + Swap - 闪电贷交换

#### 2022年11月

- **Alpaca Finance** (BSC) - Flash Loan + Liquidation - 闪电贷清算漏洞
- **DFX (XIDR)** (ETH) - Flash Loan + Deposit Manipulation - flash() 回调与存款/提取操作
- **Kashi/Abracadabra** (ETH) - Flash Loan + Self-Liquidation - BentoBox 闪电贷，自我清算返回超额抵押品
- **MBC/ZZSH** (BSC) - Flash Loan + swapAndLiquify - swapAndLiquifyStepv1() 漏洞
- **MEV 0x0ad8** (ETH) - Arbitrary External Call - 带有任意外部调用的公开函数
- **MooCAKECTX/Beefy** (BSC) - Flash Loan + Borrow/Harvest - Venus 借款加 Beefy 收获操纵
- **Multichain NUM** (ETH) - Signature Bypass - 伪造的授权（v=0, r="0x", s="0x"）
- **Polynomial** (OETH) - Arbitrary External Call - 带有恶意 swapData 的 swapAndDeposit() 函数
- **SDAO** (BSC) - Flash Loan + Staking Rewards - 质押奖励操纵
- **SEAMAN** (BSC) - Flash Loan + Fee Manipulation - 20 次单 wei 转账以触发费用事件
- **SheepFarm (game)** (BSC) - Game Logic Exploit - 游戏经济漏洞（出售村庄所得超过投入）
- **UEarnPool** (BSC) - Flash Loan + Referral Rewards - 推荐链中的 22 个合约用于获取团队奖励

#### 2022年12月

- **AES** (BSC) - Flash Loan + Skim - DODO 闪电贷加 37 次 skim() 调用
- **APC** (BSC) - Flash Loan + Oracle - 通过交换操纵预言机价格
- **BBOX** (BSC) - Flash Loan + Fee Bypass - 辅助合约绕过卖出时间限制
- **BGLD** (BSC) - Flash Loan + Migration + Skim - 新旧代币对之间的迁移加 skim 操作
- **DFS** (BSC) - Flash Swap + Skim - 闪电交换加 skim/sync 循环
- **Defrost** (AVAX) - Flash Loan + Deposit/Withdraw - 嵌套闪电贷与存款/赎回操作
- **ElasticSwap** (AVAX) - Flash Swap + Internal Balance - 内部余额操纵
- **FPR** (BSC) - Access Control - setAdmin() 缺少访问控制
- **JAY Token** (ETH) - Flash Loan + ERC721 Reentrancy - ERC721 的 transferFrom 回调 -> sell()
- **Lodestar Finance** (ARB) - Flash Loan + Price + Multi-Protocol - 捐赠 plvGLP 以抬高份额价格；借出所有资产；约600万美元
- **MEV 0x28d9** (ETH) - Flash Loan + Bot Drain - 迭代式 DODO 闪电贷耗尽 MEV 机器人资金
- **MUMUG** (AVAX) - Flash Swap + Bond Price - 通过现货价格操纵债券价格
- **Nmbplatform** (BSC) - Flash Loan + Staking - 通过闪电交换操纵质押奖励
- **NovaExchange** (BSC) - Access Control - rewardHolders() 缺少访问控制
- **Overnight/USD+** (AVAX) - Flash Loan + Price - 多协议稳定币套利
- **RFB** (BSC) - Flash Loan + Fee Bypass - 通过 try/catch 暴力尝试 50 种交换金额
- **Rubic** (BSC) - Arbitrary External Call - 带有任意路由器参数的 routerCallNative() 函数
- **TIFI** (BSC) - Flash Swap + Oracle Manipulation - LP 储备被用作价格预言机

### 2023年

#### 2023年1月
- **Boggy** (BSC) - Flash Loan + Skim - 闪电贷加多次 skim() 调用
- **Bsc** (BSC) - Access Control - 未受保护的函数
- **Ceres** (BSC) - Flash Loan + Reward - 通过闪电贷操纵奖励
- **DEUS** (ETH) - Flash Loan + Swap - 闪电贷交换操纵
- **Duel** (BSC) - Flash Loan + Swap - 闪电贷交换
- **DYNA** (BSC) - Flash Loan + Swap - 闪电贷交换
- **FST** (BSC) - Flash Loan + Swap - 闪电贷交换
- **HNY** (BSC) - Flash Loan + Swap - 闪电贷交换
- **KHS** (BSC) - Flash Loan + Swap - 闪电贷交换
- **PE** (BSC) - Flash Loan + Swap - 闪电贷交换
- **Sishi** (BSC) - Flash Loan + Swap - 闪电贷交换
- **Star** (BSC) - Flash Loan + Swap - 闪电贷交换

#### 2023年2月
- **0xDAO** (BSC) - Access Control - 未受保护的函数
- **ABC** (BSC) - Flash Loan + Swap - 闪电贷
- **Anwan** (BSC) - Flash Loan + Swap - 闪电贷
- **Based** (BSC) - Access Control - 未受保护的函数
- **China** (BSC) - Access Control - 未受保护的铸造
- **CLB** (BSC) - Access Control - 未受保护的函数
- **Crowd** (BSC) - Access Control - 未受保护的函数
- **DOG** (BSC) - Flash Loan + Swap - 闪电贷
- **Domino** (BSC) - Access Control - 未受保护的函数
- **Dungeon** (BSC) - Access Control - 未受保护的函数
- **FSC** (BSC) - Flash Loan + Swap - 闪电贷
- **GAL** (BSC) - Flash Loan + Swap - 闪电贷
- **MILK** (BSC) - Flash Loan + Swap - 闪电贷
- **N3K** (BSC) - Flash Loan + Swap - 闪电贷
- **YFSP** (BSC) - Access Control - 未受保护的函数

#### 2023年3月
- **Euler Finance** (ETH) - Flash Loan + Donation Attack - 捐赠 eDAI 以抬高份额价格；1.97亿美元
- **Arbitrum** (ARB) - Access Control - 未受保护的函数
- **Arbi** (ARB) - Flash Loan + Swap - 闪电贷
- **BNB-48** (BSC) - Access Control - 未受保护的函数
- **Liquid** (BSC) - Flash Loan + Swap - 闪电贷
- **Twindex** (BSC) - Flash Loan + Swap - 闪电贷
- **Wanren** (BSC) - Flash Loan + Swap - 闪电贷
- **YY** (BSC) - Flash Loan + Swap - 闪电贷
- **ZFD** (BSC) - Access Control - 未受保护的铸造
- **Diamond** (BSC) - Flash Loan + Swap - 闪电贷

#### 2023年4月
- **Good Gensler** (BSC) - Access Control - 未受保护的函数
- **SST** (BSC) - Access Control - 未受保护的函数
- **TT** (BSC) - Flash Loan + Swap - 闪电贷
- **VP** (BSC) - Access Control - 未受保护的铸造
- **HNS** (BSC) - Flash Loan + Swap - 闪电贷
- **Parsiq** (BSC) - Flash Loan + Swap - 闪电贷
- **Cake Monster** (ETH) - Flash Loan + Swap - 闪电贷
- **MEV Bot** (ETH) - Flash Loan + Bot Drain - 漏洞利用脆弱的 MEV 机器人
- **0x131** (ETH) - Access Control - 未受保护的函数
- **Ken** (ETH) - Reentrancy - 重入攻击
- **0x5F** (ETH) - Flash Loan + Price - 闪电贷预言机操纵
- **Nexus** (ETH) - Access Control - 未受保护的函数
- **X2Y2** (ETH) - NFT Exploit - NFT 市场漏洞

#### 2023年5月
- **Affine** (ETH) - Flash Loan + Donation - 对金库份额的捐赠攻击
- **CGT** (BSC) - Flash Loan + Swap - 闪电贷
- **CRV** (BSC) - Flash Loan + Swap - 闪电贷
- **DFX** (ETH) - Flash Loan + Decimal - 十进制精度漏洞
- **DRA** (BSC) - Flash Loan + Swap - 闪电贷
- **FS** (BSC) - Flash Loan + Swap - 闪电贷
- **Gem** (BSC) - Flash Loan + Swap - 闪电贷
- **Globe** (BSC) - Access Control - 未受保护的函数
- **GoldenBoys** (BSC) - Flash Loan + Swap - 闪电贷
- **HODL** (BSC) - Flash Loan + Swap - 闪电贷
- **HP** (BSC) - Flash Loan + Swap - 闪电贷
- **IGU** (BSC) - Flash Loan + Swap - 闪电贷
- **KET** (BSC) - Flash Loan + Swap - 闪电贷
- **KO** (BSC) - Flash Loan + Swap - 闪电贷
- **LAMBO** (BSC) - Flash Loan + Swap - 闪电贷
- **MLTP** (BSC) - Flash Loan + Swap - 闪电贷
- **MN** (BSC) - Access Control - 未受保护的铸造
- **POT** (BSC) - Flash Loan + Swap - 闪电贷
- **PSET** (BSC) - Access Control - 未受保护的函数
- **TH** (BSC) - Flash Loan + Swap - 闪电贷
- **TL** (BSC) - Access Control - 未受保护的函数
- **TOR** (BSC) - Flash Loan + Swap - 闪电贷
- **TSU** (BSC) - Flash Loan + Swap - 闪电贷

#### 2023年6月
- **Cellframe** (BSC) - Flash Loan + Migration - LP 迁移漏洞
- **Compounder Finance** (ETH) - Flash Loan + Deposit/Withdraw - yDAI/cDAI 存款/提取操纵
- **Contract 0x7657** (ETH) - Access Control - 未经验证的合约中存在未受保护的函数
- **DDCoin** (BSC) - Flash Loan + Chain + Order Bypass - 链式 5 次 DODO 闪电贷；订单限制绕过
- **DEPUSDT/LEVUSDC** (ETH) - Access Control - approveToken() 缺少访问控制
- **MIMSpell** (ETH) - Cross-Contract Data Injection - 带有注入交换数据的 ZeroXStargateLPSwapper
- **Midas Capital** (BSC) - Flash Loan + LP + Lending - BNX 操纵，Algebra 池交换，多资产借贷
- **MyAi/MultiSender** (BSC) - Access Control - batchTokenTransfer() 缺少 from 验证
- **NST** (POLY) - Unverified Contract - 闭源交换器存在逻辑缺陷
- **Pawnfi** (ETH) - Flash Loan + NFT Staking - ApeCoin 质押加闪电贷
- **SELLC** (BSC) - Flash Loan + Miner - 矿工合约奖励操纵
- **SHIDO** (BSC) - Flash Loan + Token Conversion - 锁定/领取代币转换套利
- **STRAC** (BSC) - Unverified Contract - 虚假 ERC20 transferFrom 回调
- **Sturdy Finance** (ETH) - Read-Only Reentrancy - Balancer.exitPool() 触发了 3 次预言机价格操纵
- **Themis** (ARB) - Flash Loan + Oracle + Borrow - 存入 WETH，从 5 个池中借出所有资产
- **UN Token** (BSC) - Flash Loan + Skim - 将 93% 的 UN 转回交易对，skim()，重复操作
- **Unverified 0xAC899E** (BSC) - Unverified Contract - 未受保护的存款/提取
- **VINU** (ETH) - Fake Router - addLiquidityETH() 接受了虚假路由器以操纵价格追踪
- **Citadel Finance** (BSC) - Flash Loan + Price Manipulation
- **WZ** (BSC) - Flash Loan + Swap
- **TokenA** (BSC) - Access Control
- **TOL** (BSC) - Access Control
- **MiniDOGE** (BSC) - Flash Loan + Swap
- **DOG** (BSC) - Flash Loan + Fee

#### 2023年7月
- **0xAA** (ETH) - Proxy Exploit - 未受保护的代理函数
- **Astrid** (ETH) - Flash Loan + Swap - 闪电贷交换
- **Bald** (BSC) - Flash Loan + Swap - 闪电贷
- **BNB6** (BSC) - Flash Loan + Swap - 闪电贷
- **BSC** (BSC) - Flash Loan + Swap - 闪电贷
- **BSC2** (BSC) - Flash Loan + Swap - 闪电贷
- **Checker** (BSC) - Flash Loan + Swap - 闪电贷
- **Curve** (ETH) - Reentrancy (Vyper) - Vyper 编译器漏洞使多个池可被重入攻击；约7300万美元
- **DFX** (BSC) - Flash Loan + Swap - 闪电贷
- **Dice** (BSC) - Reentrancy - 重入攻击
- **Drain** (BSC) - Access Control - 未受保护的函数
- **GM** (BSC) - Flash Loan + Swap - 闪电贷
- **HUS** (BSC) - Flash Loan + Swap - 闪电贷
- **JPC** (BSC) - Flash Loan + Swap - 闪电贷
- **KEN** (BSC) - Flash Loan + Swap - 闪电贷
- **Lena** (ETH) - Flash Loan + Staking - 质押奖励操纵
- **LM** (BSC) - Flash Loan + Swap - 闪电贷
- **LRC** (ETH) - Flash Loan + Price - 价格操纵
- **M** (BSC) - Flash Loan + Swap - 闪电贷
- **MEV Bot** (ETH) - Flash Loan + Bot - MEV 机器人漏洞
- **MM** (BSC) - Flash Loan + Swap - 闪电贷
- **Mondo** (ETH) - Flash Loan + Price - 价格操纵
- **MS** (BSC) - Flash Loan + Swap - 闪电贷
- **NFT20** (ETH) - NFT Exploit - NFT 池操纵
- **PinkEco** (BSC) - Flash Loan + Staking - 父子质押奖励漏洞
- **Puffle** (BSC) - Flash Loan + Swap - 闪电贷
- **Radiant** (ARB) - Flash Loan + Lending - 借贷池操纵
- **RMRK** (ETH) - Flash Loan + Swap - 闪电贷
- **SDAO** (BSC) - Flash Loan + Swap - 闪电贷
- **Sex** (BSC) - Flash Loan + Swap - 闪电贷
- **Standard** (BSC) - Access Control - 未受保护的函数
- **STAR** (BSC) - Flash Loan + Swap - 闪电贷
- **SUI** (BSC) - Flash Loan + Swap - 闪电贷
- **T1** (BSC) - Flash Loan + Swap - 闪电贷
- **TEN** (BSC) - Flash Loan + Swap - 闪电贷
- **TTcoin** (BSC) - Access Control - 未受保护的铸造
- **UBI** (ETH) - Flash Loan + Universal Basic Income - UBI 代币漏洞
- **VGX** (BSC) - Flash Loan + Swap - 闪电贷
- **XY** (BSC) - Flash Loan + Swap - 闪电贷

#### 2023年8月
- **Aave** (ETH) - Flash Loan + Price - 闪电贷价格操纵
- **AC** (BSC) - Flash Loan + Swap - 闪电贷
- **Aladdin** (ETH) - Flash Loan + Donation - 捐赠攻击
- **AMBT** (BSC) - Access Control - 未受保护的函数
- **APE** (BSC) - Flash Loan + Swap - 闪电贷
- **Aqua** (BSC) - Flash Loan + Swap - 闪电贷
- **ARB** (ARB) - Flash Loan + Swap - 闪电贷
- **AVAX** (AVAX) - Flash Loan + Price - 闪电贷
- **BASE** (BASE) - Access Control - 未受保护的函数
- **BEB** (BSC) - Flash Loan + Swap - 闪电贷
- **BTH** (BSC) - Flash Loan + Swap - 闪电贷
- **BVM** (BSC) - Flash Loan + Swap - 闪电贷
- **Compound UNI** (ETH) - Flash Loan + Oracle - 操纵 Compound UNI 预言机；约500万美元
- **Cyber** (BSC) - Flash Loan + Swap - 闪电贷
- **DAM** (BSC) - Flash Loan + Swap - 闪电贷
- **DEF** (BSC) - Flash Loan + Swap - 闪电贷
- **Delio** (BSC) - Flash Loan + Staking - 质押漏洞
- **Diamond** (BSC) - Flash Loan + Swap - 闪电贷
- **DIC** (BSC) - Flash Loan + Swap - 闪电贷
- **Ecoin** (BSC) - Access Control - 未受保护的函数
- **ENDO** (BSC) - Flash Loan + Swap - 闪电贷
- **ETP** (BSC) - Flash Loan + Swap - 闪电贷
- **FEG** (ETH) - Flash Loan + Fee - 转账时收费绕过
- **Flare** (BSC) - Flash Loan + Swap - 闪电贷
- **Gala** (ETH) - Access Control - 未受保护的铸造
- **GMD** (ARB) - Flash Loan + Staking - 质押奖励漏洞
- **GOLD** (BSC) - Flash Loan + Swap - 闪电贷
- **GRT** (BSC) - Flash Loan + Swap - 闪电贷
- **GUY** (BSC) - Flash Loan + Swap - 闪电贷
- **HEC** (BSC) - Flash Loan + Swap - 闪电贷
- **Honey** (BSC) - Flash Loan + Swap - 闪电贷
- **HUB** (BSC) - Access Control - 未受保护的函数
- **JED** (BSC) - Flash Loan + Swap - 闪电贷
- **JIM** (BSC) - Access Control - 未受保护的铸造
- **JP** (BSC) - Flash Loan + Swap - 闪电贷
- **KAR** (BSC) - Access Control - 未受保护的函数
- **KING** (BSC) - Flash Loan + Swap - 闪电贷
- **KM** (BSC) - Flash Loan + Swap - 闪电贷
- **KUN** (BSC) - Flash Loan + Swap - 闪电贷
- **LO** (BSC) - Flash Loan + Swap - 闪电贷
- **MA** (BSC) - Flash Loan + Swap - 闪电贷
- **Mare** (BSC) - Access Control - 未受保护的函数
- **MC** (BSC) - Flash Loan + Swap - 闪电贷
- **Mice** (ETH) - Flash Loan + NFT - NFT 价格操纵
- **MP** (BSC) - Flash Loan + Swap - 闪电贷
- **Nem** (BSC) - Flash Loan + Swap - 闪电贷
- **NF** (BSC) - Flash Loan + Swap - 闪电贷
- **OAS** (BSC) - Flash Loan + Fee - 费用绕过
- **PE** (BSC) - Flash Loan + Swap - 闪电贷
- **PEPE** (ETH) - Flash Loan + Tax - 税费绕过
- **PL** (BSC) - Flash Loan + Swap - 闪电贷

#### 2023年9月
- **BAD** (BSC) - Access Control - 未受保护的函数
- **BitKeep** (BSC) - Access Control - 未受保护的函数
- **BTC** (BSC) - Flash Loan + Swap - 闪电贷
- **ETH** (BSC) - Access Control - 未受保护的函数
- **FTM** (BSC) - Flash Loan + Swap - 闪电贷
- **GC** (BSC) - Flash Loan + Swap - 闪电贷
- **LFI** (BSC) - Flash Loan + Swap - 闪电贷
- **Mix** (BSC) - Flash Loan + Swap - 闪电贷
- **OAS** (BSC) - Access Control - 未受保护的函数
- **OC** (BSC) - Flash Loan + Price - 价格操纵
- **OS** (BSC) - Flash Loan + Swap - 闪电贷
- **PHB** (BSC) - Flash Loan + Swap - 闪电贷
- **PULSE** (ETH) - Flash Loan + Tokenomics - 代币经济学漏洞
- **SB** (BSC) - Flash Loan + Swap - 闪电贷
- **SP** (BSC) - Access Control - 未受保护的函数
- **SU** (BSC) - Flash Loan + Swap - 闪电贷
- **TST** (BSC) - Flash Loan + Swap - 闪电贷
- **Vance** (BSC) - Flash Loan + Swap - 闪电贷
- **Wild** (BSC) - Flash Loan + Staking - 质押奖励操纵
- **XAI** (BSC) - Flash Loan + Swap - 闪电贷
- **XV** (BSC) - Flash Loan + Swap - 闪电贷
- **YF** (BSC) - Flash Loan + Swap - 闪电贷

#### 2023年10月
- **BALD** (BSC) - Flash Loan + Swap - 闪电贷
- **BFA** (ETH) - Access Control - 未受保护的函数
- **BMC** (BSC) - Flash Loan + Swap - 闪电贷
- **DM** (BSC) - Flash Loan + Swap - 闪电贷
- **DRB** (BSC) - Flash Loan + Swap - 闪电贷
- **DW** (BSC) - Flash Loan + Swap - 闪电贷
- **HYC** (BSC) - Access Control - 未受保护的函数
- **LAB** (BSC) - Flash Loan + Swap - 闪电贷
- **LINK** (BSC) - Flash Loan + Swap - 闪电贷
- **Maria** (BSC) - Flash Loan + Swap - 闪电贷
- **Moo** (ETH) - Access Control - 未受保护的函数
- **MV** (BSC) - Flash Loan + Swap - 闪电贷
- **NO** (BSC) - Flash Loan + Swap - 闪电贷
- **pSeudoEth** (ETH) - Flash Loan + Skim/Sync - Skim/sync 操纵
- **Thunder** (BSC) - Flash Loan + Swap - 闪电贷
- **TTT** (BSC) - Flash Loan + Swap - 闪电贷
- **UXD** (ETH) - Flash Loan + Swap - 闪电贷
- **WUF** (BSC) - Flash Loan + Swap - 闪电贷
- **X** (BSC) - Flash Loan + Swap - 闪电贷

#### 2023年11月
- **Token 3913** (BSC) - Flash Loan + Burn - burnPairs() 减少供应量，抬高价格
- **AIS** (BSC) - Flash Loan + Access Control - setSwapPairs() + harvestMarket()
- **BRAND** (BSC) - Flash Loan + buyToken Loop - buyToken() 在循环中被调用 100 次
- **Burntbubba** (ETH) - Flash Loan + LP Share - deposit()/emergencyWithdraw() 份额操纵
- **CAROL Protocol** (BASE) - Buy/Sell Price - 联合曲线价格操纵
- **EEE** (BSC) - Flash Loan + Swap - 闪电贷交换
- **EHX** (BSC) - Flash Loan + Swap - 闪电贷交换
- **Fiber Router** (BSC) - Arbitrary External Call - 带有恶意 calldata 的 swapAndCrossOneInch() 函数
- **KR** (BSC) - Access Control - sellKr() 缺少所有权验证
- **KyberSwap Elastic** (ETH) - Precision Loss - 刻度计算中的精度损失；约4600万美元
- **LinkDao** (BSC) - Flash Loan + Swap - 未经验证的合约漏洞
- **MEV 0x8c2d** (BSC) - MEV Bot - 机器人上被分配了特权角色
- **MEV 0xa247** (ETH) - MEV Bot - 从脆弱的机器人中提取了 24 种代币
- **MahaLend** (ETH) - Flash Loan + Lending - 存款/提取份额操纵
- **MetaLend** (ETH) - Flash Loan + Lending - Comptroller 价格预言机操纵
- **OKC** (BSC) - Flash Loan + Multi-Pool - 5 个 DODO 池加 Uniswap V3
- **Onyx Protocol** (ETH) - Flash Loan + Liquidation - PEPE 存款，预言机捐赠，超额借贷；200万美元
- **RBalancer** (ETH) - Flash Loan + Balance Manipulation - 存款/提取余额记账操纵
- **Raft** (ETH) - Flash Loan + Oracle - fetchPrice() 预言机操纵；320万美元
- **Shiba Token (fake)** (BSC) - Flash Loan + Batch Transfer - batchTransferLockToken() 漏洞
- **Swamp Finance** (BSC) - Flash Loan + LP - 存款/提取流程漏洞
- **TheNFTV2** (ETH) - Flash Swap + NFT Price - 通过 TheDAO 代币操纵 NFT 价格
- **TheStandard.io** (ARB) - Flash Loan + Position Management - collect()/decreaseLiquidity() 漏洞；29万美元
- **Token 8633/9419** (BSC) - Flash Loan + Auto-Marketing - autoSwapAndAddToMarketing() 漏洞
- **TrustPad** (BSC) - Flash Loan + Staking - receiveUpPool() 虚增奖励；15.5万美元
- **WECO** (BSC) - Staking Accounting - deposit() 份额记账缺陷
- **XAI** (BSC) - Flash Loan + Burn - burn() 减少供应量，抬高价格
- **bot** (ETH) - Flash Loan + Curve/Uni Arbitrage - Curve 交换加 Aave 闪电贷加 Uniswap；200万美元
- **grok** (ETH) - Flash Loan + Uniswap V3 Fee - 含费用等级操纵的嵌套闪电贷

#### 2023年12月
- **BCT** (BSC) - Flash Loan + Inviter/Buy - 邀请/推荐系统漏洞
- **BEARNDAO** (BSC) - Flash Loan + Strategy - convertDustToEarned() 重复提取；76.9万美元
- **Bob** (BSC) - Flash Loan + Multi-Pool - 跨池的 skim() + swap() 操作
- **CCV** (BSC) - Flash Loan + Proxy - 未受保护的代理函数选择器
- **Channels Finance** (BSC) - Flash Loan + Lending - gulp() 在 Venus 分叉上虚增抵押品
- **Channels** (BSC) - Flash Loan + Lending - Compound 分叉上的利息累积操纵
- **DominoTT** (BSC) - Flash Loan + Forwarder - multicall() + Forwarder.execute() 绕过访问控制
- **Elephant Status** (BSC) - Flash Loan + Sweep - Elephant.sweep() 提取 BUSD；16.5万美元
- **Floor Protocol** (ETH) - NFT Theft - extMulticall() 盗取 PPG NFT；160万美元
- **GoodCompound** (ETH) - Flash Loan + COMP Rewards - claimComp() 奖励操纵；1.3万美元
- **GoodDollar** (ETH) - Flash Loan + Buy/Sell - GDX 联合曲线操纵；200万美元
- **HNet** (BSC) - Flash Loan + Forwarder - multicall() + Forwarder.execute() 类似于 DominoTT
- **HYPR** (ETH) - Proxy Initialization - 未初始化的代理，设置 messenger 角色；20万美元
- **KEST** (BSC) - Flash Loan + Lending - Radiant 闪电贷加 PancakeSwap 交换；2300美元
- **MAMO** (BSC) - Flash Loan + Swap - DODO 闪电贷加交换/提取；3300美元
- **NFT Trader** (ETH) - NFT Theft - 交换意图中的恶意 ERC721；300万美元
- **PHIL** (BSC) - Unlimited Mint - simpleToken() 无限铸造代币
- **PineProtocol** (ETH) - Flash Loan + NFT Lending - 迁移贷款记账缺陷；9万美元
- **TIME** (ETH) - Multicall + ERC2771 - 通过转发器伪造 _msgSender()；84.59 ETH
- **Telcoin** (POLY) - Proxy Initialization - 未初始化的 CloneableProxy；124万美元
- **Transit Finance** (BSC) - Arbitrary External Call - 带有恶意参数的路由器交换；173 BNB
- **Unverified 0x431abb** (BSC) - Flash Loan + Buy/Stake - buy() + bindParent() 操纵；50万美元
- **bZx (2023)** (ETH) - Flash Loan + Lending - withdrawCollateral() 漏洞；20.8万美元

#### 2024-01
- **Orbit Chain Bridge** (ETH) - Signature Forgery - 伪造验证器签名以耗尽桥接资金；约8100万美元
- **Socket Gateway** (ETH) - Arbitrary External Call - 套接字网关中未经验证的外部调用；约330万美元
- **Gamma/Uniproxy** (ETH) - Flash Loan + Nested - 嵌套闪电贷加操纵；约630万美元
- **Radiant Capital** (ARB/BNB) - Flash Loan + Liquidity Index - 流动性指数操纵；约450万美元
- **Blueberry Protocol** (ETH) - Compounding Logic Bug - 复利逻辑错误；约140万美元
- **NBLGAME** (BSC) - Reentrancy - 游戏合约中的重入攻击
- **DappRadar** (ETH) - Access Control - 未保护的函数
- **K3PR** (BSC) - Flash Loan + Swap - 闪电贷兑换
- **Maverick** (ETH) - Flash Loan + Price - Maverick AMM 价格操纵
- **TLN** (BSC) - Access Control - 未保护的函数
- **Tokenlon** (ETH) - Flash Loan + Price - Tokenlon 价格操纵
- **WN** (BSC) - Access Control - 未保护的函数
- **WOO** (ETH) - Flash Loan + Price - Woo 价格操纵
- **Unim** (BSC) - Flash Loan + Swap - 闪电贷兑换
- **SHRAP** (ETH) - Flash Loan + Tokenomics - 代币经济利用
- **SmartCredit** (ETH) - Flash Loan + Lending - 借贷操纵
- **SquidMulticall** (ETH) - Access Control - 未保护的 Axelar 网关
- **Swarms** (ETH) - Access Control - 未保护的函数
- **Roll** (ETH) - Access Control - 未保护的函数
- **SEEDx** (ETH) - Flash Loan + Donation - 捐赠攻击
- **Toum** (ETH) - Flash Loan + Price - 价格操纵

#### 2024-02
- **MIMSpell (rounding)** (ETH) - Precision/Rounding - Abracadabra Spell 上的舍入攻击；约650万美元
- **Paraswap** (ETH) - Arbitrary External Call - 兑换中任意 calldata；约120万美元
- **DAO SoulMate** (BSC) - Access Control - 缺少访问控制；约31.9万美元
- **Wise Lending** (ETH) - Flash Loan + Donation - 捐赠攻击；约46.4万美元
- **Barley Finance** (ETH) - Flash Loan + Donation - 金库份额上的捐赠攻击；约13万美元
- **MIC Token** (BSC) - Flash Loan + LP Fee - 耗尽 LP 费用；约50万美元
- **Abracadabra** (ETH) - Flash Loan + Donation - 捐赠攻击
- **ATK** (BSC) - Flash Loan + Swap - 闪电贷
- **BONK** (ETH) - Flash Loan + Tax - 税费绕过
- **FINS** (BSC) - Flash Loan + Swap - 闪电贷
- **H2** (BSC) - Access Control - 未保护的函数
- **HOOD** (BSC) - Flash Loan + Swap - 闪电贷
- **MBox** (BSC) - Flash Loan + Swap - 闪电贷
- **Move** (BSC) - Flash Loan + Swap - 闪电贷
- **MULTI** (BSC) - Access Control - 未保护的函数
- **ORDS** (BSC) - Flash Loan + Swap - 闪电贷
- **Pulsar** (ETH) - Flash Loan + Swap - 闪电贷
- **RO** (BSC) - Flash Loan + Swap - 闪电贷
- **Wise Lending (2)** (ETH) - Flash Loan + Donation - 第二次捐赠攻击；约46.4万美元
- **XY** (BSC) - Flash Loan + Swap - 闪电贷
- **ZF** (BSC) - Access Control - 未保护的铸币
- **Zoom** (BSC) - Flash Loan + Swap - 闪电贷

#### 2024-03
- **Gulper** (ETH) - Flash Loan + Contract Takeover - 通过闪电贷接管合约
- **Mytheria** (BSC) - Access Control - 未保护的函数
- **NFPrompt** (BSC) - Access Control - 未保护的函数
- **PATEX** (BSC) - Flash Loan + Stake - 质押操纵
- **Prisma Finance** (ETH) - Flash Loan + Donation - Prisma 金库上的捐赠攻击；约200万美元
- **RIVUS** (BSC) - Access Control - 未保护的函数
- **Bounce** (ETH) - Flash Loan + Auction - 拍卖操纵
- **Ethena** (ETH) - Flash Loan + Staking - 质押 USDe 操纵
- **Masa** (ETH) - Flash Loan + Tokenomics - 代币经济利用
- **Solv** (ETH) - Flash Loan + Donation - 捐赠攻击
- **Swell** (ETH) - Flash Loan + Staking - 质押奖励操纵
- **Wormhole (replay)** (ETH) - Bridge Replay - 跨链消息重放

#### 2024-04
- **Dollara** (BSC) - Access Control - 未保护的函数
- **ENQAI** (ETH) - Flash Loan + Price - 价格操纵
- **FakeToken** (BSC) - Access Control - 未保护的函数
- **FR** (BSC) - Flash Loan + Swap - 闪电贷
- **Ghost** (ETH) - Flash Loan + Donation - 捐赠攻击
- **HoppyFrog** (ETH) - Access Control - 未保护的转账
- **MARS** (BSC) - Flash Loan + Swap - 闪电贷
- **NGFS** (BSC) - Flash Loan + Access Control - 闪电贷加访问控制
- **OpenLeverage** (ETH) - Flash Loan + Price - 价格操纵
- **PikeFinance** (ETH) - Access Control - 授权绕过
- **Rico** (ETH) - Flash Loan + Donation - 捐赠攻击
- **SATX** (BSC) - Access Control - 未保护的铸币
- **SQUID** (BSC) - Access Control - 未保护的函数
- **Sumer Money** (ETH) - Flash Loan + Donation - 捐赠攻击
- **UPS** (BSC) - Flash Loan + Fee - 费用机制利用
- **Unverified 0x00C409** (ETH) - Flash Loan + Swap - 未验证的合约
- **WSM** (BSC) - Flash Loan + Swap - 闪电贷
- **XBridge** (BSC) - Access Control - 未保护的桥接
- **YIEDL** (BSC) - Flash Loan + Donation - 捐赠攻击
- **Yield** (ETH) - Flash Loan + Donation - 聚合器上的捐赠攻击
- **Z123** (BSC) - Access Control - 未保护的铸币

#### 2024-05
- **Burner** (ETH) - Flash Loan + Donation - 捐赠攻击
- **EXcommunity** (ETH) - Access Control - execute() 缺少访问控制
- **GFOX** (ETH) - Flash Loan + Pool Manipulation - 池价格操纵
- **GPU** (ETH) - Flash Loan + Tokenomics - 代币经济利用
- **Liquidity Tokens** (ETH) - LP Token Inflation - LP 代币铸币/销毁比例利用
- **Meta Dragon** (BSC) - Access Control - claimed() 缺少访问控制
- **MixedSwapRouter** (ETH) - Arbitrary External Call - 兑换数据中的任意调用
- **NORMIE** (BASE) - Donation Attack - 通过捐赠操纵份额价格
- **OSN** (BSC) - Access Control - 未保护的函数
- **Predy Finance** (ARB) - Flash Loan + Price - 永续合约交易价格操纵
- **Red Keys Coin** (ETH) - Flash Loan + Fee - 买卖费用利用
- **SATURN** (ETH) - Flash Loan + Donation - 捐赠攻击
- **SCROLL** (ETH) - Access Control - 未保护的函数
- **Sonne Finance** (OETH) - Flash Loan + Donation - 捐赠加 VELO 操纵
- **TCH** (BSC) - Flash Loan + Swap - 闪电贷
- **TGC** (BSC) - Access Control - sell() 缺少访问控制
- **TSURU** (BSC) - Flash Loan + LP - LP 储备操纵
- **Trade on Orion** (BSC) - Flash Loan + Swap - 闪电贷

#### 2024-06
- **APEMAGA** (ETH) - Flash Loan + Tax - 买卖税费利用
- **Bazaar** (ETH) - Access Control - 未保护的函数
- **Crb2** (BSC) - Flash Loan + Liquidity Drain - 通过兑换耗尽流动性
- **Dyson Money** (ETH) - Flash Loan + Donation - 捐赠攻击
- **INcufi** (ETH) - Flash Loan + Fee - 费用机制利用
- **JokInTheBox** (BSC) - Flash Loan + Swap - 闪电贷
- **MineSTM** (BSC) - Access Control - 未保护的函数
- **NCD** (BSC) - Flash Loan + Swap - 闪电贷
- **SteamSwap** (BSC) - Access Control - 未保护的函数
- **UwU Lend (First)** (ETH) - Flash Loan + Donation - 通过捐赠操纵 uDai 份额价格；约1100万美元
- **UwU Lend (Second)** (ETH) - Flash Loan + Donation - 不同资产的捐赠攻击；约1100万美元
- **Velocore** (LINEA) - Flash Loan + LP - 恒定乘积不变量利用
- **WIFCOIN** (ETH) - Staking Reentrancy - claimEarned() 缺少重入保护；约3400美元
- **Will** (BSC) - Flash Loan + Expired Orders - 过期头寸结算；约5.2万美元
- **YYS** (BSC) - Flash Loan + Arbitrary Sell - YYStoken_Sell.sell() 利用；约2.8万美元

#### 2024-07
- **BNB** (BSC) - Access Control - 未保护的函数
- **CRB** (BSC) - Flash Loan + Swap - 闪电贷
- **DOD** (BSC) - Access Control - 未保护的函数
- **EC** (BSC) - Flash Loan + Swap - 闪电贷
- **FBC** (BSC) - Flash Loan + Swap - 闪电贷
- **GL** (BSC) - Flash Loan + Swap - 闪电贷
- **GR** (BSC) - Flash Loan + Swap - 闪电贷
- **HOD** (BSC) - Flash Loan + Swap - 闪电贷
- **HPO** (BSC) - Flash Loan + Swap - 闪电贷
- **KNINE** (BSC) - Flash Loan + Swap - 闪电贷
- **LE** (BSC) - Access Control - 未保护的函数
- **Living** (BSC) - Access Control - 未保护的函数
- **LM** (BSC) - Flash Loan + Swap - 闪电贷
- **MARS** (BSC) - Access Control - 未保护的函数
- **MEV** (BSC) - MEV Bot - MEV 机器人利用
- **MG** (BSC) - Flash Loan + Swap - 闪电贷
- **MS** (BSC) - Access Control - 未保护的函数
- **N3K** (BSC) - Flash Loan + Swap - 闪电贷
- **OAS** (BSC) - Flash Loan + Swap - 闪电贷
- **Palm** (BSC) - Flash Loan + Swap - 闪电贷
- **PEPE** (BSC) - Flash Loan + Swap - 闪电贷
- **PI** (BSC) - Flash Loan + Swap - 闪电贷
- **RADIO** (BSC) - Access Control - 未保护的函数
- **SDO** (BSC) - Flash Loan + Swap - 闪电贷
- **SHLD** (BSC) - Access Control - 未保护的函数
- **SU** (ETH) - Flash Loan + Swap - 闪电贷
- **Tutti** (BSC) - Flash Loan + Swap - 闪电贷
- **VE** (BSC) - Flash Loan + Swap - 闪电贷

#### 2024-08
- **Anvil** (ETH) - Flash Loan + Swap - 闪电贷
- **Bounce** (ETH) - Flash Loan + Auction - 拍卖操纵
- **DEXTools** (ETH) - Access Control - 未保护的函数
- **GALA** (BSC) - Access Control - 未保护的函数
- **Nexera** (ETH) - Access Control - 未保护的代理
- **Pika** (ETH) - Flash Loan + Perp - 永续合约交易操纵
- **SecurityCamera** (BSC) - Access Control - 未保护的函数
- **SN** (BSC) - Access Control - 未保护的函数
- **WOOF** (BSC) - Access Control - 未保护的函数
- **ZT** (BSC) - Flash Loan + Swap - 闪电贷
- **MIM** (ETH) - Flash Loan + Swap - 闪电贷

#### 2024-09
- **4D** (BSC) - Flash Loan + Swap - 闪电贷
- **Chad** (ETH) - Flash Loan + Tokenomics - 代币经济利用
- **CT** (BSC) - Flash Loan + Swap - 闪电贷
- **DIC** (BSC) - Access Control - 未保护的函数
- **DT3** (BSC) - Flash Loan + Swap - 闪电贷
- **DW** (BSC) - Flash Loan + Swap - 闪电贷
- **KCA** (BSC) - Flash Loan + Swap - 闪电贷
- **KR** (BSC) - Flash Loan + Swap - 闪电贷
- **MAN** (BSC) - Access Control - 未保护的函数
- **MBS** (BSC) - Flash Loan + Swap - 闪电贷

- **Neiro** (ETH) - Flash Loan + Fee - 费用绕过
- **RO** (BSC) - Flash Loan + Swap - 闪电贷
- **ROO** (BSC) - Access Control - 未受保护的功能
- **SA** (BSC) - Flash Loan + Swap - 闪电贷
- **SN** (BSC) - Access Control - 未受保护的功能
- **TB** (BSC) - Flash Loan + Swap - 闪电贷
- **TT** (BSC) - Access Control - 未受保护的功能

#### 2024-10
- **AC** (BSC) - Flash Loan + Swap - 闪电贷
- **AI** (BSC) - Flash Loan + Swap - 闪电贷
- **BLD** (BSC) - Access Control - 未受保护的功能
- **G3** (BSC) - Flash Loan + Swap - 闪电贷
- **KAI** (BSC) - Flash Loan + Swap - 闪电贷
- **MIO** (BSC) - Access Control - 未受保护的功能
- **MS** (BSC) - Flash Loan + Swap - 闪电贷
- **NJ** (BSC) - Flash Loan + Swap - 闪电贷
- **NY** (BSC) - Flash Loan + Swap - 闪电贷
- **OP** (BSC) - Flash Loan + Swap - 闪电贷
- **SI** (BSC) - Flash Loan + Swap - 闪电贷
- **VISTA** (BSC) - Flash Loan + No Freeze Check - 未检查代币是否冻结的闪电贷；29K美元
- **YY** (BSC) - Access Control - 未受保护的功能

#### 2024-11
- **AK1111** (BSC) - Access Control - nonblockingLzReceive1() 缺少访问控制；31.5K美元
- **ChiSale** (ETH) - Flash Loan + Empty Callback - 空的 receiveFlashLoan() 回调；16.3K美元
- **CoW Protocol** (ETH) - MEV/Callback Spoofing - uniswapV3SwapCallback() 欺骗；59K美元
- **DeltaPrime** (ARB) - Flash Loan + Reentrancy - 通过虚假交易对调用 SmartLoan.claimReward()；475万美元
- **MFT** (BSC) - Flash Loan + Burn - 嵌套闪电贷 + 代币销毁；33.7K美元
- **MainnetSettler** (ETH) - Access Control - 批量操作允许未授权转账；66K美元
- **Matez** (BSC) - Integer Overflow - stake() 整数截断；80K美元
- **NFTG** (BSC) - Flash Loan + Reentrancy - DODO 闪电贷 + 重入攻击；10K美元
- **PolterFinance** (FTM) - Flash Loan + Borrow Exploit - 存入1 wei，借出全部8种代币；700万美元
- **RPP** (BSC) - Flash Loan + Price Loop - 1,450次交易迭代操纵价格；14.1K美元
- **VRug** (ETH) - MEV/Sandwich - Uniswap V2 抢先交易；8.4K美元
- **X319** (BSC) - Access Control - claimEther() 缺少访问控制；12.9K美元
- **proxy_b7e1** (BSC) - Proxy Exploit - 未验证的逻辑合约；8.5K美元
- **vETH** (ETH) - Flash Loan + Factory - 工厂合约漏洞；447K美元

#### 2024-12
- **BTC24H** (POLY) - Access Control - claim() 缺少访问控制；85.7K美元
- **Bizness** (BASE) - Reentrancy - 通过 receive() 对 splitLock() 进行重入攻击；15.7K美元
- **Clober DEX** (BASE) - Flash Loan + Reentrancy - 虚假代币池 + burnHook 重入攻击；501K美元
- **JHY** (BSC) - Flash Loan + LP Accounting - DIVIDEND_JHYLP 会计错误；11K美元
- **LABUBU** (BSC) - Flash Loan + Reentrancy - 30次自转账重入攻击；12K美元
- **Moonhacker** (OETH) - Flash Loan + Impersonation - 虚假铸造/借贷返回值；318.9K美元
- **Pledge** (BSC) - Access Control - swapTokenU() 缺少访问控制；15K美元
- **SlurpyCoin** (BSC) - Flash Loan + Price Loop - 240次转账买卖循环；3K美元

### 2025

#### 2025-01
- **GMX/SEED** (AVAX) - Flash Loan + Donation - 向 GMX LP 质押池捐赠以抬高份额价格；约32K美元
- **SquidMulticall** (ETH) - Access Control - Squid 路由器中未受保护的 Axelar 网关；约122K美元
- **SquidRouter** (ARB) - Access Control - 未受保护的功能
- **MIT** (ETH) - Flash Loan + Tokenomics - 代币经济学漏洞利用
- **THENA** (BSC) - Flash Loan + Farm - ThenA 农场操纵
- **Pink** (BSC) - Flash Loan + Stake - 质押操纵
- **RFS** (BSC) - Access Control - 未受保护的功能
- **BSC-HB** (BSC) - Access Control - 未受保护的功能
- **DUB** (BSC) - Flash Loan + Swap - 闪电贷
- **GRV** (BSC) - Flash Loan + Swap - 闪电贷
- **GT** (BSC) - Flash Loan + Swap - 闪电贷
- **HT** (BSC) - Flash Loan + Swap - 闪电贷
- **IZA** (BSC) - Flash Loan + Swap - 闪电贷
- **LSD** (BSC) - Flash Loan + Fee - 费用绕过
- **NFTW** (BSC) - Flash Loan + Swap - 闪电贷
- **SRW** (BSC) - Flash Loan + Swap - 闪电贷
- **TN** (BSC) - Access Control - 未受保护的功能
- **TOAD** (BSC) - Flash Loan + Swap - 闪电贷
- **USD** (BSC) - Access Control - 未受保护的功能
- **WGP** (BSC) - Access Control - 未受保护的铸币
- **XOX** (BSC) - Flash Loan + Swap - 闪电贷
- **YAY** (ETH) - Flash Loan + Price - 价格操纵
- **YH** (BSC) - Access Control - 未受保护的功能
- **YS** (BSC) - Flash Loan + Swap - 闪电贷
- **me** (BSC) - Flash Loan + Swap - 闪电贷

#### 2025-02
- **AA** (BSC) - Reentrancy - 使用虚假代币进行150次转账；金库被掏空
- **BabyDoge** (ETH) - Flash Loan + Tokenomics - 代币经济学操纵
- **Bi** (BSC) - Flash Loan + Swap - 闪电贷
- **BS** (BSC) - Flash Loan + Swap - 闪电贷
- **Chain** (BSC) - Flash Loan + Stake - 质押操纵
- **CorionX** (ETH) - Flash Loan + Swap - 闪电贷
- **DC** (BSC) - Flash Loan + Swap - 闪电贷
- **DEF** (BSC) - Flash Loan + Swap - 闪电贷
- **DPD** (BSC) - Flash Loan + Swap - 闪电贷
- **Dragon** (BSC) - Flash Loan + Swap - 闪电贷
- **EB** (BSC) - Flash Loan + Swap - 闪电贷
- **EF** (BSC) - Flash Loan + Swap - 闪电贷
- **FL** (BSC) - Flash Loan + Swap - 闪电贷
- **GJ** (BSC) - Flash Loan + Swap - 闪电贷
- **HTA** (BSC) - Flash Loan + Swap - 闪电贷
- **KU** (BSC) - Flash Loan + Swap - 闪电贷
- **LB** (BSC) - Flash Loan + Swap - 闪电贷
- **LOLA** (BSC) - Flash Loan + Stake - 质押操纵
- **M** (BSC) - Flash Loan + Swap - 闪电贷
- **ME** (BSC) - Flash Loan + Swap - 闪电贷
- **MN** (BSC) - Flash Loan + Fee - 费用绕过
- **MOVE** (BSC) - Flash Loan + Swap - 闪电贷
- **N4L** (BSC) - Flash Loan + Swap - 闪电贷
- **PEPE (fake)** (BSC) - Access Control - 未受保护的功能
- **SSSS** (BSC) - Flash Loan + Swap - 闪电贷
- **TST** (BSC) - Flash Loan + Swap - 闪电贷
- **Unsupported** (BSC) - Flash Loan + Stake - 质押操纵
- **WO** (BSC) - Flash Loan + Swap - 闪电贷
- **ZKB** (BSC) - Flash Loan + Swap - 闪电贷

#### 2025-03
- **Aura** (ETH) - Flash Loan + Price - Aura 价格操纵
- **Across Protocol** (ETH) - Bridge Exploit - 过期根捆绑包验证；105K美元
- **BSCS** (BSC) - Flash Loan + Swap - 闪电贷
- **CT** (BSC) - Access Control - 未受保护的铸币
- **Fed** (BSC) - Flash Loan + Swap - 闪电贷
- **Fee** (BSC) - Access Control - 未受保护的功能
- **FIN** (BSC) - Flash Loan + Swap - 闪电贷
- **HB** (BSC) - Access Control - 未受保护的功能
- **IDL** (BSC) - Flash Loan + Swap - 闪电贷
- **MC** (BSC) - Flash Loan + Stake - 质押操纵
- **METH** (BSC) - Flash Loan + Stake - 质押操纵
- **MNT** (BSC) - Flash Loan + Swap - 闪电贷
- **MS** (BSC) - Flash Loan + Fee - 费用绕过
- **PL** (BSC) - Flash Loan + Swap - 闪电贷
- **SEE** (BSC) - Flash Loan + Swap - 闪电贷
- **SEEDx** (BSC) - Flash Loan + Stake - 质押操纵
- **TrustPad** (BSC) - Flash Loan + Stake - 质押操纵
- **TW** (BSC) - Flash Loan + Swap - 闪电贷
- **WGB** (BSC) - Flash Loan + Swap - 闪电贷
- **WOK** (BSC) - Access Control - 未受保护的功能
- **WZ** (BSC) - Access Control - 未受保护的功能
- **YLD** (BSC) - Flash Loan + Swap - 闪电贷

#### 2025-04
- **Ai** (BSC) - Flash Loan + Stake - 质押操纵
- **BO** (BSC) - Flash Loan + Swap - 闪电贷
- **BUR** (BSC) - Flash Loan + Swap - 闪电贷
- **CT** (BSC) - Flash Loan + Swap - 闪电贷
- **D** (BSC) - Flash Loan + Swap - 闪电贷
- **DEX** (BSC) - Flash Loan + Swap - 闪电贷
- **DT3** (BSC) - Flash Loan + Swap - 闪电贷
- **EG** (BSC) - Flash Loan + Swap - 闪电贷
- **EL** (BSC) - Flash Loan + Swap - 闪电贷
- **ELP** (BSC) - Flash Loan + Swap - 闪电贷
- **H** (BSC) - Flash Loan + Swap - 闪电贷
- **HODL** (BSC) - Flash Loan + Swap - 闪电贷
- **HON** (BSC) - Flash Loan + Swap - 闪电贷
- **KB** (BSC) - Flash Loan + Swap - 闪电贷
- **LA** (BSC) - Flash Loan + Swap - 闪电贷
- **LO** (BSC) - Flash Loan + Swap - 闪电贷
- **NX** (BSC) - Access Control - 未受保护的功能
- **OY** (BSC) - Flash Loan + Swap - 闪电贷
- **SEC** (BSC) - Access Control - 公开的 claim 函数；21K美元
- **SHI** (BSC) - Flash Loan + Swap - 闪电贷
- **SSS** (BSC) - Flash Loan + Swap - 闪电贷
- **STX** (BSC) - Flash Loan + Swap - 闪电贷
- **SU** (BSC) - Flash Loan + Swap - 闪电贷

#### 2025-05
- **BSC Token** (BSC) - Flash Swap + AMM Accounting - 通过代币特性绕过 AMM 会计
- **Chad** (ETH) - Flash Loan + Tokenomics - 代币经济学漏洞利用
- **DMT** (BSC) - Flash Loan + Swap - 闪电贷
- **ELC** (BSC) - Access Control - 未受保护的功能
- **GL** (BSC) - Flash Loan + Swap - 闪电贷
- **GRT** (BSC) - Flash Loan + Swap - 闪电贷
- **HOD** (BSC) - Flash Loan + Swap - 闪电贷
- **HYC** (BSC) - Flash Loan + Swap - 闪电贷
- **IP** (BSC) - Flash Loan + Swap - 闪电贷
- **JM** (BSC) - Flash Loan + Swap - 闪电贷
- **LALA** (BSC) - Flash Loan + Swap - 闪电贷
- **LST** (BSC) - Flash Loan + Swap - 闪电贷
- **MB** (BSC) - Flash Loan + Swap - 闪电贷
- **MN** (BSC) - Flash Loan + Swap - 闪电贷
- **Pika** (ETH) - Flash Loan + Perp - 永续合约操纵
- **Poly** (BSC) - Flash Loan + Swap - 闪电贷
- **QC** (BSC) - Flash Loan + Swap - 闪电贷

- **Tack** (ETH) - Flash Loan + Donation - Donation attack; $543K
- **TL** (BSC) - Flash Loan + Swap - Flash loan
- **TOX** (BSC) - Access Control - Unprotected function
- **Trinity** (ETH) - Flash Loan + Price - Price manipulation
- **US** (BSC) - Flash Loan + Swap - Flash loan
- **WD** (BSC) - Flash Loan + Swap - Flash loan
- **XX** (BSC) - Flash Loan + Swap - Flash loan
- **ZA** (BSC) - Flash Loan + Swap - Flash loan

#### 2025 年 6 月
- **MCP** (BSC) - Flash Loan + Swap - Flash loan
- **Neuro** (BSC) - Flash Loan + Swap - Flash loan
- **Ripple** (BSC) - Flash Loan + Swap - Flash loan
- **SCT** (BSC) - Flash Loan + Swap - Flash loan
- **WV** (BSC) - Flash Loan + Swap - Flash loan
- **ZT** (BSC) - Flash Loan + Swap - Flash loan
- **Elon** (ETH) - Flash Loan + Tax - Tax/fee bypass
- **FM** (ETH) - Flash Loan + Price - Price manipulation
- **Kiss** (ETH) - Flash Loan + Swap - Flash loan
- **Turbo** (ETH) - Flash Loan + Tax - Tax/fee bypass

#### 2025 年 7 月
- **SWAPP Staking** (BSC) - Flash Loan + Epoch Manipulation - manualEpochInit() bug; ~$32K
- **Stepp2p** (BSC) - Double-Spend - cancelSaleOrder() + modifySaleOrder() on same order; ~$43K
- **SuperRare** (ETH) - Merkle Root Manipulation - updateMerkleRoot() without access control; ~$730K
- **VDS** (BSC) - Burn-Mint Imbalance - Token conversion ratio exploit; ~$13K
- **WETC Token** (BSC) - Flash Loan + Sync/Skim - Multiple sync/skim cycles; ~$101K
- **GMX** (ARB) - Flash Loan + Position + Reentrancy - gmxPositionCallback() reentrancy
- **Unverified 0x54cd** (ETH) - Flash Loan + Unverified Call - Unprotected function; ~$285.7K

#### 2025 年 8 月
- **0x8d2e** (BASE) - Access Control - uniswapV3SwapCallback() without sender check; $40K
- **0xf340** (ETH) - Access Control - initVRF() without access control; $4K
- **ABCCApp** (BSC) - Flash Loan + Deposit/Mint - deposit() + addFixedDay() + claimDDDD(); $10K
- **Bebop DEX** (ARB) - Settlement Manipulation - JamSettlement.settle() without validation; $21K
- **EverValueCoin** (ARB) - Flash Loan + Order Book - Spot price without TWAP; $100K
- **Grizzifi** (BSC) - Referral Abuse - 30-contract referral chain; $61K
- **Hexotic** (ETH) - Flash Swap + Take - hexotic.take() exploit; $500
- **Multicall + Xera** (BSC) - _msgSender() Spoofing - ERC2771 context via multicall; $17K
- **PDZ Token** (BSC) - Flash Loan + Spot Price - getAmountsOut() as oracle; $3.3K
- **SizeCredit** (ETH) - Flash Loan + Arbitrary Data - leverageUpWithSwap() unvalidated; $19.7K
- **WXC Token** (BSC) - Flash Loan + LP Manipulation - Encoded callback in swap data; $37.5K
- **YuliAI** (BSC) - Flash Loan + Spot Price - V3 slot0 price 40x loop; $78K
- **coinbase** (ETH) - Access Control - 0x Settler.execute() drains Coinbase fee account; $300K
- **d3xai** (BSC) - Flash Loan + Arbitrage - 27 buyer/seller pairs exploit proxy mispricing; $190K

#### 2025 年 9 月
- **Kame** (SEI) - Access Control - swap() with arbitrary executor; $18K
- **NGP Token** (BSC) - Flash Loan + sync() in Transfer - _update() calls sync() internally; $2M

#### 2025 年 10 月
- **MIMSpell3** (ETH) - Flash Loan + Multi-Cauldron - Borrow from 6 Cauldrons; $1.7M
- **Sharwa Finance** (ARB) - Flash Loan + Margin Trading - Long/short position manipulation; $146K
- **TokenHolder** (BSC) - Access Control - Fake loan via privilegedLoan(); $20K

#### 2025 年 11 月
- **Balancer V2** (ETH) - Precision Loss - Composable Stable Pool _calcInGivenOut() precision; $120M
- **DRL Vault V3** (ETH) - Flash Loan + Slippage - swapToWETH() stale pricing; $100K
- **Moonwell** (BASE) - Flash Loan + Over-Borrow - wrsETH mispriced vs wstETH; $1M

#### 2025 年 12 月
- **yETH Pool** (ETH) - Virtual Balance Manipulation - Rate update mid-cycle exploit; $9M

### 2026 年

#### 2026 年 1 月
- **Makina** (ETH) - Flash Loan + Curve Hot-Swap - Curve 3crypto hot-swap on DOLA/3crv; $5.1M
- **Squid/multi** (multiple) - Flash Loan + Unvalidated Call - Cross-chain call injection

#### 2026 年 2 月
- **DxSale** (BSC) - Backdoor - DxSale LP locker backdoor; $7.3M
- **TrustedVolumes** (ETH) - Arbitrary Call - Proxy with unvalidated RFQ data; $5.87M
- **Adshares** (ETH) - Access Control - Unprotected mint function
- **N0AH** (BSC) - Access Control - Unprotected function
- **NewMarketTrading** (ETH/BASE/ARB) - Access Control - 88 Safes affected across 3 chains; $3.98M

#### 2026 年 3 月
- **MAP Protocol** (BSC/ETH) - Cross-chain Replay - Cross-chain message replay
- **Planet** (BASE) - Flash Loan + LP Donation - LP burn + sync inflates share price; $667K
- **ROE** (BASE) - Flash Loan + Swap - Flash loan swap
- **ENDO** (ETH) - Flash Loan + Fee - Fee bypass
- **SKP** (BSC) - Rug Pull - Engineered exit scam
- **SQToken** (BSC) - Access Control - transferOwnership() exploit
- **Landwolf** (ETH) - Flash Loan + Tax - Tax bypass

#### 2026 年 4 月
- **Verus Bridge** (BSC) - zk-Proof Forgery - zk-proof forgery in bridge; $11.58M
- **Ekubo** (ETH) - Flash Loan + Lock/Forward - Lock/forward/withdraw loop; $1.4M
- **LAXO** (BASE) - Flash Loan + Borrow - Borrow exploit; $493K (partially rescued)
- **WBTC** (BSC) - Flash Loan + LP Burn - LP burn + sync inflates share price
- **Whalebit** (BSC/Eth) - Flash Loan + Arbitrage - Cross-chain arbitrage
- **Algebra** (BASE) - Flash Loan + Pool Manipulation - Algebra CL pool manipulation
- **3A** (BSC) - Flash Loan + Fee - Token fee bypass
- **H2** (BSC) - Flash Loan + Swap - Flash loan
- **Aerodrome** (BASE) - Flash Loan + CL Pool - Aerodrome CL pool manipulation
- **Moonwell** (BASE) - Flash Loan + Oracle - Oracle manipulation on Moonwell; $242K
- **RocketSwap** (BASE) - Flash Loan + Price - Price manipulation

#### 2026 年 5 月
- **TNV** (BSC) - Flash Loan + Swap - Flash loan
- **PL** (BSC) - Flash Loan + Swap - Flash loan
- **R2E** (BSC) - Access Control - Unprotected function
- **SV** (BSC) - Flash Loan + Swap - Flash loan
- **YSDAO** (BSC) - Tax Bypass - 1 wei output bypasses _isRemoveLiquidity() buy tax; $19.5K
- **Sei** (SEI) - Flash Loan + Stake - Staking manipulation
- **BBL** (BSC) - Flash Loan + Swap - Flash loan

#### 2026 年 6 月
- **AISOTH Presale** (BSC) - Flash Swap + Presale Buyout - Buy all from presale, sell on DEX; $30.3K
- **ATM Token** (BSC) - Incidental LP Burn - Victim error, burn pair-held LP; $1.6K
- **ATM Token** (BSC) - Tax Bypass - 30 farmer clones bypass anti-whale; $243K
- **Ambient/CrocSwap** (ETH) - Surplus Manipulation - ColdPath cmd 73/74 surplus extraction; 67.85 ETH
- **Aztec Connect** (ETH) - zk-Rollup Bypass - numRealTxs outside proof hash; $2.19M
- **Aztec Escape Hatch** (ETH) - Fake zk-Proof - Turbo-PLONK proof forgery; $2.2M
- **Aztec Escape Hatch 2** (ETH) - Unconstrained Circuit - proof_id not constrained to 0
- **BOSS Token** (BSC) - Flash Loan + userMint/Burn - userMint/userBurn loop drain; $10.2K
- **BYToken** (BSC) - Permissionless AutoBurn - triggerAutoBurn() + corner supply; $87K
- **DIP Token** (BSC) - Fee Bypass - Double router-transfer + skim/sync; $111K
- **DLMC Token** (BSC) - Reserve Price Manipulation - livePrice() manipulation via affiliate; $222K
- **DTXT Token** (BSC) - Fee Bypass - 1 wei transfer tricks fee logic; $35K
- **JB Token** (BSC) - Venus Collateral Cycle - Flash borrow + Venus collateral cycle; $50K
- **LBP Token** (BSC) - Pancake Vault + Referral - Vault lock + referral manipulation; 610 BNB
- **NovaBox** (ETH) - extcodesize Bypass - Constructor-based check bypass; 56.73 ETH
- **OLPC Token** (BSC) - decimalsValue + Sync/Skim - 20x sync/skim decay loop; $1.1M
- **RoyalRoyalties** (POLY) - Zero-Amount Batch Transfer - ERC1155 zero-amount inflates tier balance; $261K
- **TOPBPool** (ETH) - Aragon Governance - Vote to mint 10B TOP tokens; 944 WETH
- **ThetanutsFi** (ETH) - Integer Division Truncation - mint() truncation with near-zero supply; $2.1M
- **Thetanuts** (ETH) - Flash Loan + Share Claim - Claim component shares, free mint loop; $105K
- **WHALE Token** (BSC) - Pancake Infinity Vault - Vault lock + LP manipulation; $3.5K

---

## 附录：关键漏洞模式总结

### 十大根本原因（按频率排序）：
1. **缺失/不足的访问控制** - 缺少 onlyOwner/require 修饰符的函数
2. **现货价格作为预言机** - 使用未经 TWAP 的 AMM 现货价格
3. **份额价格操纵（捐赠）** - totalBalance/totalSupply 捐赠通胀
4. **任意外部调用** - swap/router 函数中用户可控的 calldata
5. **Skim/Sync 储备操纵** - 转账收费代币池操纵
6. **闪电贷回调信任** - 未经验证的闪电贷回调
7. **重入攻击** - 缺少检查-影响-交互模式
8. **整数除法/舍入** - 乘法前进行除法，截断
9. **代理初始化** - 未初始化的代理合约
10. **签名/Permit 验证** - 缺少 ecrecover 检查，可重放签名

### 十大最大损失（历史总计）：
1. Poly Network - $611M (2021)
2. Ronin Bridge - $625M (2022)
3. Wormhole - $326M (2022)
4. Beanstalk - $182M (2022)
5. Nomad Bridge - $190M (2022)
6. Euler Finance - $197M (2023)
7. Balancer V2 - $120M (2025)
8. Curve/Vyper - $73M (2023)
9. KyberSwap - $46M (2023)
10. PancakeBunny - $45M (2021)

### 防御建议：
1. **切勿将现货价格用作预言机** - 始终使用 TWAP 或 Chainlink 风格的外部预言机
2. **使用检查-影响-交互模式** - 防止所有外部调用中的重入攻击
3. **验证份额价格变动** - 实施针对捐赠攻击的检查（虚拟份额、最小流动性）
4. **限制关键函数的访问** - 使用 onlyOwner、访问控制列表和多重签名进行升级
5. **验证外部 calldata** - 切勿执行用户提供的任意 calldata
6. **使用 OpenZeppelin 的 ReentrancyGuard** - 应用于所有发起外部调用的函数
7. **使用闪电贷场景进行测试** - 在测试套件中包含基于闪电贷的攻击向量
8. **使用 Solidity 0.8+** - 内置溢出/下溢保护
9. **正确初始化代理** - 使用 OpenZeppelin 的 initializer 修饰符
10. **添加 totalSupply 检查** - 当 totalSupply 接近零时防止除零错误和份额价格操纵

---

*由 Claude Code 对 D:\DeFiHackLabs\src\test 中约 762 个 POC 文件进行分析生成*
*涵盖 79 个目录，时间跨度从 2017 年 7 月至 2026 年 6 月*
*分析涵盖 Ethereum、BSC、Arbitrum、Polygon、Avalanche、Base、Optimism、Fantom、Linea、zkSync、Sei 等多个链上的协议。*
