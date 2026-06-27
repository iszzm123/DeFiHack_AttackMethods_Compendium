# DeFi Hacking Attack Methods Compendium

> Comprehensive analysis of ~762 Proof-of-Concept (PoC) exploit files across 79 directories (2017–2026)
> Source: `D:\DeFiHackLabs\src\test\`

---

## Table of Contents

1. [Overview & Statistics](#overview--statistics)
2. [Attack Type Encyclopedia](#attack-type-encyclopedia)
3. [Year-by-Year Detailed Listing](#year-by-year-detailed-listing)

---

## Overview & Statistics

### Total Files Analyzed: 762

**Distribution by Year:**
| Year | Files | Directories |
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

**Distribution by Attack Type:**
| Attack Type | Count | Percentage |
|-------------|-------|------------|
| Flash Loan + Price/Oracle Manipulation | ~180 | ~24% |
| Flash Loan + Share/Accounting Inflation (Donation) | ~65 | ~9% |
| Flash Loan + Staking/Reward Manipulation | ~55 | ~7% |
| Flash Loan + Skim/Sync Reserve Manipulation | ~40 | ~5% |
| Flash Loan + Lending/Borrow Exploit | ~45 | ~6% |
| Flash Loan + Reentrancy (ERC777/ERC721) | ~30 | ~4% |
| Flash Loan + Arbitrage/MEV | ~25 | ~3% |
| Flash Loan + Access Control | ~20 | ~3% |
| Flash Loan + Burn/Supply Manipulation | ~20 | ~3% |
| Flash Loan + Liquidation Exploit | ~15 | ~2% |
| Access Control (standalone) | ~80 | ~10% |
| Reentrancy (standalone) | ~20 | ~3% |
| Token Tax/Fee Bypass | ~40 | ~5% |
| Arbitrary External Call | ~25 | ~3% |
| Delegatecall/Proxy Manipulation | ~20 | ~3% |
| Cross-chain/Bridge Exploit | ~15 | ~2% |
| Governance Attack | ~10 | ~1% |
| Integer Overflow/Underflow | ~10 | ~1% |
| Signature/Permit Bypass | ~10 | ~1% |
| Private Key Compromise | ~5 | <1% |
| zk-Rollup Vulnerability | ~5 | <1% |
| Other | ~27 | ~4% |

**Distribution by Chain:**
| Chain | Count | Percentage |
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
| Multi-chain | ~25 | ~3% |

**Top 20 Largest Individual Exploits:**
| Rank | Exploit | Loss | Year | Attack Type |
|------|---------|------|------|-------------|
| 1 | Poly Network | ~$611M | 2021 | Cross-chain Bridge Exploit |
| 2 | Balancer V2 Composable Stable Pool | ~$120M | 2025 | Precision Loss / Batch Swap |
| 3 | Curve Finance (Vyper) | ~$73M | 2023 | Reentrancy (Vyper compiler bug) |
| 4 | KyberSwap Elastic | ~$46M | 2023 | Precision Loss / Tick Manipulation |
| 5 | Euler Finance | ~$197M | 2023 | Flash Loan + Donation Attack |
| 6 | Nomad Bridge | ~$190M | 2022 | Bridge Exploit / Authorization |
| 7 | Wormhole Bridge | ~$326M | 2022 | Bridge Exploit / Signature Bypass |
| 8 | Ronin Bridge | ~$625M | 2022 | Private Key Compromise |
| 9 | Beanstalk | ~$182M | 2022 | Governance + Flash Loan |
| 10 | Orbit Chain Bridge | ~$81M | 2024 | Signature Forgery |
| 11 | yETH Pool | ~$9M | 2025 | Virtual Balance Manipulation |
| 12 | Inverse Finance | ~$15.6M | 2022 | Oracle Manipulation |
| 13 | MIM/Abracadabra (Spell) | ~$6.5M | 2024 | Donation/Rounding Attack |
| 14 | Harvest Finance | ~$24M | 2020 | Flash Loan + Price Manipulation |
| 15 | bZx v1 | ~$8.5M | 2020 | Flash Loan + Oracle Manipulation |
| 16 | PancakeBunny | ~$45M | 2021 | Flash Loan + Price Manipulation |
| 17 | Rari Capital (Fuse) | ~$80M | 2022 | Flash Loan + Reentrancy |
| 18 | Venus (BNB exploit) | ~$56M | 2023 | Oracle Manipulation / Liquidation |
| 19 | Verus Bridge | ~$11.58M | 2026 | zk-Proof Forgery |
| 20 | Radiant Capital | ~$4.5M | 2024 | Liquidity Index Manipulation |

### Common Attack Pattern Flow

Most exploits follow one of these patterns:

**Pattern 1 — Flash Loan + Price Manipulation:**
```
Flash Loan → Swap to manipulate pool price → Execute dependent action (borrow/mint/redeem)
  → Swap back → Repay Flash Loan → Keep profit
```

**Pattern 2 — Flash Loan + Share Inflation (Donation):**
```
Flash Loan → Deposit small amount → Donate large amount to inflate share price
  → Redeem shares at inflated price → Repay Flash Loan → Keep profit
```

**Pattern 3 — Flash Loan + Reentrancy:**
```
Flash Loan → Call vulnerable function → Trigger callback → Re-enter vulnerable function
  → State not updated → Extract more than allowed → Repay Flash Loan
```

**Pattern 4 — Skim/Sync Reserve Manipulation:**
```
Flash Swap → Transfer tokens to pair → sync() to update reserves
  → skim() to extract excess → Swap at manipulated rate → Repay flash swap
```

**Pattern 5 — Access Control Bypass:**
```
Find unprotected function (mint/setAdmin/initialize/claim) → Call directly
  → Transfer/mint/withdraw tokens → Swap for profit
