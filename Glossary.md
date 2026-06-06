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
* **Contract Transfer Byte Count:** The data allocation per contract. Initial contracts start at 16 KiB, scaling up to 128 MiB over 4 contracts. Source: `transfer_contract_manager.go`
* **Payer:** The entity responsible for funding a contract (usually the client's account).
* **Balance Code:** A redeemable voucher that adds data quota to a user's account.
* **Voucher:** A daily bandwidth allocation (34 GiB for Free, 600 GiB for Supporter) with a 30-hour lifespan.
* **Subsidy Payout:** The provider reward pool, funded by 10% of premium subscription revenue with a minimum of $0.10 per MAU.

### Technical Terms
* **Multi-IP (Optimistic Auction):** The architecture where a client maintains 12+ provider connections simultaneously to ensure the fastest possible route.
* **Multi-Hop:** Routing traffic through one or more "Relay" nodes before reaching an "Exit" node. `MaxMultihopLength` is 8 (supports up to 8 hops). Currently used for signaling; P2P data multi-hop is on the roadmap.
* **Extender:** A protocol (`UREXTENDER1`) used to bounce traffic through intermediary nodes to bypass restrictive firewalls (SNI spoofing). Uses TLS 1.3 over TCP, mimicking standard HTTPS traffic.
* **Extender Profile:** Configuration for an extender connection, specifying `ConnectMode`, `ServerName`, `Port`, and optional `Fragment`/`Reorder` flags to evade DPI. Source: `net_extender.go`
* **DoH (DNS-over-HTTPS):** A method for performing DNS lookups over an encrypted HTTPS connection to prevent ISP-level tracking and poisoning.
* **PQE (Post-Quantum Encryption):** Hybrid encryption using X25519 + ML-KEM-768 (FIPS 203 / RFC 9794) designed to be secure against future quantum computer attacks.
* **Tether:** A feature that allows standard WireGuard apps to connect to the URnetwork decentralized marketplace via a userspace WireGuard interface.
* **ICE (Interactive Connectivity Establishment):** A framework used by WebRTC to find the best path between two peers, using STUN servers to discover public IPs.
* **STUN (Session Traversal Utilities for NAT):** A protocol used to discover your public IP address and port mapping behind a NAT.

### Log Message Tag Terms
These tag prefixes appear in provider logs. See [[Log Reference]] for a full guide.

* **`[net][s]select`** — Serial connection selection. Logged when the provider selects a route for a client. Each line shows the proxy/destination, strategy mode (`normal`, `fragment`, `reorder`), success/error counts, and active client count.
* **`[t]auth error`** — Transport authentication failure. The provider could not authenticate a connection to the platform. Rate-limited to 1 per minute.
* **`[s]`** — Send sequence lifecycle. Includes contract creation, session exits, and ack tracking.
* **`[r]`** — Receive sequence lifecycle. Includes packet drops and session exits.
* **`[contract]`** — Contract management events.
* **`[tls]`** — Per-peer TLS encryption session events.

### Transport Protocol Terms
* **Transport Mode:** The protocol layer used for data transfer. Modes include `h3dnspump`, `h3dns`, `h3` (QUIC/HTTP-3), and `h1` (HTTP/1.1 TCP). Source: `transport.go`
* **WebRTC:** The primary P2P transport used for NAT traversal. Configured with 4 MiB receive buffers, 4 KiB MTU, and 30-second timeouts. Source: `transport_p2p_webrtc.go`

### Protocol Version Terms
* **ProtocolVersion 2:** The current protocol version (since 2025-05-28), optimized for memory usage. v1 was the original.
* **TransportVersion 2:** The current transport version, adding latency and speed test support over v1.

---
*Technical Note: Most of these terms map directly to structs and interfaces in the `connect/` source directory.*
