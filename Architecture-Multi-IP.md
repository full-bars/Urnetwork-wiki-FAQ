# Architecture: The Multi-IP Strategy

URnetwork uses a unique architecture called the **Optimistic Auction Model** to ensure privacy and performance.

### Technical Q&A

#### Q: Why does the client maintain 12+ simultaneous IP addresses?
Unlike traditional VPNs that tunnel everything through one server, URnetwork connects you to a "window" of active providers (typically 12 or more).
* **Privacy:** Your traffic is distributed. No single provider sees your full profile.
* **Reliability:** If one provider drops, only ~1/12th of your active flows are affected.
* **Speed:** The client runs an internal "auction" for every new connection to choose the fastest node.

#### Q: What is the "Optimistic Handshake"?
Instead of waiting for a slow serial handshake, URnetwork often sends initial packets to multiple promising nodes in parallel. Whichever node returns the first acknowledgment (ACK) "wins" the flow. This ensures zero-latency route selection.

#### Q: Does URnetwork support Multi-Hop (Onion/Garlic) routing?
The **protocol** fully supports it (`MaxMultihopLength = 8`), but the **implementation** is currently in a semi-decentralized state:
* **Relay Status:** Currently, only the **central server exchange** implements the `forward()` callback.
* **Roadmap:** Future updates aim to enable community-run nodes to act as relays, allowing for true P2P multi-hop data routing.

---
*Technical Insight: This architecture is why URnetwork is often described as a "Marketplace" rather than a simple VPN service.*