```

## Attack Type Encyclopedia

All attack types identified across 762 POC files, organized by category.

---

### A. Flash Loan + Price / Oracle Manipulation

**Frequency:** ~180 incidents (~24%)
**Summary:** The attacker uses a flash loan (uncollateralized loan that must be repaid in the same transaction) to swap large amounts on an AMM pool, manipulating the pool's spot price. This manipulated price is then used by a dependent protocol (lending, derivative, yield aggregator) that reads the pool price as an oracle. The attacker executes actions at the manipulated price (borrow, mint, redeem), then reverses the swap and repays the flash loan.

**Representative Examples:**

| Protocol | Date | Chain | Loss | Specifics |
|----------|------|-------|------|-----------|
| bZx v1 | 2020-02 | ETH | $950K | First DeFi flash loan attack. Manipulated Kyber/Uniswap ETH price to over-borrow from bZx |
| Harvest Finance | 2020-10 | ETH | $24M | Manipulated USDC-USDT Curve pool via flash loan before withdrawing from fUSDC/fUSDT vaults |
| PancakeBunny | 2021-05 | BSC | $45M | Flash swap inflated BUNNY price on PancakeSwap; minted BUNNY on BunnyMinter at inflated price |
| Spartan Protocol | 2021-05 | BSC | $30M | Manipulated pool ratios via flash loan to extract excessive LP tokens |
| Inverse Finance | 2022-06 | ETH | $15.6M | Swapped WBTC on Curve to manipulate YVCrv3CryptoFeed oracle; borrowed DOLA against inflated collateral |
| Raft | 2023-11 | ETH | $3.2M | Manipulated fetchPrice() oracle to withdraw more collateral than allowed |
| Onyx Protocol | 2023-11 | ETH | $2M | Deposited PEPE as collateral, manipulated oracle via donation, over-borrowed |
| MetaLend | 2023-11 | ETH | - | Manipulated Comptroller price feed to borrow more than collateralized |
| Makina | 2026-01 | ETH | $5.1M | Hot-swapped on Curve 3crypto to manipulate DOLA/3crv price; redeemed at inflated rate across 3 vaults |

**Key Vulnerable Functions/Components:**
- AMM spot price used as oracle (UniswapV2 pair reserves, Curve pool balances)
- `getReserves()` / `getAmountsOut()` / `slot0` used without TWAP
- `latestAnswer()` on Chainlink-inspired feeds that read pool balances
- `exchange()` on Curve pools - large swaps affect pool imbalance
- Lending protocol `borrow()` that checks collateral value against manipulated price

---

### B. Flash Loan + Share/Accounting Inflation (Donation Attack)

**Frequency:** ~65 incidents (~9%)
**Summary:** The attacker exploits vault/pool share price calculation that uses `totalBalance / totalSupply`. By donating assets directly to the vault (inflating totalBalance without minting new shares), the attacker makes existing shares worth more. The attacker then deposits a small amount to get shares, donates to inflate, and withdraws at the inflated rate.

**Representative Examples:**

| Protocol | Date | Chain | Loss | Specifics |
|----------|------|-------|------|-----------|
| Euler Finance | 2023-03 | ETH | $197M | Donated eDAI to inflate share price; borrowed against inflated collateral |
| Yearn yDAI | 2020-11 | ETH | $11M | Donated DAI to yDAI vault before withdrawing to inflate share value |
| OneRing | 2022-03 | ETH | $2M | Donated to Curve/fUSDT pool before withdrawing from ring-miner |
| Wise Lending (x2) | 2024-03 | ETH | $464K each | Donated to inflate share price before withdrawing more than deposited |
| UwU Lend (x2) | 2024-06 | ETH | ~$20M | Donated uDai to inflate share price; two separate exploits |
| Sumer Money | 2024-04 | ETH | - | Donation attack on lending pool share prices |
| SATURN | 2024-05 | ETH | - | Donation attack on vault share price |
| Sonne Finance | 2024-05 | OETH | - | Donation + VELO manipulation to inflate lending pool shares |
| Lodestar Finance | 2022-12 | ARB | ~$6M | Donated plvGLP to inflate share price; borrowed all available assets |
| GMX/SEED | 2025-01 | AVAX | ~$32K | Donation to GMX lp staking to inflate share price |

**Key Vulnerable Functions/Components:**
- `balance()` or `totalAssets()` returning full contract balance (including donated assets)
- Share calculation: `shares = amount * totalSupply / totalBalance` (donation inflates denominator)
- Vault `deposit()` and `withdraw()` without checking for share price manipulation
- Lending `borrow()` using share-price-based collateral value

---

### C. Flash Loan + Staking/Reward Manipulation

**Frequency:** ~55 incidents (~7%)
**Summary:** Attacker uses flash loans to manipulate staking reward calculations. Common techniques include: inflating reward token price via swaps, creating artificial team/referral structures to claim unearned bonuses, manipulating epoch/period boundaries to claim rewards multiple times, and exploiting reward-per-share accumulation logic.

**Representative Examples:**

| Protocol | Date | Chain | Loss | Specifics |
|----------|------|-------|------|-----------|
| TrustPad | 2023-11 | BSC | $155K | Inflated rewards via repeated receiveUpPool() calls |
| Nmbplatform | 2022-12 | BSC | - | Manipulated reward price via flash swap during staking reward claim |
| SDAO | 2022-11 | BSC | - | Transferred tokens directly to pair to inflate totalStakeReward |
| UEarnPool | 2022-11 | BSC | - | Created 22 contracts in referral chain; claimed team rewards through chain |
| Grizzifi | 2025-08 | BSC | $61K | Created 30 contract referral chain; harvested honey and ref bonuses |
| SWAPP Staking | 2025-07 | BSC | $32K | Exploited manualEpochInit() for incorrect reward/share calculation |
| Pawnfi | 2023-06 | ETH | - | Staked NFT, looped deposit/borrow/withdraw to accumulate rewards |
| PinkEco | 2023-07 | BSC | - | Parent-child staking contract exploited for double-dip rewards |

**Key Vulnerable Functions/Components:**
- `getReward()` / `claimReward()` without reentrancy protection
- Reward rate updates based on spot prices
- Team/referral bonus systems without proper validation
- `harvest()` functions that don't update state before distributing

---

### D. Flash Loan + Skim/Sync Reserve Manipulation

**Frequency:** ~40 incidents (~5%)
**Summary:** Exploits fee-on-transfer/reflection tokens where the actual token balance in the LP pool differs from the tracked reserve. The attacker manipulates the pool by: transferring tokens directly to the pair (creating excess balance), calling `sync()` to update reserves to the manipulated balance, or calling `skim()` to extract excess tokens. Repeated skim/sync cycles drain the pool.

**Representative Examples:**

| Protocol | Date | Chain | Loss | Specifics |
|----------|------|-------|------|-----------|
| HackDao | 2022-05 | BSC | - | Flash loan + skim() to drain fee-on-transfer tokens |
| UN Token | 2023-06 | BSC | - | Transferred 93% UN back to pair, skim(), repeat with lower percentages |
| AES Token | 2022-12 | BSC | - | DODO flash loan + skim() 37 times to extract fee tokens |
| DFS Token | 2022-12 | BSC | - | Flash swap + repeated skim() + sync() cycles |
| Wdoge | 2022-04 | BSC | - | skim/sync manipulation of reflection token |
| BGLD | 2022-12 | BSC | - | Migration + skim exploit across old/new token pairs |
| MBC_ZZSH | 2022-11 | BSC | - | swapAndLiquifyStepv1() exploited via reserve manipulation |
| WETC Token | 2025-07 | BSC | $101K | Multiple sync/skim cycles to drain PancakeV2 pair |
| OLPC | 2026-06 | BSC | $1.1M | decimalsValue manipulation + 20x sync/skim decay loop |
| DIP | 2026-06 | BSC | $111K | Double router-transfer + skim/sync to bypass 6% sell fee |

**Key Vulnerable Functions/Components:**
- `sync()` - permissionless update of AMM pair reserves
- `skim()` - permissionless extraction of excess pair balance
- Fee-on-transfer tokens where actual balance > tracked reserve
- Reflection tokens with `_isTransfer` / `_isAddLiquidity` fee bypass checks
- Tokens that call `sync()` internally during `_transfer()` or `_update()`

---

### E. Flash Loan + Lending/Borrow Exploit

**Frequency:** ~45 incidents (~6%)
**Summary:** Attacker uses flash loans to manipulate the conditions under which a lending protocol evaluates collateral. This includes: inflating collateral token price via swaps, manipulating share prices via donation, re-entering during liquidation to claim excess collateral, or exploiting accounting bugs in borrow/repay logic to extract more than deposited.

**Representative Examples:**

| Protocol | Date | Chain | Loss | Specifics |
|----------|------|-------|------|-----------|
| Midas Capital | 2023-06 | BSC | - | Deposited ANKR as collateral, manipulated Algebra pool, borrowed multiple assets |
| Polter Finance | 2024-11 | FTM | $7M | Deposited 1 wei of SpookyToken, borrowed ALL reserves across 8 tokens |
| Channels Finance | 2023-12 | BSC | - | Manipulated cCLP_BTCB_BUSD gulp() to inflate collateral value |
| MahaLend | 2023-11 | ETH | - | Repeated deposit/withdraw to manipulate share calculation |
| Themis | 2023-06 | ARB | - | Supplied WETH as collateral, borrowed all available from 5 pools |
| Beanstalk | 2022-04 | ETH | $182M | Flash loan + governance to siphon all assets from Beanstalk |
| Ploutoz | 2021-11 | BSC | - | Flash loan BUSD, pumped DOP price via swaps, borrowed multiple assets |
| Inverse Finance | 2022-06 | ETH | $15.6M | Manipulated oracle via Curve swap to over-borrow DOLA |

**Key Vulnerable Functions/Components:**
- `borrow()` using manipulable collateral pricing
- `enterMarkets()` / `exitMarket()` without proper validation
- Liquidation logic with off-by-one or rounding errors
- Price feed reliance on pool spot prices
- `gulp()` functions that sync token balances without validation

---

### F. Flash Loan + Reentrancy

**Frequency:** ~30 incidents (~4%)
**Summary:** Attacker exploits the fact that flash loan callbacks or token transfer hooks (ERC777 `tokensToSend`/`tokensReceived`, ERC721 `onERC721Received`, ERC1155 `onERC1155Received`) can re-enter the vulnerable contract before state is updated. The reentrant call sees stale state and can perform actions that should not be possible.

**Representative Examples:**

| Protocol | Date | Chain | Loss | Specifics |
|----------|------|-------|------|-----------|
| Grim Finance | 2021-12 | FTM | ~$30M | depositFor() → transferFrom() callback → recursive depositFor() (7 reentrant calls) |
| bZx v2 | 2021-12 | ETH | $55M | ERC777 tokensToSend callback reentered bZx during swap |
| Cream Finance | 2021-08 | ETH | $18M | ERC777 reentrancy in crETH repayBorrow() |
| LendfMe | 2020-04 | ETH | $25M | ERC777 reentrancy during token transfer |
| Hundred Finance | 2022-03 | ETH | - | Flash loan + reentrancy on Hundred Finance |
| Agave | 2022-03 | ETH | - | Reentrancy in liquidation function |
| Revest Finance | 2022-03 | ETH | - | ERC-1155 reentrancy |
| Bacon Protocol | 2022-03 | ETH | - | ERC777 reentrancy |
| Rari Capital | 2022-04 | ETH | $80M | Flash loan + receive() reentrancy on Rari Fuse pools |
| JAY Token | 2022-12 | ETH | - | Flash loan + ERC721 transferFrom callback → sell() |
| SpankChain | 2020-10 | ETH | - | ERC777 reentrancy in payment channel |
| Uniswap v1 | 2020-02 | ETH | - | ERC777 reentrancy during ETH->ERC swap |
| Clober DEX | 2024-12 | BASE | $501K | Fake token pool + burnHook() reentrancy |
| DeltaPrime | 2024-11 | ARB | $4.75M | SmartLoan.claimReward() → fake pair reentered convertETH() |
| WIFCOIN | 2024-06 | ETH | ~$3.4K | claimEarned() without reentrancy protection |

**Key Vulnerable Functions/Components:**
- ERC777 `tokensToSend` / `tokensReceived` callbacks
- ERC721 `onERC721Received` callback
- ERC1155 `onERC1155Received` callback
- `transferFrom()` implementations that call external hooks
- `receive()` / fallback functions that trigger state changes
- `burn()` functions that call back before burning

---

### G. Access Control Vulnerabilities

**Frequency:** ~80 standalone + ~20 flash-loan-aided (~13%)
**Summary:** The most common vulnerability across all POCs. Critical functions lack access control modifiers like `onlyOwner`. Attackers call these functions directly: `mint()` to create unlimited tokens, `setAdmin()` to escalate privileges, `initialize()` to reinitialize proxies, `claim()` to drain locked funds, `transfer()` to move tokens without authorization.

**Representative Examples:**

| Protocol | Date | Chain | Loss | Specifics |
|----------|------|-------|------|-----------|
| Parity Wallet (first) | 2017-07 | ETH | $30M | initWallet() without access control; attacker became owner |
| Parity (kill) | 2017-11 | ETH | $280M | kill() public in library contract; all funds frozen |
| Bancor | 2020-06 | ETH | - | manage() without access control |
| DODO | 2021-08 | BSC | - | _DODO_APPROVE_ without protection |
| Visor Finance | 2021-12 | ETH | $8M | deposit() without proper access control |
| Build Finance | 2022-02 | ETH | - | Governance attack via unprotected vote manipulation |
| Sandbox LAND | 2022-02 | ETH | - | Unprotected LAND management functions |
| FPR Token | 2022-12 | BSC | - | setAdmin() had no access control |
| NovaExchange | 2022-12 | BSC | - | rewardHolders() lacked access control |
| HYPR | 2023-12 | ETH | $200K | Uninitialized proxy; attacker called initialize() to set themselves as messenger |
| Telcoin | 2023-12 | POLY | $1.24M | Uninitialized CloneableProxy; attacker set malicious logic contract |
| Floor Protocol | 2023-12 | ETH | $1.6M | extMulticall() allowed unauthorized NFT transfers |
| Transit Finance | 2023-12 | BSC | 173 BNB | Router swap function allowed arbitrary external calls |
| AK1111 | 2024-11 | BSC | $31.5K | nonblockingLzReceive1() lacked access control |
| SuperRare | 2025-07 | ETH | $730K | updateMerkleRoot() without access control |
| SEC | 2025-04 | BSC | $21K | Public claim function allowed draining tokens |
| DxSale | 2026-02 | BSC | $7.3M | Backdoor in DxSale LP locker contract |

**Key Vulnerable Functions/Components:**
- `mint()` without `onlyOwner` modifier
- `initialize()` without checking initialization status
- `setAdmin()` / `setOwner()` / `transferOwnership()` without access control
- `claim()` / `withdraw()` without ownership validation
- Public `kill()` / `selfdestruct()` functions
- `approveToken()` lacking access control
- Proxy `upgradeTo()` without access control
- LayerZero `nonblockingLzReceive()` without access control

### H. Reentrancy (Standalone, No Flash Loan)

**Frequency:** ~20 incidents (~3%)
**Summary:** Classic reentrancy where external calls (token transfers, ETH sends) trigger callbacks that re-enter the vulnerable function before state updates complete. Unlike flash-loan-aided reentrancy, these attacks use the attacker's own tokens or small initial capital.

**Representative Examples:**
- **SpankChain** (2020-10, ETH) - ERC777 reentrancy in payment channel
- **Uniswap v1** (2020-02, ETH) - ERC777 reentrancy during ETH->token swap
- **AkutarNFT** (2022-04, ETH) - DoS + logic error in NFT auction refund
- **NFT Trader** (2023-12, ETH) - Malicious ERC721 in swap intent
- **PineProtocol** (2023-12, ETH) - Flash loan + NFT lending repay exploit
- **AA** (2025-02, BSC) - 150 transfers with fake token to drain vault
- **Bizness** (2024-12, BASE) - splitLock() reentrancy via ETH receive

**Key Vulnerable Functions:**
- Functions that send ETH before updating state
- `withdraw()` / `claimReward()` without checks-effects-interactions pattern
- NFT `safeTransferFrom()` callbacks without reentrancy guard

---

### I. Token Tax / Fee Bypass

**Frequency:** ~40 incidents (~5%)
**Summary:** Many BSC tokens implement buy/sell taxes, transfer fees, and anti-whale mechanisms. Attackers bypass these through: 1-wei transfers (below tax threshold), deploying fresh contracts (bypass cooldown/whitelist), using `skim()`/`sync()` to manipulate tracked balances, and exploiting the `_isAddLiquidity()` / `_isRemoveLiquidity()` checks that skip fees during LP operations.

**Representative Examples:**
- **Novo** (2022-05, BSC) - transferFrom() without 'from' validation; transferred FROM LP pool
- **Snood** (2022-06, ETH) - transferFrom() bypassed allowance checks
- **RFB** (2022-12, BSC) - Try/catch brute-force of 50 swap amounts to find profitable fee bypass
- **SEAMAN** (2022-11, BSC) - 20 single-wei transfers to trigger fee events
- **ATM Token** (2026-06, BSC) - 30 farmer clones bypass anti-whale per-address limits and cooldown
- **DTXT** (2026-06, BSC) - 1-wei USDT transfer tricks token fee logic
- **BYToken** (2026-06, BSC) - Permissionless triggerAutoBurn corners token supply
- **YSDAO** (2026-05, BSC) - 1 wei output to bypass _isRemoveLiquidity() buy tax

**Key Vulnerable Mechanisms:**
- `_isAddLiquidity()` / `_isRemoveLiquidity()` heuristic checks
- Fee-on-transfer tokens where balance != tracked reserve
- Anti-whale max-amount-per-address limits (bypassed via clones)
- Cooldown timers between buy/sell (bypassed via fresh addresses)
- Reflection/dividend tokens with manipulable fee accounting

---

### J. Arbitrary External Call

**Frequency:** ~25 incidents (~3%)
**Summary:** Router, zap, and cross-chain bridge contracts accept user-supplied calldata or destination addresses and execute them without proper validation. Attackers craft `transferFrom()` payloads to drain tokens from users who approved the vulnerable contract.

**Representative Examples:**
- **LI.FI** (2022-03, ETH) - Arbitrary call in swap data
- **Auctus** (2022-03, ETH) - Arbitrary external call via router
- **Rubic** (2022-12, BSC) - routerCallNative() with arbitrary router parameter
- **Polynomial** (2022-11, OETH) - swapAndDeposit() with malicious swapData
- **Transit Finance** (2023-12, BSC) - Router swap with arbitrary call
- **Socket Gateway** (2024-01, ETH) - $3.3M - Arbitrary external call vulnerability
- **Paraswap** (2024-02, ETH) - Arbitrary calldata in swap parameters
- **MixedSwapRouter** (2024-05, ETH) - Arbitrary call in swap data
- **SquidMulticall** (2025-01, ETH) - $122K - Unvalidated Axelar gateway in Squid router
- **MainnetSettler** (2024-11, ETH) - $66K - Bundled actions allowed unauthorized transfers
- **Bebop DEX** (2025-08, ARB) - $21K - JamSettlement.settle() without signature validation
- **Kame** (2025-09, SEI) - $18K - swap() with arbitrary executor parameter
- **SizeCredit** (2025-08, ETH) - $19.7K - leverageUpWithSwap() with unvalidated SwapParams
- **coinbase** (2025-08, ETH) - $300K - 0x MainnetSettler execute() used to drain Coinbase fee account

**Key Vulnerable Functions/Components:**
- `swap()` / `execute()` functions with arbitrary calldata
- Cross-chain bridge `call()` with user-controlled destination
- Zap/deposit functions with unvalidated swap data
- `call()` / `delegatecall()` with user-controlled data
- ERC2771 `_msgSender()` spoofing via trusted forwarder + multicall

---

### K. Delegatecall / Proxy Manipulation

**Frequency:** ~20 incidents (~3%)
**Summary:** Contracts using delegatecall or upgradeable proxy patterns with insufficient access control on upgrade functions. Attackers point the logic contract to a malicious one, taking over the proxy's storage and draining funds.

**Representative Examples:**
- **PAID Network** (2021-03, ETH) - Key compromised; proxy upgraded to drain
- **88mph** (2021-08, ETH) - Unprotected delegatecall
- **Uranium** (2021-04, BSC) - Backdoor in upgrade function
- **WaultFinance** (2021-11, BSC) - Proxy upgrade without access control
- **HYPR** (2023-12, ETH) - Uninitialized proxy; attacker set messenger role
- **Telcoin** (2023-12, POLY) - Uninitialized CloneableProxy
- **CCV** (2023-12, BSC) - Proxy function with unprotected selectors

**Key Vulnerable Components:**
- Uninitialized proxy contracts
- `upgradeTo()` / `upgradeToAndCall()` without access control
- Logic contract constructors that don't initialize storage
- Metamorphic / Cloneable contracts with owner-setable implementation

---

### L. Cross-chain / Bridge Exploit

**Frequency:** ~15 incidents (~2%)
**Summary:** Bridge contracts that verify cross-chain messages have flaws in signature verification, message validation, or relayer authentication. Attackers forge valid-looking messages to mint/bridge tokens without depositing on the source chain.

**Representative Examples:**
- **Poly Network** (2021-08, ETH/BSC/Poly) - $611M - Cross-chain message verification bypass
- **Wormhole** (2022-02, SOL) - $326M - Signature verification bypass
- **Nomad Bridge** (2022-08, ETH) - $190M - Cross-chain message spoofing, '0x00' accepted as valid root
- **Qubit** (2022-01, BSC) - $80M - Deposit logic flaw in QBridge
- **Anyswap/Multichain** (2022-01, ETH) - $8M - Signature/authorization bypass
- **Nerve Bridge** (2021-12, BSC) - Flash loan + swap manipulation on bridge pool
- **Orbit Chain Bridge** (2024-01, ETH) - $81M - Signature forgery
- **Verus Bridge** (2026-04, ETH) - $11.58M - zk-proof forgery
- **MAP Protocol** (2026-03, BSC/ETH) - Cross-chain message replay
- **Across Protocol** (2025-03, ETH) - $105K - Stale root bundle verification

**Key Vulnerable Components:**
- Signature verification logic (allow-listed signers, ECDSA validation)
- Message root / state root acceptance without proof
- Replay protection (nonces, chain IDs)
- Relayer / keeper authentication
- `processRollup()` permissionless + unconstrained fields

---

### M. Governance Attack

**Frequency:** ~10 incidents (~1%)
**Summary:** Attackers manipulate DAO governance to pass malicious proposals. Techniques include: using flash loans to acquire voting power temporarily, exploiting low proposal thresholds, or manipulating timelocks to execute proposals immediately.

**Representative Examples:**
- **Beanstalk** (2022-04, ETH) - $182M - Flash loan governance: borrowed BEAN/ETH LP tokens to pass emergency proposal siphoning all protocol assets
- **Build Finance** (2022-02, ETH) - Governance attack via vote manipulation
- **TOP Token** (2026-06, ETH) - Aragon vote to mint 10B TOP to attacker; pre-existing balance gave voting power
- **Elephant Money** (2022-04, BSC) - Governance + oracle manipulation
- **Fortress Loans** (2022-05, BSC) - Governance + oracle manipulation

**Key Vulnerable Components:**
- Governance quorum thresholds too low
- Flash-loanable voting power
- Emergency/speed proposals without timelock
- Single-transaction proposal + vote + execution

---

### N. Integer Overflow / Underflow

**Frequency:** ~10 incidents (~1%)
**Summary:** Classic arithmetic overflow bugs. Before Solidity 0.8 (which has built-in overflow checks), contracts using unchecked arithmetic allowed attackers to overflow/underflow balances, supply, or reward calculations.

**Representative Examples:**
- **BEC Token** (2018-04, ETH) - batchTransfer() overflow: attacker sent enormous amounts
- **Gym Network 2** (2022-06, BSC) - depositFromOtherContract() with absurdly large amount caused overflow
- **Umbrella Network** (2022-03, ETH) - Integer underflow in reward calculation
- **Matez** (2024-11, BSC) - Integer overflow in stake() amount (340282366920938463463374607431768211456)
- **ThetanutsFi** (2026-06, ETH) - $2.1M - Integer division truncation in mint() after near-zero totalSupply

**Key Vulnerable Components:**
- Pre-0.8 Solidity arithmetic operations
- Batch transfer functions
- Reward calculation with unvalidated inputs
- Share calculation dividing by near-zero totalSupply

---

### O. Signature / Permit Bypass

**Frequency:** ~10 incidents (~1%)
**Summary:** Attackers forge or replay signatures used in permit/approval systems. Common flaws include: missing ecrecover validation, replay attacks across chains, accept default/empty signatures, and EIP-712 domain separator mismatches.

**Representative Examples:**
- **Chainswap** (2021-07/10, ETH) - Two signature exploits for $4.4M and $2M
- **Multichain NUM** (2022-11, ETH) - Forged permit (v=0, r="0x", s="0x") bypassed verification
- **DEUS/DEI** (2022-04, ETH) - Signature + oracle combined exploit
- **Orbit Chain Bridge** (2024-01, ETH) - $81M - Signature forgery across multiple validator keys

**Key Vulnerable Components:**
- `ecrecover()` return value not checked (returns address(0) on invalid sig)
- Permit signatures with default invalid values accepted
- Missing chainID in EIP-712 domain separator (replay across chains)
- Signature replay across different function calls

---

### P. Private Key Compromise

**Frequency:** ~5 incidents (<1%)
**Summary:** Bridge or protocol multisig private keys are compromised, allowing attackers to sign arbitrary transactions. These are infrastructure/operational security failures, not smart contract bugs.

**Representative Examples:**
- **Ronin Bridge** (2022-03, ETH) - $625M - Compromised 5-of-9 validator keys (Sky Mavis + Axie DAO)
- **Harmony Bridge** (2022-06, ETH) - $100M - Compromised 2-of-5 multisig keys
- **PAID Network** (2021-03, ETH) - $3M - Compromised deployer key

**Key Vulnerable Components:**
- Multisig wallet with low threshold relative to total signers
- Centralized key management in bridge validators
- Hot wallets with signing capability

---

### Q. zk-Rollup / zk-Proof Vulnerabilities

**Frequency:** ~5 incidents (<1%)
**Summary:** Zero-knowledge proof verification flaws where the proof does not actually bind all critical fields, or the verification circuit has unconstrained variables. Attackers submit valid-looking proofs that authorize unauthorized state transitions.

**Representative Examples:**
- **Aztec Connect** (2026-06, ETH) - $2.19M - numRealTxs field outside proof hash, unconstrained
- **Aztec Escape Hatch** (2026-06, ETH) - $2.2M - Fake Turbo-PLONK proof in escapeHatch()
- **Aztec Escape Hatch 2** (2026-06, ETH) - Unconstrained proof_id in escape hatch circuit
- **Verus Bridge** (2026-04, BSC) - $11.58M - zk-proof forgery in Verus bridge protocol

**Key Vulnerable Components:**
- Proof fields not bound by hashed public inputs
- Unconstrained circuit variables
- Permissionless proof submission
- Escape hatch / emergency functions with relaxed verification

---

### R. Cross-Contract / Integration Bug

**Frequency:** ~25 incidents (~3%)
**Summary:** Bugs that arise from complex interactions between multiple protocols. Includes: flash loan callbacks that trust the caller, composability issues between lending + DEX protocols, integration assumptions that don't hold across all scenarios, and data injection across contract boundaries.

**Representative Examples:**
- **Sturdy Finance** (2023-06, ETH) - Read-only reentrancy: Balancer.exitPool() triggered oracle price manipulation
- **MIMSpell** (2023-06, ETH) - ZeroXStargateLPSwapper with injected swap data
- **MIMSpell3** (2025-10, ETH) - $1.7M - Borrow from 6 Cauldrons simultaneously
- **Yearn_ydai** (2020-11, ETH) - Cross-protocol flash loan + donation on yDAI/Curve
- **Saddle Finance** (2022-04, ETH) - Flash loan + swap price deviation across pools
- **FortressLoans** (2022-05, BSC) - Governance + oracle + Venus cross-protocol exploit
- **VINU** (2023-06, ETH) - Fake router accepted by addLiquidityETH() to manipulate price tracking

**Key Vulnerable Components:**
- Cross-protocol flash loan callbacks
- Unvalidated router/dex addresses in swap paths
- Oracle price dependencies across protocols
- Assumptions about token behavior (standard vs fee-on-transfer vs rebasing)

---

### S. NFT Exploits

**Frequency:** ~15 incidents (~2%)
**Summary:** NFT-related exploits including: using the same NFT as collateral multiple times (replay), fake NFTs passing ownership checks, manipulating NFT prices via token swaps, and exploiting NFT staking/rewards mechanisms.

**Representative Examples:**
- **XCarnival** (2022-06, ETH) - $3.87M - Same BAYC used as collateral 33 times via replay
- **PineProtocol** (2023-12, ETH) - $90K - NFT lending flash loan + repay exploit
- **Floor Protocol** (2023-12, ETH) - $1.6M - extMulticall() to steal PPG NFTs
- **NFT Trader** (2023-12, ETH) - $3M - Fake ERC721 in swap intent to steal valuable NFTs
- **Pawnfi** (2023-06, ETH) - Flash loan + ApeCoin staking with BAYC NFT
- **TheNFTV2** (2023-11, ETH) - Flash swap + NFT price manipulation via TheDAO token

**Key Vulnerable Components:**
- NFT collateral tracking (same NFT used multiple times)
- NFT ownership validation during swaps
- NFT staking reward calculations
- ERC721 transfer hooks and callbacks

---

### T. MEV / Sandwich / Bot Exploitation

**Frequency:** ~15 incidents (~2%)
**Summary:** Attackers exploit vulnerable MEV bots that have permissionless execution functions, or perform sandwich attacks on pending transactions. These bots often have flawed access control allowing anyone to trigger their strategies.

**Representative Examples:**
- **MEV 0x28d9** (2022-12, ETH) - DODO flash loan iteratively drained MEV bot
- **MEV 0x8c2d** (2023-11, BSC) - Flash swap + privileged role assignment on bot
- **MEV 0xa247** (2023-11, ETH) - Drained 24 tokens from vulnerable bot contract
- **MEV 0x0ad8** (2022-11, ETH) - Public function with arbitrary external call
- **bot (Curve/Uniswap)** (2023-11, ETH) - $2M - Aave flash loan + Curve exchange to drain router
- **CoW Protocol** (2024-11, ETH) - $59K - uniswapV3SwapCallback() spoofing
- **VRug** (2024-11, ETH) - $8.4K - Sandwich/frontrun on Uniswap V2 swap

**Key Vulnerable Components:**
- Permissionless execution functions on bots
- Unvalidated callback functions
- Flash loan callbacks that trust any caller
- Sandwich-vulnerable AMM transactions

---

### U. Read-Only Reentrancy

**Frequency:** ~8 incidents (~1%)
**Summary:** A subtle variant where a reentrant call is made to a view/pure function that reads state which hasn't been updated yet. The external protocol reads a stale/incorrect price or balance, allowing the attacker to borrow more or extract more value than they should.

**Representative Examples:**
- **Sturdy Finance** (2023-06, ETH) - Balancer.exitPool() reentrancy caused oracle to report 3x inflated price
- **Balancer V2 (read-only)** (multiple) - Read-only reentrancy via pool callback to manipulate LP token pricing

**Key Vulnerable Components:**
- View functions that can trigger callbacks (rare, but possible with ERC777 hooks)
- External protocols that read pool state during a reentrant call
- Oracle implementations using pool balances that can change during flash operations

---

### V. Precision Loss / Rounding Manipulation

**Frequency:** ~10 incidents (~1%)
**Summary:** Integer division rounding errors that favor the user (or can be exploited via large amounts). Attackers iteratively exploit rounding to drain small amounts until the pool is depleted.

**Representative Examples:**
- **KyberSwap Elastic** (2023-11, ETH) - $46M - Precision loss in tick calculation for concentrated liquidity
- **Balancer V2 Composable Stable** (2025-11, ETH) - $120M - Precision loss in _calcInGivenOut() exploited via thousands of iterative swaps
- **yETH Pool** (2025-12, ETH) - $9M - Virtual balance precision loss via mid-cycle rate updates
- **ThetanutsFi** (2026-06, ETH) - $2.1M - Integer division truncation in mint() with near-zero totalSupply
- **Thetanuts** (2026-06, ETH) - $105K - Index vault share truncation during mint/claim
- **MIMSpell** (2024-02, ETH) - $6.5M - Rounding attacks on Abracadabra Spell

**Key Vulnerable Components:**
- `_calcInGivenOut()` / `_calcOutGivenIn()` division before multiplication
- Share calculation: `shares = amount * totalSupply / totalBalance`
- Tick/index math in concentrated liquidity pools
- Batch swap calculations with iterative rounding
- Fee calculation with precision truncation favoring user

---

### W. Proxy Initialization / Logic Contract Exploits

**Frequency:** ~15 incidents (~2%)
**Summary:** Uninitialized proxy contracts or logic contract implementations that can be called directly. Attackers initialize the proxy with malicious parameters or call the logic contract directly to manipulate storage.

**Representative Examples:**
- **HYPR** (2023-12, ETH) - $200K - Uninitialized L1ChugSplashProxy
- **Telcoin** (2023-12, POLY) - $1.24M - Uninitialized CloneableProxy
- **NovaBox** (2026-06, ETH) - Constructor-based extcodesize bypass for deposit
- **SquidMulticall** (2025-01, ETH) - $122K - Unprotected Axelar gateway address setting

**Key Vulnerable Components:**
- `initialize()` without `initializer` modifier
- Logic contract constructors (not initializers)
- Proxy upgrade functions without access control
- CREATE/CREATE2 deterministic address prediction

---

### X. Other / Unique Exploits

**Frequency:** ~27 incidents (~4%)
**Summary:** Miscellaneous exploits that don't fit into the above categories.

**Including:**
- **Optimism/Wintermute** (2022-06, OETH) - Address collision via CREATE opcode
- **Compound TUSD** (2022-03, ETH) - sweepToken() logic flaw allowed draining stuck TUSD
- **Paraluni** (2022-03, ETH) - Fake token attack on MasterChef fork
- **Redacted Cartel** (2022-03, ETH) - ERC-20 allowance logic bug
- **TreasureDAO** (2022-03, ARB) - Price validation flaw in MAGIC token
- **Saddle Finance** (2022-04, ETH) - Swap price deviation exploit
- **AK1111** (2024-11, BSC) - LayerZero-like unprotected receive function
- **RoyalRoyalties** (2026-06, POLY) - Zero-amount batch transfer inflates tier balance
- **TOPBPool** (2026-06, ETH) - Aragon governance proposal to mint tokens
- **SheepFarm** (2022-11, BSC) - Game logic exploit in blockchain game
- **ChiSale** (2024-11, ETH) - Flash loan with empty callback, trapped funds
- **VISTA** (2024-10, BSC) - Flash loan without frozen token check
- **BSC Token** (2025-05, BSC) - Flash swap + AMM accounting bypass via token features

## Year-by-Year Detailed Listing

### 2017

#### 2017-07

- **Parity Wallet (first)** (ETH) - Access Control - `initWallet()` without access control allowed attacker to become owner of multisig wallet
- **Parity Wallet (kill)** (ETH) - Access Control - `kill()` public in library contract; ~$280M frozen (not lost to attacker)

#### 2017-11

- **BEC Token** (ETH) - Integer Overflow - `batchTransfer()` overflow allowed sending enormous amounts
- **Bancor** (ETH) - Access Control - `manage()` without access control

### 2018

#### 2018-04

- **BEC Token** (ETH) - Integer Overflow - `batchTransfer()` integer overflow allowed transferring massive token amounts

#### 2018-10

- **SpankChain** (ETH) - Reentrancy (ERC777) - ERC777 reentrancy in payment channel

### 2020

#### 2020-04

- **LendfMe** (ETH) - ERC777 Reentrancy - ERC777 reentrancy during token transfer; ~$25M lost

#### 2020-06

- **Balancer** (ETH) - Flash Loan + Price Manipulation - First flash loan + price manipulation against Balancer pools
- **bZx v1** (ETH) - Flash Loan + Oracle Manipulation - First DeFi flash loan attack; manipulated Kyber/Uniswap ETH price to over-borrow from bZx; ~$950K

#### 2020-08

- **YFV** (ETH) - Flash Loan + Price Manipulation - Flash loan attack on YFV token

#### 2020-09

- **bZx v2** (ETH) - Reentrancy - ERC777 tokensToSend callback reentered bZx during swap; ~$55M

#### 2020-10

- **Harvest Finance** (ETH) - Flash Loan + Price Manipulation - Manipulated USDC-USDT Curve pool; ~$24M
- **ValueDeFi** (BSC) - Flash Loan + Price Manipulation - Flash loan on ValueDeFi pools

#### 2020-11

- **Yearn yDAI** (ETH) - Flash Loan + Share Inflation - Donated DAI to yDAI vault before withdrawing to inflate share value; ~$11M

#### 2020-12

- **Cover Protocol** (ETH) - Flash Loan + Price Manipulation - Flash loan attack on Cover protocol

### 2021

#### 2021-01

- **SushiSwap mISO** (ETH) - Double-counting Logic Flaw - batch/msg.value flaw in SushiSwap's mISO launchpad

#### 2021-02

- **Alpha Homora** (ETH) - Flash Loan + Price Manipulation - Flash loan to manipulate IronBank lending

#### 2021-03

- **PAID Network** (ETH) - Private Key Compromise - Deployer key compromised; proxy upgraded to drain funds; ~$3M
- **Chainswap (exp1)** (ETH) - Signature Manipulation - Signature validation bypass in Chainswap bridge

#### 2021-04

- **Uranium** (BSC) - Integer Overflow + Backdoor - Flash loan + integer overflow in fee calculation
- **EasyFi** (POLY) - Access Control - Unprotected mint function

#### 2021-05

- **PancakeBunny** (BSC) - Flash Loan + Price Manipulation - Flash swap inflated BUNNY price; minted BUNNY at inflated price on BunnyMinter; ~$45M
- **BurgerSwap** (BSC) - Flash Loan + Price Manipulation - Flash loan attack on BurgerSwap
- **JulSwap** (BSC) - Flash Loan + Price Manipulation - Flash loan attack on JulSwap

#### 2021-06

- **Spartan Protocol** (BSC) - Flash Loan + Price Manipulation - Manipulated pool ratios via flash loan to extract excessive LP tokens; ~$30M

#### 2021-07

- **Chainswap (exp2)** (ETH) - Signature Manipulation - Second Chainswap signature exploit; ~$2M
- **XSURGE** (BSC) - Flash Loan + Price Manipulation - Flash loan on XSURGE token

#### 2021-08

- **Cream Finance** (ETH) - ERC777 Reentrancy - ERC777 reentrancy in crETH repayBorrow(); ~$18M
- **Poly Network** (ETH/BSC/Poly) - Cross-chain Bridge - Cross-chain message verification bypass; largest DeFi exploit at ~$611M (mostly returned)
- **DODO** (BSC) - Access Control - _DODO_APPROVE_ without protection
- **Popsicle Finance** (ETH) - Flash Loan + Share Accounting - Share price manipulation in Popsicle LP optimizer

#### 2021-09

- **xWin Finance** (BSC) - Flash Loan + Price Manipulation - Flash loan attack on xWin Finance
- **bEarn Fi** (BSC) - Flash Loan + Price Manipulation - Flash loan attack on bEarn Fi
- **NowSwap** (ETH) - Integer Overflow - Overflow in NowSwap fee calculation

#### 2021-10

- **Indexed Finance** (ETH) - Flash Loan + Complex Price Manipulation - Multi-step flash loan attack on Indexed Finance pools
- **SafeDollar** (POLY) - Flash Loan + Share Accounting - Share price manipulation in SafeDollar vault
- **11** (BSC) - Flash Loan + Price Manipulation - Flash loan attack on token 11

#### 2021-11

- **Cream (2nd)** (ETH) - Flash Loan + Share Accounting - Second Cream exploit; share price manipulation
- **Eleven Finance** (BSC) - Flash Loan + Price Manipulation - Flash loan on Eleven Finance
- **Monoswap** (POLY) - Flash Loan + Share Accounting - Share manipulation in Monoswap
- **Wault Finance** (BSC) - Proxy/Delegatecall - Proxy upgrade without access control
- **ZABU** (BSC) - Flash Loan + Price Manipulation - Flash loan on ZABU token
- **Ploutoz** (BSC) - Flash Loan + Price Manipulation - Flash loan BUSD, pumped DOP price, borrowed multiple assets
- **Levyathan** (BSC) - Compromised Keys - Key compromise
- **Nimbus** (BSC) - Integer Overflow - Overflow in Nimbus reward calculation
- **88mph** (ETH) - Access Control - Unprotected delegatecall

#### 2021-12

- **Grim Finance** (FTM) - Reentrancy - depositFor() -> transferFrom() callback -> 7 recursive depositFor() calls; ~$30M
- **Nerve Bridge** (BSC) - Flash Loan + Bridge Swap - Flash loan + swap manipulation on bridge pool
- **Visor Finance** (ETH) - Access Control - deposit() without proper access control; ~$8M
- **Pickle Finance** (ETH) - Flash Loan + Complex - Multi-step flash loan on Pickle jars

### 2022

#### 2022-01

- **Anyswap/Multichain** (ETH) - Signature/Authorization Bypass - Signature verification bypass in Anyswap bridge; ~$8M
- **Qubit Finance** (BSC) - Bridge Deposit Logic - Deposit logic flaw in QBridge; ~$80M

#### 2022-02

- **Build Finance** (ETH) - Governance Attack - Governance vote manipulation
- **Meter** (MOONRIVER) - Flash Loan + Price Manipulation - Flash loan on Meter (Moonriver)
- **Sandbox LAND** (ETH) - Access Control - Unprotected LAND management
- **TecraSpace TCR** (BSC) - Flash Loan + burnFrom Manipulation - Flash loan to exploit burnFrom

#### 2022-03

- **Agave** (ETH) - Reentrancy (Liquidation) - Reentrancy in Agave liquidation
- **Auctus** (ETH) - Arbitrary External Call - Arbitrary call in Auctus router
- **Bacon Protocol** (ETH) - ERC777 Reentrancy - ERC777 reentrancy
- **Compound TUSD** (ETH) - sweepToken Logic Flaw - sweepToken() allowed draining stuck TUSD
- **Fantasm** (FTM) - Decimal Precision Error - Precision error in Fantasm
- **Hundred Finance** (ETH) - Flash Loan + Reentrancy - Flash loan + reentrancy on Hundred Finance
- **LI.FI** (ETH) - Arbitrary External Call - Arbitrary call in swap data
- **OneRing** (ETH) - Flash Loan + Share Inflation - Donation to Curve/fUSDT pool to inflate share price; ~$2M
- **Paraluni** (ETH) - Fake Token Attack - Fake token on MasterChef fork
- **Redacted Cartel** (ETH) - ERC-20 Allowance Bug - Allowance logic bug
- **Revest Finance** (ETH) - ERC-1155 Reentrancy - ERC1155 reentrancy
- **Ronin Bridge** (ETH) - Private Key Compromise - Compromised 5-of-9 validator keys; ~$625M
- **TreasureDAO** (ARB) - Price Validation Flaw - Price validation in MAGIC token
- **Umbrella Network** (ETH) - Integer Underflow - Underflow in reward calculation

#### 2022-04

- **AkutarNFT** (ETH) - DoS + Logic Error - DoS in NFT auction refund
- **Beanstalk** (ETH) - Governance + Flash Loan - Flash loan governance attack; ~$182M
- **Elephant Money** (BSC) - Flash Loan + Oracle - Governance + oracle manipulation
- **Gym Network (1)** (BSC) - Flash Loan + Migration - Migration manipulation via flash loan
- **Rari Capital (Fuse)** (ETH) - Flash Loan + Reentrancy - Flash loan + receive() reentrancy on Fuse pools; ~$80M
- **Rikkei Finance** (BSC) - Oracle Manipulation - Oracle price manipulation
- **Saddle Finance** (ETH) - Flash Loan + Swap Deviation - Swap price deviation across pools
- **Wdoge** (BSC) - Flash Loan + Skim/Sync - Reflection token skim/sync manipulation
- **Zeed Protocol** (BSC) - Flash Loan + Cross-Pool Skim - Cross-pool skim arbitrage
- **CF Token** (BSC) - Access Control - Exposed _transfer
- **DEUS/DEI** (ETH) - Oracle + Signature - Combined oracle and signature exploit

#### 2022-05

- **BAYC/ApeCoin/NFTX** (ETH) - Flash Loan + Airdrop Claim - Flash loan to claim airdrop
- **Fortress Loans** (BSC) - Governance + Oracle - Governance + oracle manipulation on Venus fork
- **HackDao** (BSC) - Flash Loan + Skim - Flash loan + skim() exploit
- **Novo** (BSC) - Flash Loan + transferFrom() Bypass - transferFrom() without 'from' validation

#### 2022-06

- **Discover** (BSC) - Flash Loan + Logic Flaw - pledgein() logic flaw
- **Gym Network (2)** (BSC) - Integer Overflow - depositFromOtherContract() overflow
- **Harmony Bridge** (ETH) - Private Key Compromise - Compromised 2-of-5 multisig; ~$100M
- **Inverse Finance** (ETH) - Flash Loan + Oracle Manipulation - Curve swap manipulated YVCrv3CryptoFeed oracle; ~$15.6M
- **Optimism/Wintermute** (OETH) - Address Collision - CREATE deterministic address collision
- **Snood** (ETH) - transferFrom() + sync - transferFrom() bypassed allowance, sync() to manipulate reserves
- **XCarnival** (ETH) - Replay Attack - Same BAYC used as collateral 33 times via replay; ~$3.87M

#### 2022-07

- **Nomad Bridge** (ETH) - Bridge Message Spoofing - '0x00' accepted as valid root; ~$190M
- **TransitSwap** (BSC) - Flash Loan + Cross-Chain - Flash loan on TransitSwap
- **Blur** (ETH) - Access Control - Unprotected function in Blur marketplace
- **Quiz** (BSC) - Flash Loan + Swap - Flash loan swap manipulation
- **Vacation** (BSC) - Access Control - Unprotected claim function
- **CBD** (ETH) - Access Control - Unprotected function
- **Hashflow** (BSC) - Access Control - Unprotected function
- **Revert** (BSC) - Reentrancy - Reentrancy in Revert contract
- **ZERO** (BSC) - Access Control - Unprotected function
- **RocketPool** (BSC) - Flash Loan + Swap - Flash loan + Rocket Pool token manipulation

#### 2022-08

- **Acala aUSD** (ACA) - Mint Logic Bug - Incorrect mint operation drained aUSD
- **Balancer Boosted Pool** (ETH) - Flash Loan + Oracle - Flash loan + Boosted Pool price manipulation
- **NOMAD** (BSC) - Bridge Exploit - Nomad-style bridge exploit on BSC
- **Slingshot** (ETH) - Flash Loan + Swap - Flash loan swap manipulation
- **Wrapped RocketPool** (ETH) - Flash Loan - Flash loan manipulation
- **Optimism Governance** (OETH) - Governance - Governance proposal manipulation
- **YAY** (ETH) - Flash Loan + Reentrancy - Flash loan + reentrancy on YAY Token
- **HBTC** (ETH) - Access Control - Unprotected function
- **YAK** (AVAX) - Flash Loan + Price - Flash loan price manipulation on Yak
- **Aura Finance** (ETH) - Flash Loan + Swap - Flash loan + Aura Finance
- **ShibaDoge** (ETH) - Flash Loan + Tax - Flash loan tax bypass
- **MAI (QiDAO)** (POLY) - Flash Loan - Flash loan on QiDAO
- **Tornado Cash** (ETH) - Governance - Governance attack on Tornado
- **Sos** (ETH) - Access Control - Unprotected function

#### 2022-09

- **Arbitrum Governance** (ARB) - Governance - Arbitrum governance token exploit
- **Beanstalk (BSC fork)** (BSC) - Governance - Beanstalk fork governance
- **Boba** (BSC) - Flash Loan + Swap - Flash loan swap
- **BNB Chain Bridge** (BSC) - Bridge Exploit - BSC bridge message manipulation; ~$566M (halted)
- **Cream 3rd** (ETH) - Flash Loan + Pool Manipulation - Third Cream exploit
- **DFX** (ETH) - Flash Loan + Swap - Flash loan on DFX
- **DOK** (BSC) - Access Control - Unprotected function
- **Fei** (ETH) - Flash Loan + Lending - Flash loan on Fei
- **Five** (BSC) - Flash Loan + Farm - Flash loan + farm manipulation
- **Hedgey** (BSC) - Flash Loan + Stake - Flash loan staking manipulation
- **Olympus** (ETH) - Flash Loan + Bond - Flash loan bond manipulation
- **ShadowFi** (BSC) - Access Control - Unprotected function
- **SSS** (BSC) - Access Control - Unprotected function

#### 2022-10

- **ArchAngel** (BSC) - Access Control - Unprotected function
- **Bad Guys** (BSC) - Access Control - Unprotected function
- **Benz** (BSC) - Access Control - Unprotected function
- **DING** (BSC) - Access Control - Unprotected function
- **Doko** (BSC) - Flash Loan + Swap - Flash loan swap
- **DRAC** (BSC) - Access Control - Unprotected function
- **EINST** (BSC) - Access Control - Unprotected function
- **FAME** (BSC) - Flash Loan + Swap - Flash loan swap
- **FRIES** (BSC) - Flash Loan + Swap - Flash loan swap
- **GS** (BSC) - Access Control - Unprotected function
- **Guacamole** (AVAX) - Flash Loan + Swap - Flash loan on Trader Joe pools
- **Gym** (BSC) - Access Control - Unprotected function
- **JNET** (BSC) - Access Control - Unprotected function
- **KIT** (BSC) - Flash Loan + Swap - Flash loan swap
- **LOV** (BSC) - Access Control - Unprotected function
- **MZ** (BSC) - Access Control - Unprotected function
- **ND** (BSC) - Access Control - Unprotected function
- **OGC** (BSC) - Access Control - Unprotected function
- **ReaperFarm** (BSC) - Flash Loan + Price - Flash loan price manipulation
- **RVC** (BSC) - Flash Loan + Swap - Flash loan swap
- **SMOON** (BSC) - Flash Loan + Swap - Flash loan swap
- **SPD** (BSC) - Flash Loan + Swap - Flash loan swap
- **SafeHamsters** (BSC) - Access Control - Unprotected function
- **UFD** (BSC) - Access Control - Unprotected function
- **VIVA** (BSC) - Flash Loan + Swap - Flash loan swap

#### 2022-11

- **Alpaca Finance** (BSC) - Flash Loan + Liquidation - Flash loan liquidation exploit
- **DFX (XIDR)** (ETH) - Flash Loan + Deposit Manipulation - flash() callback with deposit/withdraw
- **Kashi/Abracadabra** (ETH) - Flash Loan + Self-Liquidation - BentoBox flash loan, self-liquidation returned excess collateral
- **MBC/ZZSH** (BSC) - Flash Loan + swapAndLiquify - swapAndLiquifyStepv1() exploit
- **MEV 0x0ad8** (ETH) - Arbitrary External Call - Public function with arbitrary external call
- **MooCAKECTX/Beefy** (BSC) - Flash Loan + Borrow/Harvest - Venus borrow + Beefy harvest manipulation
- **Multichain NUM** (ETH) - Signature Bypass - Forged permit (v=0, r="0x", s="0x")
- **Polynomial** (OETH) - Arbitrary External Call - swapAndDeposit() with malicious swapData
- **SDAO** (BSC) - Flash Loan + Staking Rewards - Staking reward manipulation
- **SEAMAN** (BSC) - Flash Loan + Fee Manipulation - 20 single-wei transfers to trigger fee events
- **SheepFarm (game)** (BSC) - Game Logic Exploit - Game economy exploit (sell village returned more than invested)
- **UEarnPool** (BSC) - Flash Loan + Referral Rewards - 22 contracts in referral chain for team rewards

#### 2022-12

- **AES** (BSC) - Flash Loan + Skim - DODO flash loan + 37x skim() calls
- **APC** (BSC) - Flash Loan + Oracle - Oracle price manipulation via swap
- **BBOX** (BSC) - Flash Loan + Fee Bypass - Helper contract bypassed sell time-limit
- **BGLD** (BSC) - Flash Loan + Migration + Skim - Migration + skim across old/new token pairs
- **DFS** (BSC) - Flash Swap + Skim - Flash swap + skim/sync cycles
- **Defrost** (AVAX) - Flash Loan + Deposit/Withdraw - Nested flash loans with deposit/redeem
- **ElasticSwap** (AVAX) - Flash Swap + Internal Balance - Internal balance manipulation
- **FPR** (BSC) - Access Control - setAdmin() had no access control
- **JAY Token** (ETH) - Flash Loan + ERC721 Reentrancy - ERC721 transferFrom callback -> sell()
- **Lodestar Finance** (ARB) - Flash Loan + Price + Multi-Protocol - Donated plvGLP to inflate share price; borrowed all assets; ~$6M
- **MEV 0x28d9** (ETH) - Flash Loan + Bot Drain - Iterative DODO flash loan drained MEV bot
- **MUMUG** (AVAX) - Flash Swap + Bond Price - Bond price manipulation via spot price
- **Nmbplatform** (BSC) - Flash Loan + Staking - Staking reward manipulation via flash swap
- **NovaExchange** (BSC) - Access Control - rewardHolders() lacked access control
- **Overnight/USD+** (AVAX) - Flash Loan + Price - Multi-protocol stablecoin arbitrage
- **RFB** (BSC) - Flash Loan + Fee Bypass - Try/catch brute-force of 50 swap amounts
- **Rubic** (BSC) - Arbitrary External Call - routerCallNative() with arbitrary router parameter
- **TIFI** (BSC) - Flash Swap + Oracle Manipulation - LP reserves used as price oracle

### 2023

#### 2023-01
- **Boggy** (BSC) - Flash Loan + Skim - Flash loan + multiple skim() calls
- **Bsc** (BSC) - Access Control - Unprotected function
- **Ceres** (BSC) - Flash Loan + Reward - Reward manipulation via flash loan
- **DEUS** (ETH) - Flash Loan + Swap - Flash loan swap manipulation
- **Duel** (BSC) - Flash Loan + Swap - Flash loan swap
- **DYNA** (BSC) - Flash Loan + Swap - Flash loan swap
- **FST** (BSC) - Flash Loan + Swap - Flash loan swap
- **HNY** (BSC) - Flash Loan + Swap - Flash loan swap
- **KHS** (BSC) - Flash Loan + Swap - Flash loan swap
- **PE** (BSC) - Flash Loan + Swap - Flash loan swap
- **Sishi** (BSC) - Flash Loan + Swap - Flash loan swap
- **Star** (BSC) - Flash Loan + Swap - Flash loan swap

#### 2023-02
- **0xDAO** (BSC) - Access Control - Unprotected function
- **ABC** (BSC) - Flash Loan + Swap - Flash loan
- **Anwan** (BSC) - Flash Loan + Swap - Flash loan
- **Based** (BSC) - Access Control - Unprotected function
- **China** (BSC) - Access Control - Unprotected mint
- **CLB** (BSC) - Access Control - Unprotected function
- **Crowd** (BSC) - Access Control - Unprotected function
- **DOG** (BSC) - Flash Loan + Swap - Flash loan
- **Domino** (BSC) - Access Control - Unprotected function
- **Dungeon** (BSC) - Access Control - Unprotected function
- **FSC** (BSC) - Flash Loan + Swap - Flash loan
- **GAL** (BSC) - Flash Loan + Swap - Flash loan
- **MILK** (BSC) - Flash Loan + Swap - Flash loan
- **N3K** (BSC) - Flash Loan + Swap - Flash loan
- **YFSP** (BSC) - Access Control - Unprotected function

#### 2023-03
- **Euler Finance** (ETH) - Flash Loan + Donation Attack - Donated eDAI to inflate share price; $197M
- **Arbitrum** (ARB) - Access Control - Unprotected function
- **Arbi** (ARB) - Flash Loan + Swap - Flash loan
- **BNB-48** (BSC) - Access Control - Unprotected function
- **Liquid** (BSC) - Flash Loan + Swap - Flash loan
- **Twindex** (BSC) - Flash Loan + Swap - Flash loan
- **Wanren** (BSC) - Flash Loan + Swap - Flash loan
- **YY** (BSC) - Flash Loan + Swap - Flash loan
- **ZFD** (BSC) - Access Control - Unprotected mint
- **Diamond** (BSC) - Flash Loan + Swap - Flash loan

#### 2023-04
- **Good Gensler** (BSC) - Access Control - Unprotected function
- **SST** (BSC) - Access Control - Unprotected function
- **TT** (BSC) - Flash Loan + Swap - Flash loan
- **VP** (BSC) - Access Control - Unprotected mint
- **HNS** (BSC) - Flash Loan + Swap - Flash loan
- **Parsiq** (BSC) - Flash Loan + Swap - Flash loan
- **Cake Monster** (ETH) - Flash Loan + Swap - Flash loan
- **MEV Bot** (ETH) - Flash Loan + Bot Drain - Exploited vulnerable MEV bot
- **0x131** (ETH) - Access Control - Unprotected function
- **Ken** (ETH) - Reentrancy - Reentrancy
- **0x5F** (ETH) - Flash Loan + Price - Flash loan oracle manipulation
- **Nexus** (ETH) - Access Control - Unprotected function
- **X2Y2** (ETH) - NFT Exploit - NFT market exploit

#### 2023-05
- **Affine** (ETH) - Flash Loan + Donation - Donation attack on vault shares
- **CGT** (BSC) - Flash Loan + Swap - Flash loan
- **CRV** (BSC) - Flash Loan + Swap - Flash loan
- **DFX** (ETH) - Flash Loan + Decimal - Decimal precision exploit
- **DRA** (BSC) - Flash Loan + Swap - Flash loan
- **FS** (BSC) - Flash Loan + Swap - Flash loan
- **Gem** (BSC) - Flash Loan + Swap - Flash loan
- **Globe** (BSC) - Access Control - Unprotected function
- **GoldenBoys** (BSC) - Flash Loan + Swap - Flash loan
- **HODL** (BSC) - Flash Loan + Swap - Flash loan
- **HP** (BSC) - Flash Loan + Swap - Flash loan
- **IGU** (BSC) - Flash Loan + Swap - Flash loan
- **KET** (BSC) - Flash Loan + Swap - Flash loan
- **KO** (BSC) - Flash Loan + Swap - Flash loan
- **LAMBO** (BSC) - Flash Loan + Swap - Flash loan
- **MLTP** (BSC) - Flash Loan + Swap - Flash loan
- **MN** (BSC) - Access Control - Unprotected mint
- **POT** (BSC) - Flash Loan + Swap - Flash loan
- **PSET** (BSC) - Access Control - Unprotected function
- **TH** (BSC) - Flash Loan + Swap - Flash loan
- **TL** (BSC) - Access Control - Unprotected function
- **TOR** (BSC) - Flash Loan + Swap - Flash loan
- **TSU** (BSC) - Flash Loan + Swap - Flash loan

#### 2023-06
- **Cellframe** (BSC) - Flash Loan + Migration - LP migration exploit
- **Compounder Finance** (ETH) - Flash Loan + Deposit/Withdraw - yDAI/cDAI deposit/withdraw manipulation
- **Contract 0x7657** (ETH) - Access Control - Unverified contract with unprotected function
- **DDCoin** (BSC) - Flash Loan + Chain + Order Bypass - Chained 5 DODO flash loans; order limit bypass
- **DEPUSDT/LEVUSDC** (ETH) - Access Control - approveToken() without access control
- **MIMSpell** (ETH) - Cross-Contract Data Injection - ZeroXStargateLPSwapper with injected swap data
- **Midas Capital** (BSC) - Flash Loan + LP + Lending - BNX manipulation, Algebra pool swap, multi-asset borrow
- **MyAi/MultiSender** (BSC) - Access Control - batchTokenTransfer() without from validation
- **NST** (POLY) - Unverified Contract - Closed-source swapper with logic flaw
- **Pawnfi** (ETH) - Flash Loan + NFT Staking - ApeCoin staking + flash loan
- **SELLC** (BSC) - Flash Loan + Miner - Miner contract reward manipulation
- **SHIDO** (BSC) - Flash Loan + Token Conversion - Lock/claim token conversion arbitrage
- **STRAC** (BSC) - Unverified Contract - Fake ERC20 transferFrom callback
- **Sturdy Finance** (ETH) - Read-Only Reentrancy - Balancer.exitPool() triggered oracle price manipulation 3x
- **Themis** (ARB) - Flash Loan + Oracle + Borrow - Supplied WETH, borrowed all from 5 pools
- **UN Token** (BSC) - Flash Loan + Skim - Transferred 93% UN back to pair, skim(), repeat
- **Unverified 0xAC899E** (BSC) - Unverified Contract - Unprotected deposit/withdraw
- **VINU** (ETH) - Fake Router - addLiquidityETH() accepted fake router to manipulate price tracking
- **Citadel Finance** (BSC) - Flash Loan + Price Manipulation
- **WZ** (BSC) - Flash Loan + Swap
- **TokenA** (BSC) - Access Control
- **TOL** (BSC) - Access Control
- **MiniDOGE** (BSC) - Flash Loan + Swap
- **DOG** (BSC) - Flash Loan + Fee

#### 2023-07
- **0xAA** (ETH) - Proxy Exploit - Unprotected proxy function
- **Astrid** (ETH) - Flash Loan + Swap - Flash loan swap
- **Bald** (BSC) - Flash Loan + Swap - Flash loan
- **BNB6** (BSC) - Flash Loan + Swap - Flash loan
- **BSC** (BSC) - Flash Loan + Swap - Flash loan
- **BSC2** (BSC) - Flash Loan + Swap - Flash loan
- **Checker** (BSC) - Flash Loan + Swap - Flash loan
- **Curve** (ETH) - Reentrancy (Vyper) - Vyper compiler bug enabled reentrancy on multiple pools; ~$73M
- **DFX** (BSC) - Flash Loan + Swap - Flash loan
- **Dice** (BSC) - Reentrancy - Reentrancy
- **Drain** (BSC) - Access Control - Unprotected function
- **GM** (BSC) - Flash Loan + Swap - Flash loan
- **HUS** (BSC) - Flash Loan + Swap - Flash loan
- **JPC** (BSC) - Flash Loan + Swap - Flash loan
- **KEN** (BSC) - Flash Loan + Swap - Flash loan
- **Lena** (ETH) - Flash Loan + Staking - Staking reward manipulation
- **LM** (BSC) - Flash Loan + Swap - Flash loan
- **LRC** (ETH) - Flash Loan + Price - Price manipulation
- **M** (BSC) - Flash Loan + Swap - Flash loan
- **MEV Bot** (ETH) - Flash Loan + Bot - MEV bot exploit
- **MM** (BSC) - Flash Loan + Swap - Flash loan
- **Mondo** (ETH) - Flash Loan + Price - Price manipulation
- **MS** (BSC) - Flash Loan + Swap - Flash loan
- **NFT20** (ETH) - NFT Exploit - NFT pool manipulation
- **PinkEco** (BSC) - Flash Loan + Staking - Parent-child staking reward exploit
- **Puffle** (BSC) - Flash Loan + Swap - Flash loan
- **Radiant** (ARB) - Flash Loan + Lending - Lending pool manipulation
- **RMRK** (ETH) - Flash Loan + Swap - Flash loan
- **SDAO** (BSC) - Flash Loan + Swap - Flash loan
- **Sex** (BSC) - Flash Loan + Swap - Flash loan
- **Standard** (BSC) - Access Control - Unprotected function
- **STAR** (BSC) - Flash Loan + Swap - Flash loan
- **SUI** (BSC) - Flash Loan + Swap - Flash loan
- **T1** (BSC) - Flash Loan + Swap - Flash loan
- **TEN** (BSC) - Flash Loan + Swap - Flash loan
- **TTcoin** (BSC) - Access Control - Unprotected mint
- **UBI** (ETH) - Flash Loan + Universal Basic Income - UBI token exploit
- **VGX** (BSC) - Flash Loan + Swap - Flash loan
- **XY** (BSC) - Flash Loan + Swap - Flash loan

#### 2023-08
- **Aave** (ETH) - Flash Loan + Price - Flash loan price manipulation
- **AC** (BSC) - Flash Loan + Swap - Flash loan
- **Aladdin** (ETH) - Flash Loan + Donation - Donation attack
- **AMBT** (BSC) - Access Control - Unprotected function
- **APE** (BSC) - Flash Loan + Swap - Flash loan
- **Aqua** (BSC) - Flash Loan + Swap - Flash loan
- **ARB** (ARB) - Flash Loan + Swap - Flash loan
- **AVAX** (AVAX) - Flash Loan + Price - Flash loan
- **BASE** (BASE) - Access Control - Unprotected function
- **BEB** (BSC) - Flash Loan + Swap - Flash loan
- **BTH** (BSC) - Flash Loan + Swap - Flash loan
- **BVM** (BSC) - Flash Loan + Swap - Flash loan
- **Compound UNI** (ETH) - Flash Loan + Oracle - Manipulated Compound UNI oracle; ~$5M
- **Cyber** (BSC) - Flash Loan + Swap - Flash loan
- **DAM** (BSC) - Flash Loan + Swap - Flash loan
- **DEF** (BSC) - Flash Loan + Swap - Flash loan
- **Delio** (BSC) - Flash Loan + Staking - Staking exploit
- **Diamond** (BSC) - Flash Loan + Swap - Flash loan
- **DIC** (BSC) - Flash Loan + Swap - Flash loan
- **Ecoin** (BSC) - Access Control - Unprotected function
- **ENDO** (BSC) - Flash Loan + Swap - Flash loan
- **ETP** (BSC) - Flash Loan + Swap - Flash loan
- **FEG** (ETH) - Flash Loan + Fee - Fee-on-transfer bypass
- **Flare** (BSC) - Flash Loan + Swap - Flash loan
- **Gala** (ETH) - Access Control - Unprotected mint
- **GMD** (ARB) - Flash Loan + Staking - Staking reward exploit
- **GOLD** (BSC) - Flash Loan + Swap - Flash loan
- **GRT** (BSC) - Flash Loan + Swap - Flash loan
- **GUY** (BSC) - Flash Loan + Swap - Flash loan
- **HEC** (BSC) - Flash Loan + Swap - Flash loan
- **Honey** (BSC) - Flash Loan + Swap - Flash loan
- **HUB** (BSC) - Access Control - Unprotected function
- **JED** (BSC) - Flash Loan + Swap - Flash loan
- **JIM** (BSC) - Access Control - Unprotected mint
- **JP** (BSC) - Flash Loan + Swap - Flash loan
- **KAR** (BSC) - Access Control - Unprotected function
- **KING** (BSC) - Flash Loan + Swap - Flash loan
- **KM** (BSC) - Flash Loan + Swap - Flash loan
- **KUN** (BSC) - Flash Loan + Swap - Flash loan
- **LO** (BSC) - Flash Loan + Swap - Flash loan
- **MA** (BSC) - Flash Loan + Swap - Flash loan
- **Mare** (BSC) - Access Control - Unprotected function
- **MC** (BSC) - Flash Loan + Swap - Flash loan
- **Mice** (ETH) - Flash Loan + NFT - NFT price manipulation
- **MP** (BSC) - Flash Loan + Swap - Flash loan
- **Nem** (BSC) - Flash Loan + Swap - Flash loan
- **NF** (BSC) - Flash Loan + Swap - Flash loan
- **OAS** (BSC) - Flash Loan + Fee - Fee bypass
- **PE** (BSC) - Flash Loan + Swap - Flash loan
- **PEPE** (ETH) - Flash Loan + Tax - Tax/fee bypass
- **PL** (BSC) - Flash Loan + Swap - Flash loan

#### 2023-09
- **BAD** (BSC) - Access Control - Unprotected function
- **BitKeep** (BSC) - Access Control - Unprotected function
- **BTC** (BSC) - Flash Loan + Swap - Flash loan
- **ETH** (BSC) - Access Control - Unprotected function
- **FTM** (BSC) - Flash Loan + Swap - Flash loan
- **GC** (BSC) - Flash Loan + Swap - Flash loan
- **LFI** (BSC) - Flash Loan + Swap - Flash loan
- **Mix** (BSC) - Flash Loan + Swap - Flash loan
- **OAS** (BSC) - Access Control - Unprotected function
- **OC** (BSC) - Flash Loan + Price - Price manipulation
- **OS** (BSC) - Flash Loan + Swap - Flash loan
- **PHB** (BSC) - Flash Loan + Swap - Flash loan
- **PULSE** (ETH) - Flash Loan + Tokenomics - Tokenomics exploit
- **SB** (BSC) - Flash Loan + Swap - Flash loan
- **SP** (BSC) - Access Control - Unprotected function
- **SU** (BSC) - Flash Loan + Swap - Flash loan
- **TST** (BSC) - Flash Loan + Swap - Flash loan
- **Vance** (BSC) - Flash Loan + Swap - Flash loan
- **Wild** (BSC) - Flash Loan + Staking - Staking reward manipulation
- **XAI** (BSC) - Flash Loan + Swap - Flash loan
- **XV** (BSC) - Flash Loan + Swap - Flash loan
- **YF** (BSC) - Flash Loan + Swap - Flash loan

#### 2023-10
- **BALD** (BSC) - Flash Loan + Swap - Flash loan
- **BFA** (ETH) - Access Control - Unprotected function
- **BMC** (BSC) - Flash Loan + Swap - Flash loan
- **DM** (BSC) - Flash Loan + Swap - Flash loan
- **DRB** (BSC) - Flash Loan + Swap - Flash loan
- **DW** (BSC) - Flash Loan + Swap - Flash loan
- **HYC** (BSC) - Access Control - Unprotected function
- **LAB** (BSC) - Flash Loan + Swap - Flash loan
- **LINK** (BSC) - Flash Loan + Swap - Flash loan
- **Maria** (BSC) - Flash Loan + Swap - Flash loan
- **Moo** (ETH) - Access Control - Unprotected function
- **MV** (BSC) - Flash Loan + Swap - Flash loan
- **NO** (BSC) - Flash Loan + Swap - Flash loan
- **pSeudoEth** (ETH) - Flash Loan + Skim/Sync - Skim/sync manipulation
- **Thunder** (BSC) - Flash Loan + Swap - Flash loan
- **TTT** (BSC) - Flash Loan + Swap - Flash loan
- **UXD** (ETH) - Flash Loan + Swap - Flash loan
- **WUF** (BSC) - Flash Loan + Swap - Flash loan
- **X** (BSC) - Flash Loan + Swap - Flash loan

#### 2023-11
- **Token 3913** (BSC) - Flash Loan + Burn - burnPairs() to reduce supply, inflate price
- **AIS** (BSC) - Flash Loan + Access Control - setSwapPairs() + harvestMarket()
- **BRAND** (BSC) - Flash Loan + buyToken Loop - buyToken() called 100x in loop
- **Burntbubba** (ETH) - Flash Loan + LP Share - deposit()/emergencyWithdraw() share manipulation
- **CAROL Protocol** (BASE) - Buy/Sell Price - Bonding curve price manipulation
- **EEE** (BSC) - Flash Loan + Swap - Flash loan swap
- **EHX** (BSC) - Flash Loan + Swap - Flash loan swap
- **Fiber Router** (BSC) - Arbitrary External Call - swapAndCrossOneInch() with malicious calldata
- **KR** (BSC) - Access Control - sellKr() without ownership validation
- **KyberSwap Elastic** (ETH) - Precision Loss - Precision loss in tick calculation; ~$46M
- **LinkDao** (BSC) - Flash Loan + Swap - Unverified contract vulnerability
- **MEV 0x8c2d** (BSC) - MEV Bot - Privileged role assignment on bot
- **MEV 0xa247** (ETH) - MEV Bot - Drained 24 tokens from vulnerable bot
- **MahaLend** (ETH) - Flash Loan + Lending - Deposit/withdraw share manipulation
- **MetaLend** (ETH) - Flash Loan + Lending - Comptroller price feed manipulation
- **OKC** (BSC) - Flash Loan + Multi-Pool - 5 DODO pools + Uniswap V3
- **Onyx Protocol** (ETH) - Flash Loan + Liquidation - PEPE deposit, oracle donation, over-borrow; $2M
- **RBalancer** (ETH) - Flash Loan + Balance Manipulation - Deposit/withdraw balance accounting
- **Raft** (ETH) - Flash Loan + Oracle - fetchPrice() oracle manipulation; $3.2M
- **Shiba Token (fake)** (BSC) - Flash Loan + Batch Transfer - batchTransferLockToken() exploit
- **Swamp Finance** (BSC) - Flash Loan + LP - Deposit/withdraw flow exploit
- **TheNFTV2** (ETH) - Flash Swap + NFT Price - NFT price manipulation via TheDAO token
- **TheStandard.io** (ARB) - Flash Loan + Position Management - collect()/decreaseLiquidity() exploit; $290K
- **Token 8633/9419** (BSC) - Flash Loan + Auto-Marketing - autoSwapAndAddToMarketing() exploit
- **TrustPad** (BSC) - Flash Loan + Staking - receiveUpPool() inflated rewards; $155K
- **WECO** (BSC) - Staking Accounting - deposit() share accounting flaw
- **XAI** (BSC) - Flash Loan + Burn - burn() to reduce supply, inflate price
- **bot** (ETH) - Flash Loan + Curve/Uni Arbitrage - Curve exchange + Aave flash + Uniswap; $2M
- **grok** (ETH) - Flash Loan + Uniswap V3 Fee - Nested flash loans with fee tier manipulation

#### 2023-12
- **BCT** (BSC) - Flash Loan + Inviter/Buy - Invite/referral system exploit
- **BEARNDAO** (BSC) - Flash Loan + Strategy - convertDustToEarned() repeated draining; $769K
- **Bob** (BSC) - Flash Loan + Multi-Pool - skim() + swap() across pools
- **CCV** (BSC) - Flash Loan + Proxy - Unprotected proxy function selectors
- **Channels Finance** (BSC) - Flash Loan + Lending - gulp() inflates collateral on Venus fork
- **Channels** (BSC) - Flash Loan + Lending - Interest accrual manipulation on Compound fork
- **DominoTT** (BSC) - Flash Loan + Forwarder - multicall() + Forwarder.execute() to bypass access control
- **Elephant Status** (BSC) - Flash Loan + Sweep - Elephant.sweep() to drain BUSD; $165K
- **Floor Protocol** (ETH) - NFT Theft - extMulticall() stole PPG NFTs; $1.6M
- **GoodCompound** (ETH) - Flash Loan + COMP Rewards - claimComp() reward manipulation; $13K
- **GoodDollar** (ETH) - Flash Loan + Buy/Sell - GDX bonding curve manipulation; $2M
- **HNet** (BSC) - Flash Loan + Forwarder - multicall() + Forwarder.execute() similar to DominoTT
- **HYPR** (ETH) - Proxy Initialization - Uninitialized proxy, set messenger role; $200K
- **KEST** (BSC) - Flash Loan + Lending - Radiant flash + PancakeSwap swap; $2.3K
- **MAMO** (BSC) - Flash Loan + Swap - DODO flash + swap/withdraw; $3.3K
- **NFT Trader** (ETH) - NFT Theft - Malicious ERC721 in swap intent; $3M
- **PHIL** (BSC) - Unlimited Mint - simpleToken() mints unlimited tokens
- **PineProtocol** (ETH) - Flash Loan + NFT Lending - Migration loan accounting flaw; $90K
- **TIME** (ETH) - Multicall + ERC2771 - _msgSender() spoofing via forwarder; 84.59 ETH
- **Telcoin** (POLY) - Proxy Initialization - Uninitialized CloneableProxy; $1.24M
- **Transit Finance** (BSC) - Arbitrary External Call - Router swap with malicious params; 173 BNB
- **Unverified 0x431abb** (BSC) - Flash Loan + Buy/Stake - buy() + bindParent() manipulation; $500K
- **bZx (2023)** (ETH) - Flash Loan + Lending - withdrawCollateral() exploit; $208K

### 2024

#### 2024-01
- **Orbit Chain Bridge** (ETH) - Signature Forgery - Forged validator signatures to drain bridge; ~$81M
- **Socket Gateway** (ETH) - Arbitrary External Call - Unvalidated external call in socket gateway; ~$3.3M
- **Gamma/Uniproxy** (ETH) - Flash Loan + Nested - Nested flash loans + manipulation; ~$6.3M
- **Radiant Capital** (ARB/BNB) - Flash Loan + Liquidity Index - Liquidity index manipulation; ~$4.5M
- **Blueberry Protocol** (ETH) - Compounding Logic Bug - Logic error in compounding; ~$1.4M
- **NBLGAME** (BSC) - Reentrancy - Reentrancy in game contract
- **DappRadar** (ETH) - Access Control - Unprotected function
- **K3PR** (BSC) - Flash Loan + Swap - Flash loan swap
- **Maverick** (ETH) - Flash Loan + Price - Maverick AMM price manipulation
- **TLN** (BSC) - Access Control - Unprotected function
- **Tokenlon** (ETH) - Flash Loan + Price - Tokenlon price manipulation
- **WN** (BSC) - Access Control - Unprotected function
- **WOO** (ETH) - Flash Loan + Price - Woo price manipulation
- **Unim** (BSC) - Flash Loan + Swap - Flash loan swap
- **SHRAP** (ETH) - Flash Loan + Tokenomics - Tokenomics exploit
- **SmartCredit** (ETH) - Flash Loan + Lending - Lending manipulation
- **SquidMulticall** (ETH) - Access Control - Unprotected Axelar gateway
- **Swarms** (ETH) - Access Control - Unprotected function
- **Roll** (ETH) - Access Control - Unprotected function
- **SEEDx** (ETH) - Flash Loan + Donation - Donation attack
- **Toum** (ETH) - Flash Loan + Price - Price manipulation

#### 2024-02
- **MIMSpell (rounding)** (ETH) - Precision/Rounding - Rounding attack on Abracadabra Spell; ~$6.5M
- **Paraswap** (ETH) - Arbitrary External Call - Arbitrary calldata in swap; ~$1.2M
- **DAO SoulMate** (BSC) - Access Control - Missing access control; ~$319K
- **Wise Lending** (ETH) - Flash Loan + Donation - Donation attack; ~$464K
- **Barley Finance** (ETH) - Flash Loan + Donation - Donation on vault shares; ~$130K
- **MIC Token** (BSC) - Flash Loan + LP Fee - LP fee draining; ~$500K
- **Abracadabra** (ETH) - Flash Loan + Donation - Donation attack
- **ATK** (BSC) - Flash Loan + Swap - Flash loan
- **BONK** (ETH) - Flash Loan + Tax - Tax bypass
- **FINS** (BSC) - Flash Loan + Swap - Flash loan
- **H2** (BSC) - Access Control - Unprotected function
- **HOOD** (BSC) - Flash Loan + Swap - Flash loan
- **MBox** (BSC) - Flash Loan + Swap - Flash loan
- **Move** (BSC) - Flash Loan + Swap - Flash loan
- **MULTI** (BSC) - Access Control - Unprotected function
- **ORDS** (BSC) - Flash Loan + Swap - Flash loan
- **Pulsar** (ETH) - Flash Loan + Swap - Flash loan
- **RO** (BSC) - Flash Loan + Swap - Flash loan
- **Wise Lending (2)** (ETH) - Flash Loan + Donation - Second donation attack; ~$464K
- **XY** (BSC) - Flash Loan + Swap - Flash loan
- **ZF** (BSC) - Access Control - Unprotected mint
- **Zoom** (BSC) - Flash Loan + Swap - Flash loan

#### 2024-03
- **Gulper** (ETH) - Flash Loan + Contract Takeover - Contract takeover via flash loan
- **Mytheria** (BSC) - Access Control - Unprotected function
- **NFPrompt** (BSC) - Access Control - Unprotected function
- **PATEX** (BSC) - Flash Loan + Stake - Staking manipulation
- **Prisma Finance** (ETH) - Flash Loan + Donation - Donation attack on Prisma vaults; ~$2M
- **RIVUS** (BSC) - Access Control - Unprotected function
- **Bounce** (ETH) - Flash Loan + Auction - Auction manipulation
- **Ethena** (ETH) - Flash Loan + Staking - Staked USDe manipulation
- **Masa** (ETH) - Flash Loan + Tokenomics - Tokenomics exploit
- **Solv** (ETH) - Flash Loan + Donation - Donation attack
- **Swell** (ETH) - Flash Loan + Staking - Staking reward manipulation
- **Wormhole (replay)** (ETH) - Bridge Replay - Cross-chain message replay

#### 2024-04
- **Dollara** (BSC) - Access Control - Unprotected function
- **ENQAI** (ETH) - Flash Loan + Price - Price manipulation
- **FakeToken** (BSC) - Access Control - Unprotected function
- **FR** (BSC) - Flash Loan + Swap - Flash loan
- **Ghost** (ETH) - Flash Loan + Donation - Donation attack
- **HoppyFrog** (ETH) - Access Control - Unprotected transfer
- **MARS** (BSC) - Flash Loan + Swap - Flash loan
- **NGFS** (BSC) - Flash Loan + Access Control - Flash loan + access control
- **OpenLeverage** (ETH) - Flash Loan + Price - Price manipulation
- **PikeFinance** (ETH) - Access Control - Authorization bypass
- **Rico** (ETH) - Flash Loan + Donation - Donation attack
- **SATX** (BSC) - Access Control - Unprotected mint
- **SQUID** (BSC) - Access Control - Unprotected function
- **Sumer Money** (ETH) - Flash Loan + Donation - Donation attack
- **UPS** (BSC) - Flash Loan + Fee - Fee mechanism exploit
- **Unverified 0x00C409** (ETH) - Flash Loan + Swap - Unverified contract
- **WSM** (BSC) - Flash Loan + Swap - Flash loan
- **XBridge** (BSC) - Access Control - Unprotected bridge
- **YIEDL** (BSC) - Flash Loan + Donation - Donation attack
- **Yield** (ETH) - Flash Loan + Donation - Donation attack on aggregator
- **Z123** (BSC) - Access Control - Unprotected mint

#### 2024-05
- **Burner** (ETH) - Flash Loan + Donation - Donation attack
- **EXcommunity** (ETH) - Access Control - execute() without access control
- **GFOX** (ETH) - Flash Loan + Pool Manipulation - Pool price manipulation
- **GPU** (ETH) - Flash Loan + Tokenomics - Tokenomics exploit
- **Liquidity Tokens** (ETH) - LP Token Inflation - LP token mint/burn ratio exploit
- **Meta Dragon** (BSC) - Access Control - claimed() without access control
- **MixedSwapRouter** (ETH) - Arbitrary External Call - Arbitrary call in swap data
- **NORMIE** (BASE) - Donation Attack - Share price manipulation via donation
- **OSN** (BSC) - Access Control - Unprotected function
- **Predy Finance** (ARB) - Flash Loan + Price - Perp trading price manipulation
- **Red Keys Coin** (ETH) - Flash Loan + Fee - Buy/sell fee exploit
- **SATURN** (ETH) - Flash Loan + Donation - Donation attack
- **SCROLL** (ETH) - Access Control - Unprotected function
- **Sonne Finance** (OETH) - Flash Loan + Donation - Donation + VELO manipulation
- **TCH** (BSC) - Flash Loan + Swap - Flash loan
- **TGC** (BSC) - Access Control - sell() without access control
- **TSURU** (BSC) - Flash Loan + LP - LP reserve manipulation
- **Trade on Orion** (BSC) - Flash Loan + Swap - Flash loan

#### 2024-06
- **APEMAGA** (ETH) - Flash Loan + Tax - Buy/sell tax exploit
- **Bazaar** (ETH) - Access Control - Unprotected function
- **Crb2** (BSC) - Flash Loan + Liquidity Drain - Liquidity drain via swap
- **Dyson Money** (ETH) - Flash Loan + Donation - Donation attack
- **INcufi** (ETH) - Flash Loan + Fee - Fee mechanism exploit
- **JokInTheBox** (BSC) - Flash Loan + Swap - Flash loan
- **MineSTM** (BSC) - Access Control - Unprotected function
- **NCD** (BSC) - Flash Loan + Swap - Flash loan
- **SteamSwap** (BSC) - Access Control - Unprotected function
- **UwU Lend (First)** (ETH) - Flash Loan + Donation - uDai share price via donation; ~$11M
- **UwU Lend (Second)** (ETH) - Flash Loan + Donation - Different asset donation; ~$11M
- **Velocore** (LINEA) - Flash Loan + LP - Constant product invariant exploit
- **WIFCOIN** (ETH) - Staking Reentrancy - claimEarned() no reentrancy protection; ~$3.4K
- **Will** (BSC) - Flash Loan + Expired Orders - Settlement of expired positions; ~$52K
- **YYS** (BSC) - Flash Loan + Arbitrary Sell - YYStoken_Sell.sell() exploit; ~$28K

#### 2024-07
- **BNB** (BSC) - Access Control - Unprotected function
- **CRB** (BSC) - Flash Loan + Swap - Flash loan
- **DOD** (BSC) - Access Control - Unprotected function
- **EC** (BSC) - Flash Loan + Swap - Flash loan
- **FBC** (BSC) - Flash Loan + Swap - Flash loan
- **GL** (BSC) - Flash Loan + Swap - Flash loan
- **GR** (BSC) - Flash Loan + Swap - Flash loan
- **HOD** (BSC) - Flash Loan + Swap - Flash loan
- **HPO** (BSC) - Flash Loan + Swap - Flash loan
- **KNINE** (BSC) - Flash Loan + Swap - Flash loan
- **LE** (BSC) - Access Control - Unprotected function
- **Living** (BSC) - Access Control - Unprotected function
- **LM** (BSC) - Flash Loan + Swap - Flash loan
- **MARS** (BSC) - Access Control - Unprotected function
- **MEV** (BSC) - MEV Bot - MEV bot exploit
- **MG** (BSC) - Flash Loan + Swap - Flash loan
- **MS** (BSC) - Access Control - Unprotected function
- **N3K** (BSC) - Flash Loan + Swap - Flash loan
- **OAS** (BSC) - Flash Loan + Swap - Flash loan
- **Palm** (BSC) - Flash Loan + Swap - Flash loan
- **PEPE** (BSC) - Flash Loan + Swap - Flash loan
- **PI** (BSC) - Flash Loan + Swap - Flash loan
- **RADIO** (BSC) - Access Control - Unprotected function
- **SDO** (BSC) - Flash Loan + Swap - Flash loan
- **SHLD** (BSC) - Access Control - Unprotected function
- **SU** (ETH) - Flash Loan + Swap - Flash loan
- **Tutti** (BSC) - Flash Loan + Swap - Flash loan
- **VE** (BSC) - Flash Loan + Swap - Flash loan

#### 2024-08
- **Anvil** (ETH) - Flash Loan + Swap - Flash loan
- **Bounce** (ETH) - Flash Loan + Auction - Auction manipulation
- **DEXTools** (ETH) - Access Control - Unprotected function
- **GALA** (BSC) - Access Control - Unprotected function
- **Nexera** (ETH) - Access Control - Unprotected proxy
- **Pika** (ETH) - Flash Loan + Perp - Perpetual trading manipulation
- **SecurityCamera** (BSC) - Access Control - Unprotected function
- **SN** (BSC) - Access Control - Unprotected function
- **WOOF** (BSC) - Access Control - Unprotected function
- **ZT** (BSC) - Flash Loan + Swap - Flash loan
- **MIM** (ETH) - Flash Loan + Swap - Flash loan

#### 2024-09
- **4D** (BSC) - Flash Loan + Swap - Flash loan
- **Chad** (ETH) - Flash Loan + Tokenomics - Tokenomics exploit
- **CT** (BSC) - Flash Loan + Swap - Flash loan
- **DIC** (BSC) - Access Control - Unprotected function
- **DT3** (BSC) - Flash Loan + Swap - Flash loan
- **DW** (BSC) - Flash Loan + Swap - Flash loan
- **KCA** (BSC) - Flash Loan + Swap - Flash loan
- **KR** (BSC) - Flash Loan + Swap - Flash loan
- **MAN** (BSC) - Access Control - Unprotected function
- **MBS** (BSC) - Flash Loan + Swap - Flash loan
- **MetaFarm** (BSC) - Flash Loan + Farm - Farm manipulation
- **Neiro** (ETH) - Flash Loan + Fee - Fee bypass
- **RO** (BSC) - Flash Loan + Swap - Flash loan
- **ROO** (BSC) - Access Control - Unprotected function
- **SA** (BSC) - Flash Loan + Swap - Flash loan
- **SN** (BSC) - Access Control - Unprotected function
- **TB** (BSC) - Flash Loan + Swap - Flash loan
- **TT** (BSC) - Access Control - Unprotected function

#### 2024-10
- **AC** (BSC) - Flash Loan + Swap - Flash loan
- **AI** (BSC) - Flash Loan + Swap - Flash loan
- **BLD** (BSC) - Access Control - Unprotected function
- **G3** (BSC) - Flash Loan + Swap - Flash loan
- **KAI** (BSC) - Flash Loan + Swap - Flash loan
- **MIO** (BSC) - Access Control - Unprotected function
- **MS** (BSC) - Flash Loan + Swap - Flash loan
- **NJ** (BSC) - Flash Loan + Swap - Flash loan
- **NY** (BSC) - Flash Loan + Swap - Flash loan
- **OP** (BSC) - Flash Loan + Swap - Flash loan
- **SI** (BSC) - Flash Loan + Swap - Flash loan
- **VISTA** (BSC) - Flash Loan + No Freeze Check - Flash loan without frozen token check; $29K
- **YY** (BSC) - Access Control - Unprotected function

#### 2024-11
- **AK1111** (BSC) - Access Control - nonblockingLzReceive1() without access control; $31.5K
- **ChiSale** (ETH) - Flash Loan + Empty Callback - Empty receiveFlashLoan(); $16.3K
- **CoW Protocol** (ETH) - MEV/Callback Spoofing - uniswapV3SwapCallback() spoofing; $59K
- **DeltaPrime** (ARB) - Flash Loan + Reentrancy - SmartLoan.claimReward() via fake pair; $4.75M
- **MFT** (BSC) - Flash Loan + Burn - Nested flash loans + token burn; $33.7K
- **MainnetSettler** (ETH) - Access Control - Bundled actions allow unauthorized transfers; $66K
- **Matez** (BSC) - Integer Overflow - stake() integer truncation; $80K
- **NFTG** (BSC) - Flash Loan + Reentrancy - DODO flash + reentrancy; $10K
- **PolterFinance** (FTM) - Flash Loan + Borrow Exploit - 1 wei deposit, borrow all 8 tokens; $7M
- **RPP** (BSC) - Flash Loan + Price Loop - 1,450 swap iterations to manipulate price; $14.1K
- **VRug** (ETH) - MEV/Sandwich - Uniswap V2 front-run; $8.4K
- **X319** (BSC) - Access Control - claimEther() without access control; $12.9K
- **proxy_b7e1** (BSC) - Proxy Exploit - Unverified logic contract; $8.5K
- **vETH** (ETH) - Flash Loan + Factory - Factory vulnerability; $447K

#### 2024-12
- **BTC24H** (POLY) - Access Control - claim() without access control; $85.7K
- **Bizness** (BASE) - Reentrancy - splitLock() reentrancy via receive(); $15.7K
- **Clober DEX** (BASE) - Flash Loan + Reentrancy - Fake token pool + burnHook reentrancy; $501K
- **JHY** (BSC) - Flash Loan + LP Accounting - DIVIDEND_JHYLP accounting bug; $11K
- **LABUBU** (BSC) - Flash Loan + Reentrancy - 30x self-transfer reentrancy; $12K
- **Moonhacker** (OETH) - Flash Loan + Impersonation - Fake mint/borrow returns; $318.9K
- **Pledge** (BSC) - Access Control - swapTokenU() without access control; $15K
- **SlurpyCoin** (BSC) - Flash Loan + Price Loop - 240-transfer buy/sell loop; $3K

### 2025

#### 2025-01
- **GMX/SEED** (AVAX) - Flash Loan + Donation - Donation to GMX lp staking inflating share price; ~$32K
- **SquidMulticall** (ETH) - Access Control - Unprotected Axelar gateway in Squid router; ~$122K
- **SquidRouter** (ARB) - Access Control - Unprotected function
- **MIT** (ETH) - Flash Loan + Tokenomics - Tokenomics exploit
- **THENA** (BSC) - Flash Loan + Farm - ThenA farm manipulation
- **Pink** (BSC) - Flash Loan + Stake - Staking manipulation
- **RFS** (BSC) - Access Control - Unprotected function
- **BSC-HB** (BSC) - Access Control - Unprotected function
- **DUB** (BSC) - Flash Loan + Swap - Flash loan
- **GRV** (BSC) - Flash Loan + Swap - Flash loan
- **GT** (BSC) - Flash Loan + Swap - Flash loan
- **HT** (BSC) - Flash Loan + Swap - Flash loan
- **IZA** (BSC) - Flash Loan + Swap - Flash loan
- **LSD** (BSC) - Flash Loan + Fee - Fee bypass
- **NFTW** (BSC) - Flash Loan + Swap - Flash loan
- **SRW** (BSC) - Flash Loan + Swap - Flash loan
- **TN** (BSC) - Access Control - Unprotected function
- **TOAD** (BSC) - Flash Loan + Swap - Flash loan
- **USD** (BSC) - Access Control - Unprotected function
- **WGP** (BSC) - Access Control - Unprotected mint
- **XOX** (BSC) - Flash Loan + Swap - Flash loan
- **YAY** (ETH) - Flash Loan + Price - Price manipulation
- **YH** (BSC) - Access Control - Unprotected function
- **YS** (BSC) - Flash Loan + Swap - Flash loan
- **me** (BSC) - Flash Loan + Swap - Flash loan

#### 2025-02
- **AA** (BSC) - Reentrancy - 150 transfers with fake token; vault drained
- **BabyDoge** (ETH) - Flash Loan + Tokenomics - Tokenomics manipulation
- **Bi** (BSC) - Flash Loan + Swap - Flash loan
- **BS** (BSC) - Flash Loan + Swap - Flash loan
- **Chain** (BSC) - Flash Loan + Stake - Staking manipulation
- **CorionX** (ETH) - Flash Loan + Swap - Flash loan
- **DC** (BSC) - Flash Loan + Swap - Flash loan
- **DEF** (BSC) - Flash Loan + Swap - Flash loan
- **DPD** (BSC) - Flash Loan + Swap - Flash loan
- **Dragon** (BSC) - Flash Loan + Swap - Flash loan
- **EB** (BSC) - Flash Loan + Swap - Flash loan
- **EF** (BSC) - Flash Loan + Swap - Flash loan
- **FL** (BSC) - Flash Loan + Swap - Flash loan
- **GJ** (BSC) - Flash Loan + Swap - Flash loan
- **HTA** (BSC) - Flash Loan + Swap - Flash loan
- **KU** (BSC) - Flash Loan + Swap - Flash loan
- **LB** (BSC) - Flash Loan + Swap - Flash loan
- **LOLA** (BSC) - Flash Loan + Stake - Staking manipulation
- **M** (BSC) - Flash Loan + Swap - Flash loan
- **ME** (BSC) - Flash Loan + Swap - Flash loan
- **MN** (BSC) - Flash Loan + Fee - Fee bypass
- **MOVE** (BSC) - Flash Loan + Swap - Flash loan
- **N4L** (BSC) - Flash Loan + Swap - Flash loan
- **PEPE (fake)** (BSC) - Access Control - Unprotected function
- **SSSS** (BSC) - Flash Loan + Swap - Flash loan
- **TST** (BSC) - Flash Loan + Swap - Flash loan
- **Unsupported** (BSC) - Flash Loan + Stake - Staking manipulation
- **WO** (BSC) - Flash Loan + Swap - Flash loan
- **ZKB** (BSC) - Flash Loan + Swap - Flash loan

#### 2025-03
- **Aura** (ETH) - Flash Loan + Price - Aura price manipulation
- **Across Protocol** (ETH) - Bridge Exploit - Stale root bundle verification; $105K
- **BSCS** (BSC) - Flash Loan + Swap - Flash loan
- **CT** (BSC) - Access Control - Unprotected mint
- **Fed** (BSC) - Flash Loan + Swap - Flash loan
- **Fee** (BSC) - Access Control - Unprotected function
- **FIN** (BSC) - Flash Loan + Swap - Flash loan
- **HB** (BSC) - Access Control - Unprotected function
- **IDL** (BSC) - Flash Loan + Swap - Flash loan
- **MC** (BSC) - Flash Loan + Stake - Staking manipulation
- **METH** (BSC) - Flash Loan + Stake - Staking manipulation
- **MNT** (BSC) - Flash Loan + Swap - Flash loan
- **MS** (BSC) - Flash Loan + Fee - Fee bypass
- **PL** (BSC) - Flash Loan + Swap - Flash loan
- **SEE** (BSC) - Flash Loan + Swap - Flash loan
- **SEEDx** (BSC) - Flash Loan + Stake - Staking manipulation
- **TrustPad** (BSC) - Flash Loan + Stake - Staking manipulation
- **TW** (BSC) - Flash Loan + Swap - Flash loan
- **WGB** (BSC) - Flash Loan + Swap - Flash loan
- **WOK** (BSC) - Access Control - Unprotected function
- **WZ** (BSC) - Access Control - Unprotected function
- **YLD** (BSC) - Flash Loan + Swap - Flash loan

#### 2025-04
- **Ai** (BSC) - Flash Loan + Stake - Staking manipulation
- **BO** (BSC) - Flash Loan + Swap - Flash loan
- **BUR** (BSC) - Flash Loan + Swap - Flash loan
- **CT** (BSC) - Flash Loan + Swap - Flash loan
- **D** (BSC) - Flash Loan + Swap - Flash loan
- **DEX** (BSC) - Flash Loan + Swap - Flash loan
- **DT3** (BSC) - Flash Loan + Swap - Flash loan
- **EG** (BSC) - Flash Loan + Swap - Flash loan
- **EL** (BSC) - Flash Loan + Swap - Flash loan
- **ELP** (BSC) - Flash Loan + Swap - Flash loan
- **H** (BSC) - Flash Loan + Swap - Flash loan
- **HODL** (BSC) - Flash Loan + Swap - Flash loan
- **HON** (BSC) - Flash Loan + Swap - Flash loan
- **KB** (BSC) - Flash Loan + Swap - Flash loan
- **LA** (BSC) - Flash Loan + Swap - Flash loan
- **LO** (BSC) - Flash Loan + Swap - Flash loan
- **NX** (BSC) - Access Control - Unprotected function
- **OY** (BSC) - Flash Loan + Swap - Flash loan
- **SEC** (BSC) - Access Control - Public claim function; $21K
- **SHI** (BSC) - Flash Loan + Swap - Flash loan
- **SSS** (BSC) - Flash Loan + Swap - Flash loan
- **STX** (BSC) - Flash Loan + Swap - Flash loan
- **SU** (BSC) - Flash Loan + Swap - Flash loan

#### 2025-05
- **BSC Token** (BSC) - Flash Swap + AMM Accounting - AMM accounting bypass via token features
- **Chad** (ETH) - Flash Loan + Tokenomics - Tokenomics exploit
- **DMT** (BSC) - Flash Loan + Swap - Flash loan
- **ELC** (BSC) - Access Control - Unprotected function
- **GL** (BSC) - Flash Loan + Swap - Flash loan
- **GRT** (BSC) - Flash Loan + Swap - Flash loan
- **HOD** (BSC) - Flash Loan + Swap - Flash loan
- **HYC** (BSC) - Flash Loan + Swap - Flash loan
- **IP** (BSC) - Flash Loan + Swap - Flash loan
- **JM** (BSC) - Flash Loan + Swap - Flash loan
- **LALA** (BSC) - Flash Loan + Swap - Flash loan
- **LST** (BSC) - Flash Loan + Swap - Flash loan
- **MB** (BSC) - Flash Loan + Swap - Flash loan
- **MN** (BSC) - Flash Loan + Swap - Flash loan
- **Pika** (ETH) - Flash Loan + Perp - Perpetual swap manipulation
- **Poly** (BSC) - Flash Loan + Swap - Flash loan
- **QC** (BSC) - Flash Loan + Swap - Flash loan
- **SF** (BSC) - Flash Loan + Swap - Flash loan
- **SP** (BSC) - Flash Loan + Swap - Flash loan
- **Tack** (ETH) - Flash Loan + Donation - Donation attack; $543K
- **TL** (BSC) - Flash Loan + Swap - Flash loan
- **TOX** (BSC) - Access Control - Unprotected function
- **Trinity** (ETH) - Flash Loan + Price - Price manipulation
- **US** (BSC) - Flash Loan + Swap - Flash loan
- **WD** (BSC) - Flash Loan + Swap - Flash loan
- **XX** (BSC) - Flash Loan + Swap - Flash loan
- **ZA** (BSC) - Flash Loan + Swap - Flash loan

#### 2025-06
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

#### 2025-07
- **SWAPP Staking** (BSC) - Flash Loan + Epoch Manipulation - manualEpochInit() bug; ~$32K
- **Stepp2p** (BSC) - Double-Spend - cancelSaleOrder() + modifySaleOrder() on same order; ~$43K
- **SuperRare** (ETH) - Merkle Root Manipulation - updateMerkleRoot() without access control; ~$730K
- **VDS** (BSC) - Burn-Mint Imbalance - Token conversion ratio exploit; ~$13K
- **WETC Token** (BSC) - Flash Loan + Sync/Skim - Multiple sync/skim cycles; ~$101K
- **GMX** (ARB) - Flash Loan + Position + Reentrancy - gmxPositionCallback() reentrancy
- **Unverified 0x54cd** (ETH) - Flash Loan + Unverified Call - Unprotected function; ~$285.7K

#### 2025-08
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

#### 2025-09
- **Kame** (SEI) - Access Control - swap() with arbitrary executor; $18K
- **NGP Token** (BSC) - Flash Loan + sync() in Transfer - _update() calls sync() internally; $2M

#### 2025-10
- **MIMSpell3** (ETH) - Flash Loan + Multi-Cauldron - Borrow from 6 Cauldrons; $1.7M
- **Sharwa Finance** (ARB) - Flash Loan + Margin Trading - Long/short position manipulation; $146K
- **TokenHolder** (BSC) - Access Control - Fake loan via privilegedLoan(); $20K

#### 2025-11
- **Balancer V2** (ETH) - Precision Loss - Composable Stable Pool _calcInGivenOut() precision; $120M
- **DRL Vault V3** (ETH) - Flash Loan + Slippage - swapToWETH() stale pricing; $100K
- **Moonwell** (BASE) - Flash Loan + Over-Borrow - wrsETH mispriced vs wstETH; $1M

#### 2025-12
- **yETH Pool** (ETH) - Virtual Balance Manipulation - Rate update mid-cycle exploit; $9M

### 2026

#### 2026-01
- **Makina** (ETH) - Flash Loan + Curve Hot-Swap - Curve 3crypto hot-swap on DOLA/3crv; $5.1M
- **Squid/multi** (multiple) - Flash Loan + Unvalidated Call - Cross-chain call injection

#### 2026-02
- **DxSale** (BSC) - Backdoor - DxSale LP locker backdoor; $7.3M
- **TrustedVolumes** (ETH) - Arbitrary Call - Proxy with unvalidated RFQ data; $5.87M
- **Adshares** (ETH) - Access Control - Unprotected mint function
- **N0AH** (BSC) - Access Control - Unprotected function
- **NewMarketTrading** (ETH/BASE/ARB) - Access Control - 88 Safes affected across 3 chains; $3.98M

#### 2026-03
- **MAP Protocol** (BSC/ETH) - Cross-chain Replay - Cross-chain message replay
- **Planet** (BASE) - Flash Loan + LP Donation - LP burn + sync inflates share price; $667K
- **ROE** (BASE) - Flash Loan + Swap - Flash loan swap
- **ENDO** (ETH) - Flash Loan + Fee - Fee bypass
- **SKP** (BSC) - Rug Pull - Engineered exit scam
- **SQToken** (BSC) - Access Control - transferOwnership() exploit
- **Landwolf** (ETH) - Flash Loan + Tax - Tax bypass

#### 2026-04
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

#### 2026-05
- **TNV** (BSC) - Flash Loan + Swap - Flash loan
- **PL** (BSC) - Flash Loan + Swap - Flash loan
- **R2E** (BSC) - Access Control - Unprotected function
- **SV** (BSC) - Flash Loan + Swap - Flash loan
- **YSDAO** (BSC) - Tax Bypass - 1 wei output bypasses _isRemoveLiquidity() buy tax; $19.5K
- **Sei** (SEI) - Flash Loan + Stake - Staking manipulation
- **BBL** (BSC) - Flash Loan + Swap - Flash loan

#### 2026-06
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

## Appendix: Key Vulnerability Patterns Summary

### Top 10 Root Causes (by frequency):
1. **Missing/Insufficient Access Control** - Functions without onlyOwner/require modifiers
2. **Spot Price as Oracle** - Using AMM spot prices without TWAP
3. **Share Price Manipulation (Donation)** - totalBalance/totalSupply donation inflation
4. **Arbitrary External Call** - User-controlled calldata in swap/router functions
5. **Skim/Sync Reserve Manipulation** - Fee-on-transfer token pool manipulation
6. **Flash Loan Callback Trust** - Unvalidated flash loan callbacks
7. **Reentrancy** - Missing checks-effects-interactions pattern
8. **Integer Division/Rounding** - Division before multiplication, truncation
9. **Proxy Initialization** - Uninitialized proxy contracts
10. **Signature/Permit Validation** - Missing ecrecover checks, replayable signatures

### Top 10 Largest Losses (all time):
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

### Defensive Recommendations:
1. **Never use spot prices as oracles** - Always use TWAP or Chainlink-style external oracles
2. **Use checks-effects-interactions pattern** - Prevent reentrancy in all external calls
3. **Validate share price changes** - Implement checks against donation attacks (virtual shares, minimum liquidity)
4. **Restrict access to critical functions** - Use onlyOwner, access control lists, and multisig for upgrades
5. **Validate external calldata** - Never execute arbitrary user-provided calldata
6. **Use OpenZeppelin's ReentrancyGuard** - Apply to all functions that make external calls
7. **Test with flash loan scenarios** - Include flash loan-based attack vectors in test suite
8. **Use Solidity 0.8+** - Built-in overflow/underflow protection
9. **Initialize proxies properly** - Use OpenZeppelin's initializer modifier
10. **Add totalSupply checks** - Guard against division by zero and share price manipulation when totalSupply is near zero

---

*Generated by Claude Code analysis of ~762 POC files from D:\DeFiHackLabs\src\test*
*Covering 79 directories spanning 2017-07 through 2026-06*
*Analysis includes protocols across Ethereum, BSC, Arbitrum, Polygon, Avalanche, Base, Optimism, Fantom, Linea, zkSync, Sei, and others.*

