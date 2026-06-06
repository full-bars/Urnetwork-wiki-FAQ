# Country multipliers

### What are Country Multipliers?
To ensure the network is available everywhere, the protocol offers increased rewards for providers in "underserved" regions. These multipliers apply to both **USDC earnings** and **Reliability Points**.

### How are they calculated?
Unlike most VPNs with static rates, URnetwork uses a **Dynamic Multiplier** system. 
* **Technical Logic:** The protocol compares the `netReliabilityWeight` of a country against a global target. 
* **Supply & Demand:** If a country has very few providers but high potential demand, the multiplier automatically increases (up to the `MaxCountryReliabilityMultiplier` defined in the server config).
* **Saturation:** As more providers join a high-multiplier region, the multiplier will slowly decrease to balance the network.

### How to check multipliers?
Multipliers are updated periodically by the backend. 
1. **Dashboard:** High-multiplier regions are often highlighted or listed in community updates.
2. **Discord:** Check the `#announcements` channel for the latest "Boosted Regions."

### Payout Impact
If you are in a country with a **2.0x multiplier**:
* You earn **2x more Points** for every hour of uptime.
* Your share of the weekly **USDC pool** is effectively doubled compared to a provider with the same uptime in a 1.0x region.

### Stability Requirement
Multipliers only matter if your node is stable. A 10x multiplier on a node with 50% uptime will still earn less than a 1x region node with 99.9% uptime.

---
*Technical Note: Multipliers are calculated in the `server/model/network_client_reliability_model.go` file using an algorithmic encoding of network health.*
