# Points & airdrop

### What are \"Points\"?
Points are a parallel reward system to USDC. While USDC is paid out weekly, Points accumulate to determine your share of the future **$UR token airdrop** on Solana.

### Earning Points
There are three primary ways to accumulate points:

| Point Type | Description | Multipliers |
| :--- | :--- | :--- |
| **Traffic Points** | Earned from moving billable data | Seeker (2x) |
| **Reliability Points** | Earned from uptime per IP address | Country Multiplier, Stability Multiplier (>99.9%) |
| **Referral Points** | Earned when people you refer move traffic | Referral percentages |

### $UR Token Airdrop
* **Conversion:** Accumulated points will be redeemable for the $UR token.
* **Network Ownership:** The $UR token represents ownership and governance in the URnetwork protocol.

### Multipliers
* **Seeker Holder Multiplier:** Confirmed in the codebase (`subsidyConfig.SeekerHolderMultiplier`) as a **2x boost** to points earned from traffic and payouts.
* **Stability Bonus:** Achieving **>99.9% reliability** for a measurement block triggers an additional internal multiplier to reward "infrastructure-grade" providers.

### Reliability Points Calculation
* **Per IP:** Calculated per unique IP address. Each IP can earn a maximum of 1 full reliability point per payout period.
* **IP Redundancy:** Running multiple nodes on the same IP improves your chance of achieving 100% uptime (redundancy) but **splits** the single reliability point among the active nodes. It does not generate "extra" points for the same IP.

### Referral Points
* **Network Effect:** When you refer a user who starts providing bandwidth, you earn a percentage of their points. 
* **Referral Parent/Child:** Bonuses are applied recursively up the chain (parent/grandparent) as defined in `account_point_model.go`.

> **Staff Reminder:** Points are your ticket to the token airdrop. Focus on stability and low-latency connections to maximize your accumulation.
