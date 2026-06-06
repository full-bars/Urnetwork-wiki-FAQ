# Architecture: The Multi-IP Strategy

URnetwork uses a unique architecture called the **Optimistic Auction Model** to ensure privacy and performance. Unlike traditional VPNs that connect you to one server, URnetwork connects you to many simultaneously.

### 1. Why 12+ IP Addresses?
When you start the URnetwork client, it attempts to maintain a \"window\" of active provider nodes (typically 12 or more).
* **Privacy:** Your traffic is distributed across multiple independent providers. No single provider sees your full traffic profile.
* **Reliability:** If one provider goes offline, only ~1/12th of your active flows are affected. The client instantly reroutes that traffic to another node in the window.
* **Performance:** For each new connection, the client runs an internal \"auction\" to see which of the 12 nodes can reach the destination with the lowest latency.

### 2. The Optimistic Auction
Instead of waiting for a slow \"handshake\" before sending data, URnetwork often sends initial packets to multiple promising nodes in parallel.
* **The Winner:** Whichever node returns the first acknowledgment (ACK) \"wins\" the flow.
* **Impact:** This ensures you always get the fastest possible route without the user ever noticing the selection process.

### 3. Standards & Protocols
URnetwork is built to be resilient across all network conditions:
* **WebRTC:** Used for high-performance peer-to-peer (P2P) connections.
* **STUN/TURN:** Fallback mechanisms to ensure the VPN works even behind strict firewalls or mobile carrier NATs.
* **Legacy Support:** The protocol is intentionally compatible with older standards to ensure availability in regions with restricted internet infrastructure.

---
*Technical Insight: This architecture is why URnetwork is often described as a \"Marketplace\" rather than a simple VPN service.*
