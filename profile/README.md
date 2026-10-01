<div align="center">

<img src="https://pippinfor.me/brand/logo-horizontal.png" alt="Pippin's Hollow" width="420" />

### An AI agent running a guarded liquidity vault on Robinhood Chain

**Every number on our site is read from the contracts. Including the ones that look bad.**

<br />

[![Live NAV](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fpippinfor.me%2Fvault.json&query=%24.navUsdg&label=live%20NAV&suffix=%20USDG&style=for-the-badge&color=dfb04b&labelColor=07110c)](https://pippinfor.me/dashboard)
[![Chain](https://img.shields.io/badge/Robinhood%20Chain-4663-07110c?style=for-the-badge&labelColor=07110c&color=93b87d)](https://robinhoodchain.blockscout.com)
[![Contracts verified](https://img.shields.io/badge/contracts-verified%20on%20chain-93b87d?style=for-the-badge&labelColor=07110c)](https://robinhoodchain.blockscout.com/address/0x770600Cd29de10196E2AD5F6985Cf762f8AFe057?tab=contract)
[![Tests](https://img.shields.io/badge/tests-184%20passing-93b87d?style=for-the-badge&labelColor=07110c)](#built-to-be-checked)

[**Open the dashboard →**](https://pippinfor.me/dashboard) &nbsp;·&nbsp; [Read the guardrails](https://pippinfor.me/trust) &nbsp;·&nbsp; [Follow along](https://x.com/pippinrobinhood)

</div>

<br />

<img src="https://pippinfor.me/brand/og-1200x630.png" alt="" width="100%" />

<br />

## What this is

You deposit USDG. You get **Pippin Leaves**, a share token priced at the vault's net asset value divided
by the shares outstanding. An autonomous keeper puts that capital to work as concentrated liquidity on
Uniswap v3, across a three-rung ladder of price ranges, and moves those ranges as the market moves.

Your Leaf count never changes. What changes is what each Leaf is worth.

<br />

## Why we built it differently

Most yield products show you one number and ask you to trust it. The number is usually annualised from
a good week, measured before costs, and quietly assumes nothing goes wrong.

We think that number is the problem. So we built the opposite:

| | |
|---|---|
| **Fee income and price exposure are reported apart** | A liquidity position earns fees *and* holds whatever the market handed it. Those two move independently and often in opposite directions. Blending them into one yield figure hides the larger of the two. |
| **The mark is always the one that favours the vault** | Net asset value is bounded between spot and a time-weighted average. Deposits price at the higher mark, redemptions at the lower. Nobody can profit by nudging the price and transacting. |
| **The keeper spends on arithmetic, not on a timer** | Before every range move it compares the gas cost against the fees that range is expected to earn before it drifts out. If the move does not pay for itself, it does not happen. |
| **Costs are published next to income** | Gas spent, swap friction, realised versus unrealised, all on the dashboard. The uncomfortable rows are not in a footnote. |

<br />

## The ladder

<img src="https://pippinfor.me/brand/hero-underforest.webp" alt="" width="100%" />

Three overlapping ranges rather than one, because a single range stops earning the moment the price
leaves it:

- **Low** sits below the price holding only USDG. If the market falls into it, it buys ETH cheaply.
- **Centre** straddles the price, two-sided, earning now. The narrowest and most capital-efficient, so
  it carries the least, because it is also breached first.
- **High** sits above holding mostly ETH. A rise sells into strength.

Inventory management as a side effect of where liquidity is placed, rather than as a separate strategy
that costs a swap every time it fires.

Range widths are multiples of measured volatility, so the whole ladder widens in a turbulent market and
tightens in a calm one.

<br />

## Built to be checked

> **Nobody can mint your Leaves, move them, or take them.**

The share token's minter is set once at construction and has **no setter**. The vault burns only from
`msg.sender`, never from an address anyone names. Deposits are the only path that mints. There is no
admin override, because an admin override is the same switch an attacker reaches for.

Which cuts both ways: lose your keys and nobody recovers your position. Not us, not a support form, not
a multisig.

**Verified source, on chain:**

| Contract | What it does | |
|---|---|---|
| `PippinVault` | Holds the buffer, prices every Leaf | [view](https://robinhoodchain.blockscout.com/address/0x770600Cd29de10196E2AD5F6985Cf762f8AFe057?tab=contract) |
| `PippinLeaves` | The share token you hold | [view](https://robinhoodchain.blockscout.com/address/0x447b1E5457FE64ECc1690303f991Cb37700FC217?tab=contract) |
| `LiquidityManager` | Moves capital into and out of ranges | [view](https://robinhoodchain.blockscout.com/address/0x20fBC55B2A8ee5d162309cAE5Ca0d307a961f53c?tab=contract) |
| `WithdrawalQueue` | Serves a large exit without a fire sale | [view](https://robinhoodchain.blockscout.com/address/0x30acd7e7A7F16077c971D31EECA45082f0f1D147?tab=contract) |

Every keeper action is bounded by on-chain guardrails it cannot raise: a per-action value-loss ceiling,
a pool concentration cap, a minimum interval between moves, and a tight spot-versus-average band that
refuses to act while the price is dislocated. A compromised keeper is survivable by construction, not
by promise.

<br />

## What is coming in v2

Today the vault takes dollars. **v2 takes ETH as well**, and whichever you put in is what you get back.

- Deposit USDG or ETH, withdraw the same asset. No silent conversion.
- A separate share token for each side.
- Fees route to whichever side earned them, rather than being averaged across both.
- More pairs after the first.

v2 is written and tested and **is not deployed**. It is running as a paper book on live prices first,
funded with 3,000 USDG and 1 ETH, with results published daily per asset. Nothing ships until that
teaches us something.

<div align="center">
<br />
<img src="https://pippinfor.me/brand/pippin-mascot.webp" alt="" width="160" />
<br /><br />
<strong>Paper first. Measured in public. Shipped when the numbers say so.</strong>
<br /><br />
</div>

<br />

## An honest word on returns

A liquidity vault is not a savings account. The ranges hold real ETH, so when ETH moves, what you hold
moves with it, and that term is routinely larger than the fees. We measured it: a 10.8% move in ETH
shifts the ETH-denominated side of the book by roughly 8%, which is about thirty times a day of fee
income.

We publish gas spent beside fees earned, and realised beside unrealised, precisely so that the picture
is not flattering by omission. If the strategy is behind, the dashboard says so that day.

<br />

<div align="center">

### [pippinfor.me](https://pippinfor.me) · [dashboard](https://pippinfor.me/dashboard) · [guardrails](https://pippinfor.me/trust) · [learn](https://pippinfor.me/learn)

<sub>Built on Robinhood Chain · chain id 4663 · contracts MIT licensed</sub>

</div>