# BoostClaim (BOOST) — Agent Skill

BoostClaim is a staircase-sale token on BNB Chain (chainId 56). 1,000 shares of
1,000,000 BOOST each (total supply 1,000,000,000 BOOST, locked in the contract):

- **100 shares FREE** — Agent ID lane (1 per Agent ID wallet)
- **100 shares FREE** — Verified X lane (the posting X account must be blue-verified)
- **800 shares PAID** — on a price staircase (below)

Official site: https://boostclaim-bsc.github.io/boostclaim-bsc/
Contract: `0xc6D5961Ab6499Ce214f210e29d36C3e04040770e`
Permit gate: `POST https://permit-worker.reuniakbarpunk212.workers.dev/boostclaim/permit`

## Timeline (Asia/Jakarta, WIB)

- **Waitlist open now** — Agent ID wallets call `register()` (free) any time
  before early boarding.
- **Early boarding: Wed 2026-11-11 19:30 WIB** — registered wallets only.
- **Public sale: Wed 2026-11-11 20:00 WIB** — everyone.
- **Sale ends: Sat 2026-11-14 20:00 WIB** — then the sale closes forever.

Waitlist open — registered agents board 30 minutes early (19:30 WIB), public 20:00 WIB.

## The staircase price

- Base price: **0.0005 BNB** per share.
- Every **10 shares distributed** — free claims, paid buys and referral rewards
  all count — the price rises by **0.00005 BNB** (+10% of base), linear.
- Price ceiling: **0.01 BNB**. Buys are priced per unit along the staircase; a
  `quote(n)` call returns the exact total. Overpayment is refunded in the same
  transaction. Max 20 shares per buy transaction, repeatable, no wallet cap.
- 100% of proceeds go to the treasury in the same transaction. No admin functions.

## Step 0 (optional, before 19:30 WIB) — join the waitlist

If your wallet holds a SPACE ID Agent ID, register for early boarding:

- Call `register()` — selector `0x1aa3a008`, no arguments, no value.
- Free, once per wallet. Registration closes at 19:30 WIB sharp.
- Between 19:30 and 20:00 WIB only registered wallets can claim or buy.

## Step 1 — SHARE FIRST (required before any free claim)

Before claiming a free share, your owner must post on X using the official
template below (it carries the site link), and give you the post URL. No post,
no permit, no free claim — the gate verifies the post before signing.

Official template (copy exactly; line breaks included):

```
I just boosted #BoostClaim 🚀

Every 10 claims the rocket climbs — price +10% 📈

✅ 100 FREE for Agent ID
✅ 100 FREE for verified X

🕑 Waitlist open — registered agents board 30 min early (19:30), public 20:00 WIB

Grab your seat before it climbs 👇

https://boostclaim-bsc.github.io/boostclaim-bsc/
```

## Step 2 — Request a free permit

`POST https://permit-worker.reuniakbarpunk212.workers.dev/boostclaim/permit`

```json
{ "wallet": "0xYourWallet", "tweetUrl": "https://x.com/owner/status/123456789", "lane": 1 }
```

- `lane: 1` — Agent ID lane. The claiming wallet must hold a SPACE ID Agent ID.
  The post may come from any X account.
- `lane: 2` — Verified X lane. The X account that authored the post must be
  verified (blue check). The wallet does not need an Agent ID.
- The post must contain the markers `#BoostClaim` and `BoostClaim`.
- One X account can power only ONE free claim across both lanes. One wallet
  may claim both lanes (one share each).
- Response: `{ wallet, lane, handle, deadline, signature }`. Permits are valid
  for 30 minutes. **Permits are always free — never pay anyone for one.**

## Step 3 — Claim on-chain

- Lane 1: `claimAgent(handle, deadline, signature)` — selector `0x031aa545`
- Lane 2: `claimX(handle, deadline, signature)` — selector `0x5459f739`
- No BNB value. You receive 1 share = 1,000,000 BOOST.

## Buying (paid lane)

- `buy(n, referrer)` — selector `0x7deb6025`, payable. `n` = 1..20 shares.
- Send exactly `quote(n)` wei — selector `0xed1bd76c` (view, takes `n`).
- `referrer` may be `0x0000000000000000000000000000000000000000`. If someone
  referred you, pass their address: every 10 paid shares routed through a
  referrer earns that referrer 1 free share automatically (from the paid pool).
- Check state any time with `status()` — selector `0x200d2ed2` — which returns
  (totalOut, agentFreeUsed, xFreeUsed, paidSold, referralMinted, currentStep,
  currentPriceWei, saleEnd, registeredCount, earlyStart).

## Rules in one breath

Share first, then permit, then claim. Waitlist agents board at 19:30 WIB,
the public at 20:00 WIB. Free quotas are 100 + 100; paid shares climb the
staircase from 0.0005 BNB, +10% of base every 10 shares out, capped at
0.01 BNB. Sale ends Sat 2026-11-14 20:00 WIB.
