# Economic Model

URnetwork's economic model is designed to reward providers while building a sustainable business. This page explains how revenue flows through the network and how providers get paid.

### Revenue Sources

URnetwork earns revenue from **user premium subscriptions** ($5/month for Supporter tier). This revenue funds the community payout pool and network operations.

Unlike traditional VPNs that must spend heavily on bandwidth and servers, URnetwork's providers run the egress nodes themselves. This means data center costs are almost fixed, allowing the network to offer free access with a data cap.

### Community Payouts

Payments to providers come from a **subsidy payout** calculated as:

- **10% of premium subscription revenue**, with a guaranteed minimum of $0.10 per MAU (Monthly Active User)
- MAU counts human users only (automation and bots are excluded)

### Payout Calculation

Per-device payout is determined by data routed, weighted:
- **75%** against all global devices
- **25%** against devices in the same country (all countries weighted equally)

This dual weighting ensures providers in underserved regions earn more, while still rewarding global traffic volume.

### Referral Bonuses

The referral system has a recursive structure:
- **50%** of the referred user's earnings
- **50%** of their referral bonuses (passed up the chain)

Combined with the direct subsidy, up to **20% of premium revenue** flows back to the community, with a minimum of $0.20 per MAU.

### Payout Sweep

- **Frequency:** Weekly (Sunday 00:00 UTC)
- **Currency:** USDC on Solana (wallet set in the app)
- **Gas Fees:** Subtracted from the payout. The system holds and combines payouts until the value minus gas is positive.
- **Contract Data Retention:** Settled contracts used for payouts are deleted one week after payout, in line with the network's privacy-focused data deletion policy.

### Paid vs. Subsidized Transfer

The model has two historical phases:
- **Paid Transfer (legacy):** The original model from public beta (H1 2024). 50% revenue share. This trends toward zero as the network transitions.
- **Subsidized Transfer (current):** The new model (from H2 2024 launch). Based on the 10% premium revenue formula above.

### Company Phases

The economic model defines four phases:
1. **Phase 0 (Repeatable Network):** Build a network better than consumer VPNs with almost fixed operating costs.
2. **Phase 1 (Repeatable Premium):** Premium revenue is not yet enough to fully subsidize community payments. Break-even is at ~4% premium conversion at $5/month.
3. **Phase 2 (Scale):** Revenue scales with MAU while operating costs stay nearly fixed.
4. **Phase 3 (Profitable):** The company becomes profitable at scale, targeting 70-80% margins.

### Privacy

Payment on the network is private. Each transfer between a user and provider is preceded by a **contract** that combines payment info, access control, and priority. Both parties close the contract; if they agree, it is settled and added to the payout.

---

*Source:* [Official Economic Model](https://docs.ur.io/economic-model/economic-model)
