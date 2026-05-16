# Traffic contracts

### What is a traffic contract?
The core of URnetwork's economy is the **Contract**. When a client needs to route data, the network auctions the request among available providers. The winning node gets a "contract" to carry that specific traffic and earns rewards.
* **Selection Criteria:** Your IP's reputation, reliability score (>99.9% is ideal), latency, and country multiplier all influence whether you win a contract.
* **Earnings:** You are paid for **successful contracts**, not merely for having your computer on or for moving non-billable system data.

### Troubleshooting "No Traffic"
If your node is connected but not moving traffic:
1. **Oversupply:** There may be more providers than client demand in your region (common in the USA).
2. **IP Quality:** Your IP may be flagged as a VPN or have a poor reputation score.
3. **Network Bugs:** Periodic backend issues can affect contract assignment. When these are fixed, the team often issues retroactive credits.
4. **Demand:** Some regions (like Vietnam) have many providers but very little client demand, leading to low win rates.

### Error: "exit could not create contract"
This log message indicates a matchmaking or infrastructure issue on the URnetwork side.
* **Fix:** There is nothing to fix locally. Wait for the developers to resolve the backend issue. Your node will still earn reliability points for being online during this time.

### System Data vs. Billable Data
Your OS network monitor will always show more data usage than the URnetwork dashboard.
* **Non-Billable Traffic:** The provider binary communicates with the URnetwork API (e.g., `api.bringyour.com`) for signaling, status updates, and announcement. This data keeps your node connected but is not paid.
* **Contract Cap:** There is currently a known discussion regarding a hardcoded ceiling (approximately 13KB) for initial contract transfers. This means some contracts may "fill up" very quickly. The team is continuously optimizing this as the protocol matures.

> **Staff Tip:** As long as you have more "success" than "error" messages in your logs (or high uptime on the graph), your node is performing correctly. Lower contract win rates are typically market-driven.