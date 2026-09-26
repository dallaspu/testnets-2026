# Testnets in 2026 — which one to use

**An annotated link list.** Two things changed, and they are different things:

1. **On L1, almost every testnet named in old material is switched off.** Goerli, Rinkeby, Ropsten, Kovan and Morden are gone; Holesky was deprecated September 2025. There are exactly two maintained public testnets: **Sepolia** for application development and **Hoodi** for validator work.
2. **On L2, the testnets exist but their quality varies enormously** — and nothing in a typical "best L2" list tells you which. One of the largest L2s by TVL has a testnet that stopped producing blocks in July 2026 and whose RPC still answers as if nothing is wrong.

This page is links, plus the specific warnings that save you an afternoon. ⛔ None of the projects below are mine. Every number here is a reading from a live public RPC on **2026-09-24**.


---

## ⛔ First: pick the testnet, not the chain

**Most L2 comparison tables answer the mainnet question and assume the testnet follows. It does not follow.** Several strong mainnets have weak or dead testnets, and a chain's TVL tells you nothing about whether its testnet works.

That matters because the testnet is where you spend your time. You will deploy to it dozens of times for every one deployment to mainnet — so a chain with great liquidity and a broken testnet is a chain you cannot actually build on.

| If you are… | Use | chain ID |
|:--|:--|--:|
| building an application, deploying contracts | **Sepolia** | `11155111` |
| running a validator, testing client or protocol changes | **Hoodi** | `560048` |
| choosing an L2 testnet | see the table below — **it depends** | — |

