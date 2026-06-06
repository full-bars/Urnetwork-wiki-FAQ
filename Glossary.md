# Glossary of Terms

To help you navigate the URnetwork technical documentation and community discussions, here is a breakdown of the core terminology used in the protocol.

### Network Roles
* **Provider (Node):** An individual or entity that contributes internet bandwidth to the network. Providers run the `urnetwork provide` binary and earn rewards (USDC/Points).
* **Client (User):** A person using the URnetwork VPN to route their traffic. The client software runs an internal "auction" to choose the best providers.
* **Network Operator (NO):** The central infrastructure (URnetwork team) that handles signaling, matchmaking, and reward distribution.
* **Exchange:** A high-performance signaling server that coordinates connections between clients and providers. 
* **Resident:** A persistent virtual identity for a client or provider within an Exchange. Residents ensure that even if a physical connection drops, the network session remains stable.

### Economic Terms
* **Traffic Contract:** A billable agreement between a client and a provider to move a specific amount of data. Rewards are calculated per successful contract.
* **Payer:** The entity responsible for funding a contract (usually the client's account).
* **Balance Code:** A redeemable voucher that adds data quota to a user's account.
* **Voucher:** A daily bandwidth allocation (34 GiB for Free, 600 GiB for Supporter) with a 30-hour lifespan.

### Technical Terms
* **Multi-IP (Optimistic Auction):** The architecture where a client maintains 12+ provider connections simultaneously to ensure the fastest possible route.
* **Multi-Hop:** Routing traffic through one or more "Relay" nodes before reaching an "Exit" node. Currently used for signaling; P2P data multi-hop is on the roadmap.
* **Extender:** A protocol (`UREXTENDER1`) used to bounce traffic through intermediary nodes to bypass restrictive firewalls (SNI spoofing).
* **DoH (DNS-over-HTTPS):** A method for performing DNS lookups over an encrypted HTTPS connection to prevent ISP-level tracking and poisoning.
* **PQE (Post-Quantum Encryption):** Advanced hybrid encryption (e.g., ML-KEM-768) designed to be secure against future quantum computer attacks.
* **Tether:** A feature that allows standard WireGuard apps to connect to the URnetwork decentralized marketplace.

---
*Technical Note: Most of these terms map directly to structs and interfaces in the `connect/` source directory.*
