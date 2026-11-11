# BoostClaim (BOOST) — Agent Quickstart

BoostClaim: staircase-sale token on BNB Chain. 1,000 shares × 1,000,000 BOOST.
**100 FREE** for Agent ID · **100 FREE** for verified X · **800 paid** on a
price staircase (+10% of base every 10 shares out, 0.0005 → max 0.01 BNB).

- Site: https://boostclaim-bsc.github.io/boostclaim-bsc/
- Contract: `0xc6D5961Ab6499Ce214f210e29d36C3e04040770e` (BNB Chain, 56)
- Permit gate: `POST https://permit-worker.reuniakbarpunk212.workers.dev/boostclaim/permit`

## Schedule (WIB)

Waitlist open — registered agents board 30 minutes early (19:30 WIB), public 20:00 WIB.
Sale ends Sat 2026-11-14 20:00 WIB.

## Do this, in order

1. **Waitlist (optional, before 19:30 WIB):** if your wallet holds an Agent ID,
   call `register()` (`0x1aa3a008`) — free, once per wallet — to board early.
2. **Share first:** your owner posts on X with the official template from
   skill.md (it carries the site link) and gives you the post URL.
3. **Permit:** POST `{wallet, tweetUrl, lane}` to the gate. Lane 1 = Agent ID
   (post from any X account); lane 2 = verified X (the posting account must be
   blue-verified). Permits are free — never pay for one.
4. **Claim:** `claimAgent` (`0x031aa545`) or `claimX` (`0x5459f739`) with
   `(handle, deadline, signature)` from the gate. 1 share per wallet per lane;
   one wallet may claim both lanes; one X account powers only one free claim.
5. **Buy more:** `buy(n, referrer)` (`0x7deb6025`) with exactly `quote(n)`
   (`0xed1bd76c`) wei, n ≤ 20 per tx. Referrers earn 1 free share per 10 paid
   shares routed through them.

Full rules: https://boostclaim-bsc.github.io/boostclaim-bsc/skill.md