⛔ **Do not use** Goerli, Rinkeby, Ropsten, Kovan, Mumbai or Holesky. [Ethereum's own docs](https://ethereum.org/en/developers/docs/networks/) name Sepolia and Hoodi as the only two maintained public testnets; the endpoints above were probed directly, and `goerli.etherscan.io`, `rinkeby.etherscan.io`, `kovan.etherscan.io` and `holesky.etherscan.io` all fail to resolve, with `sepolia.etherscan.io` answering `200` from the same machine in the same second as the control.

**Why the old ones died the way they did:** most of them were not retired on their own schedule — they were anchored to something else that retired first. Goerli was the anchor, and when it went, the testnets built on top of it went with it. That is why a list can name "Mumbai" next to "Sepolia" as if they were alternatives of the same kind; they are not, and the difference is a chain of dependencies rather than a change of preference.

---

## L2 testnets

| L2 | Testnet | chain ID | Docs | ⚠️ |
|:--|:--|--:|:--|:--|
| Base | Base Sepolia | `84532` | [docs.base.org](https://docs.base.org/) | by far the most liquidity on mainnet |
| Arbitrum | Arbitrum Sepolia | `421614` | [docs.arbitrum.io](https://docs.arbitrum.io/) · [explorer](https://sepolia.arbiscan.io/) | fastest, but see the warning below |
| OP Mainnet | OP Sepolia | `11155420` | [docs.optimism.io](https://docs.optimism.io/) · [explorer](https://sepolia-optimism.etherscan.io/) | gas at the floor |
| Unichain | Unichain Sepolia | `1301` | [docs.unichain.org](https://docs.unichain.org/) | gas at the floor |
| Ink | Ink Sepolia | `763373` | [docs.inkonchain.com](https://docs.inkonchain.com/) | gas at the floor |
| Soneium | Soneium Minato | `1946` | [docs.soneium.org](https://docs.soneium.org/) | gas at the floor |
| MegaETH | MegaETH Testnet | `6343` | [docs.megaeth.com](https://docs.megaeth.com/) | ⚠️ the old testnet was `6342` — one digit apart |
| Taiko Alethia | Taiko Hoodi | `167013` | [docs.taiko.xyz](https://docs.taiko.xyz/) | ⚠️ the old one was Taiko Hekla, `167009` |
| World Chain | World Chain Sepolia | `4801` | [docs.world.org](https://docs.world.org/) | ⚠️ the old one was `484752` |
| Blast | Blast Sepolia | `168587773` | — | ⚠️ **chain timestamps run ~12h behind**, see below |
| Linea | Linea Sepolia | `59141` | [docs.linea.build](https://docs.linea.build/) | real zkVM |
| Abstract | Abstract Testnet | `11124` | [docs.abs.xyz](https://docs.abs.xyz/) | slower than L1 |
| ZKsync Era | ZKsync Sepolia | `300` | [docs.zksync.io](https://docs.zksync.io/) | ⚠️ **slower than the L1 it settles to** — pick it for ZK Stack behaviour, not speed |
| Starknet | Starknet Sepolia | `393402133025997798000961` | [docs.starknet.io](https://docs.starknet.io/) | ⚠️ 24-digit chain ID — read it from a registry, never from memory |
| Mantle | Mantle Sepolia | `5003` | [docs.mantle.xyz](https://docs.mantle.xyz/) | 🔴 **50 gwei — about 50,000× OP Sepolia.** "Test transactions are free" is false here |
| Scroll | Scroll Sepolia | `534351` | [docs.scroll.io](https://docs.scroll.io/en/home/) | 🔴 **frozen since 2026-07-16 — do not use** |

⚠️ **This table has no total order, and a table that hands you one answer is hiding the trade-off.** Which is "best" depends on what you are building.

**How to read the columns that are easy to skip:**

- **Gas price is the one that bites.** Five chains sit at the same **0.001 gwei** floor, where a test transaction is genuinely free in any practical sense. Mantle is at **50 gwei** — roughly **50,000×** that. If your contract loops, or you run a test suite that deploys repeatedly, that column decides whether a working session takes minutes or stalls on a funding problem.
- **Block time is not speed you feel.** Arbitrum produces blocks on demand, so the number you measure depends on how busy the window was. A 0.25s reading and a 2s reading can both be correct for the same chain.
- **Chain ID is not decoration.** Starknet's testnet ID is 24 digits. Read it from a registry rather than retyping it — a hand-copied config gets this wrong silently and then fails somewhere unrelated.

⭐ **For chain IDs and RPC endpoints, read [ethereum-lists/chains](https://github.com/ethereum-lists/chains)** (★9,830 · 2026-09-26) or **[Chainlist](https://chainlist.org/)**, rather than copying from a blog post. ⚠️ But note it lists dead networks as `active` — **the registry is not a liveness oracle.**

---

## 🔴 Two things no comparison table will show you

Both of these were found by measuring, and neither is announced anywhere obvious. They are the reason this page exists.

### Scroll Sepolia has not produced a block since 2026-07-16

Head block `19039813`, timestamp `2026-07-16 07:05:07 UTC`, measured on **2026-09-24** — a gap of **~70 days**, identical on two independent providers (`scroll-sepolia.publicnode.com`, `scroll-sepolia.drpc.org`), with **no head movement over a 90-second window**, while Scroll **mainnet** produced normally at the same moment.

**The RPC still answers `eth_chainId`. It still returns `eth_getBlockByNumber`. If your only test is "does the endpoint respond", it passes.** The chain is frozen and the API does not say so.

No announcement was made, and the chain-ID registry still lists `534351` as `active` — stop trusting either one as a liveness check. **Treat the testnet as unusable.**

⭐ **Note the shape of this failure, because it is the reason this page exists.** The registry said `active`. The endpoint said `200`. Both checked, both green, both wrong — because neither one is a liveness check. **The only thing that catches a frozen chain is comparing the head block's timestamp to the clock.**

And a block-time measurement does not catch it: run a block-time script against frozen Scroll Sepolia and it hands you a perfectly normal-looking **6.2s**, computed from the last 200 blocks *before* the freeze. The measurement is correct. It answers "how fast did this chain produce blocks", not "is it producing them now", and those are different questions.

**What it looks like from your side, if you deploy to it:** the transaction is accepted, sits in the mempool, never gets included, and your tooling reports a pending transaction rather than an error. Nothing tells you the chain has been dead for two months.

### Blast Sepolia's chain timestamps run ~12 hours behind

Blocks advance at a normal 2.00s, but the head block's `timestamp` reads **~12.1 hours in the past**. Blast *mainnet* reads on time. Reproduced on two independent providers. If your contract is time-dependent — cooldowns, deadlines, vesting, `block.timestamp` comparisons in tests — that offset changes its behaviour here relative to every other chain you test on.

This one is nastier than it sounds, because it breaks the thing tests are for. A `require(block.timestamp > unlockTime)` that passes on every other chain can fail here, and it fails for a reason that has nothing to do with your code.

---

## Test coins

**"Where do I get test ETH" is a different problem with a different expiry date, and it has whole repositories dedicated to it.** What matters *for choosing a testnet* is only this: **the free options mostly gate on mainnet activity, not on identity**, so a newcomer with an empty mainnet wallet gets the worst queue.

The practical consequence: when you are choosing where to build, "can I actually keep this funded for a week of redeploying" is part of the answer, and it is not the same question as "does the chain work". A chain with cheap gas and a generous faucet is easier to work on than one with neither, even if both are technically fine.

Start from [ethereum.org's learning-tools page](https://ethereum.org/en/developers/learning-tools/) and check anything you plan around before you rely on it — **ethereum.org's own faucet list still links Infura's Sepolia faucet, and that URL now silently redirects to Infura's homepage.**

---

## The chain ID that bites you

| Testnet | chainId | Replaced by | Registry status today |
|:--|--:|:--|:--|
| Ropsten | `3` | Sepolia | still `active` |
| Rinkeby | `4` | Sepolia | still `active` |
| Goerli | `5` | Sepolia / Hoodi | still `active` |
| Optimism Goerli | `420` | OP Sepolia | still `active` |
| Optimism Kovan | `69` | OP Sepolia | still `active` |
| Mumbai | `80001` | Polygon Amoy (`80002`) | still `active` |
| Holesky | `17000` | Hoodi | `incubating` |
| Taiko Hekla | `167009` | Taiko Hoodi (`167013`) | `deprecated` |
| MegaETH (old) | `6342` | `6343` | `deprecated` |

⭐ **Read the last column again — it is the most useful thing here.** Every dead network in the top half is still listed `status: active` in the registry that wallets and tooling actually read. **A green field means nobody filed the paperwork, not that there is a chain at the other end.**

🔴 **And chain ID `42` is the trap.** It used to be Kovan, and every "dead testnet" table still says so. Query the registry today and **`42` is LUKSO Mainnet** — reused for a live, unrelated network. **A script matching on chain ID alone will silently send a transaction to the wrong chain.**

That is worth sitting with for a second: the failure is not that your config is out of date. It is that a number that used to mean one thing now means another, and **both readings are defensible** — the old table and the live registry disagree, and nothing errors either way.

---

## The rest of the 2022 guide, in one table

Testnets are the first thing that breaks in an old tutorial. They are not the only thing.

| A 2022 guide says | In 2026 |
|:--|:--|
| Polygon **Mumbai** (`80001`) | Retired April 2024 → **Amoy** (`80002`). ⚠️ Its native gas token is **POL**, not MATIC — any snippet saying `MATIC` is at least two years stale |
| Install **Truffle + Ganache** | Archived. The 2026 default is **[Foundry](https://github.com/foundry-rs/foundry)** (★10,626 · 2026-09-26) — `anvil` replaces Ganache |
| "Foundry or Hardhat?" | Not either/or: **[Hardhat](https://github.com/NomicFoundation/hardhat)** (★8,508) is at **Hardhat 3** with a Foundry compatibility layer. It is Solidity tests vs TypeScript tests |
| **OpenZeppelin 4.x** imports | 5.x broke them: `Ownable` gained a constructor argument, `increaseAllowance`/`decreaseAllowance` were removed, revert strings became custom errors. That failure lands in the same afternoon as the testnet one |
| "The Merge is next" | Five upgrades ago — Merge (2022-09) → Shanghai (2023-04) → **Dencun (2024-03-13)** → Pectra (2025-05-07) → Fusaka (2025-12-03). **Dencun is the one that made L2 fees drop**, and it is why the gas prices above are as low as they are |

**One more, because it applies directly to the choice above:** a chain's **Stage** is the measure of how much control the operator still has — Stage 2 means the code is the law, Stage 0 means a multisig can still change it. It is the column most comparison tables omit, and it is the one that tells you how much trust you are placing in a bridge. Treat it as part of "which testnet", not as a separate question.

---

## Check any of this yourself, in under a minute

Nothing here requires an account. **The liveness check is the one worth running first** — it is the only check that catches a frozen chain.

```bash
curl -s -X POST https://sepolia.base.org -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["latest",false],"id":1}' \
| python3 -c "
import json,sys,time
b=json.load(sys.stdin)['result']
t=int(b['timestamp'],16); now=int(time.time())
print('head', int(b['number'],16), 'age_hours', round((now-t)/3600,2))"
```

An `age_hours` near zero means the chain is alive; thousands means a museum piece. Run it against any endpoint before you build on it — it is one command, and it is the only one of these checks that a frozen chain cannot pass.

For TVL and stage, the source is L2BEAT: `curl -s --compressed https://l2beat.com/api/scaling/summary`. ⚠️ **`--compressed` is required** — the endpoint serves gzip, and piping compressed bytes to `json.load` fails with a truncation error rather than a clear message, which looks like the endpoint is broken when it is not.

---

## Before you rely on any of this

- **Your existing bytecode may not run unmodified on every L2.** OP Stack chains and Arbitrum run ordinary EVM bytecode; zkSync and Starknet do not. "EVM compatible" is a per-chain claim worth verifying before you assume your contracts port as-is.
- **TVL and stage are L2BEAT's reading**, not each chain's own claim.
- **Every number here is a point in time** — 2026-09-24. A testnet can stop without anyone updating the list that recommends it, and that is the entire point of the two sections above.

---

## If you only take one thing

**"The endpoint answered" is not a liveness check — and a registry saying `active` is not one either.**

Both of them will tell you a chain is fine while it has been dead for ten weeks. The only thing that catches it is comparing the head block's timestamp to the clock.
