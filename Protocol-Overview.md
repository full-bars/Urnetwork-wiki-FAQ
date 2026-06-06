# Protocol Overview

URnetwork is more than just a VPN; it is a **Decentralized Connection Marketplace**. This page explains the underlying protocol layers that make the network function.

### 1. The Connection Lifecycle
When a client wants to access a website (e.g., `google.com`), the following sequence occurs:
1. **DNS Resolution:** The client performs an encrypted lookup via **DoH** (DNS-over-HTTPS) to find the target IP.
2. **Provider Auction:** The client evaluates its "window" of ~12 active provider nodes.
3. **Optimistic Handshake:** The client sends initial data packets to multiple providers. The node that returns the first ACK (acknowledgment) "wins" the contract.
4. **Contract Creation:** A `TransferContract` is established, and rewards begin accumulating for the winning provider.
5. **Egress:** The provider node decrypts the traffic and forwards it to the open internet.

### 2. P2P Transport (WebRTC)
To ensure the network works behind strict residential NATs and mobile carrier firewalls, URnetwork utilizes **WebRTC** for its primary transport layer.
* **STUN/TURN:** The protocol uses standard signaling to negotiate direct peer-to-peer links without requiring users to configure port forwarding.
* **Reliability:** If a direct P2P link cannot be established, the protocol falls back to relaying through an **Exchange** node.

### 3. Symmetric Safety Model
The protocol protects providers using an algorithmic firewall (`ip_security.go`).
* **Default Policies:** The software automatically blocks high-risk traffic (e.g., SMTP for spam, specific P2P ports for illegal file sharing).
* **No DPI:** The network does not inspect your data. Safety is maintained by restricting **destination ports** and **traffic patterns** rather than reading content.

### 4. Encryption Layers
URnetwork employs multiple layers of security to ensure data integrity:
* **N-TLS:** A specialized TLS implementation optimized for high-performance networking.
* **Hybrid PQE:** Recently introduced **Post-Quantum Encryption** ensures that data captured today cannot be decrypted by future quantum computers.
* **Client-Provider E2EE:** When `Encrypt=true` is set, only the client and the chosen provider have the keys to the traffic; the central Network Operator cannot see the data.

---
*Technical Reference:*
* `connect/transfer.go`: Core data transfer and contract logic.
* `connect/transport_p2p_webrtc.go`: WebRTC implementation details.
* `connect/ip_security.go`: Automated firewall and safety policies.
