# Protocol Overview

URnetwork is more than just a VPN; it is a **Decentralized Connection Marketplace**. This page explains the underlying protocol layers that make the network function.

### 1. The Connection Lifecycle
When a client wants to access a website (e.g., `google.com`), the following sequence occurs:
1. **DNS Resolution:** The client performs an encrypted lookup via **DoH** (DNS-over-HTTPS). Default resolver is Cloudflare (`1.1.1.1`). Source: `connect/net_http_doh.go`
2. **Provider Auction:** The client evaluates its "window" of ~12 active provider nodes.
3. **Optimistic Handshake:** The client sends initial data packets to multiple providers. The node that returns the first ACK (acknowledgment) "wins" the contract.
4. **Contract Creation:** A `TransferContract` is established. Initial allocation is **16 KiB** (`InitialContractTransferByteCount`), scaling up to **128 MiB** (`StandardContractTransferByteCount`) over 4 contracts. Source: `connect/transfer_contract_manager.go`
5. **Egress:** The provider node decrypts the traffic and forwards it to the open internet.

### 2. Transport Protocol
URnetwork uses a layered transport system with multiple modes. The transport version is currently **v2** (added latency and speed test support).

**Transport Modes** (in order of preference):
| Mode | Type | Description |
| :--- | :--- | :--- |
| `h3dnspump` | HTTP/3 + DNS | Highest preference. Uses QUIC with DNS-based discovery and "pump" pre-connection. |
| `h3dns` | HTTP/3 + DNS | QUIC-based with DNS discovery. |
| `h3` | HTTP/3 | Standard QUIC transport. |
| `h1` | HTTP/1.1 | Fallback TCP transport. |

When set to `TransportModeAuto`, the client starts all modes in parallel and selects the best performer.

*Source: `connect/transport.go`*

### 3. P2P Transport (WebRTC)
To ensure the network works behind strict residential NATs and mobile carrier firewalls, URnetwork utilizes **WebRTC** for its primary peer-to-peer transport layer.

**Default WebRTC Settings:**
- `ReceiveBufferSize`: 4 MiB
- `ReceiveMtu`: 4 KiB
- `DisconnectedTimeout`: 30 seconds
- `FailedTimeout`: 30 seconds
- `KeepAliveTimeout`: 1 second

**ICE Servers (STUN):**
- `stun:openrelay.metered.ca:80`
- `stun:openrelay.metered.ca:443`
- `stun:stun.stunprotocol.org:3478`
- `stun:stun.l.google.com:19302` (and `stun1` through `stun4`)

If a direct P2P link cannot be established, the protocol falls back to relaying through an **Exchange** node.

*Source: `connect/transport_p2p_webrtc.go`*

### 4. Symmetric Safety Model
The protocol protects providers using an algorithmic firewall (`connect/ip_security.go`).

**Default Blocked Ports:**
| Port(s) | Reason |
| :--- | :--- |
| 6881-6889 | BitTorrent |
| 6969 | BitTorrent tracker |
| 1337, 9337, 2710 | Unofficial BitTorrent |
| < 1024 (unlisted) | System/privileged ports |
| >= 11000 | RTP/P2P ranges |

**Allowed by default:**
- Port 80 (TCP) - HTTP (temporary, for radio streaming)
- Port 443 (TCP/UDP) - HTTPS/QUIC
- Port 53 (UDP) - DNS (temporary, FIXME to upgrade to DoH)
- Ports 993, 995, 465 - Email (IMAP, POP, SMTP)
- Port 853 - DNS over TLS
- Ports 123, 500 - NTP, IPsec (Apple system services)

*Source: `connect/ip_security.go` (lines 83-162)*

### 5. Encryption Layers
URnetwork employs multiple security layers:

**Transport Layer:** Standard TLS 1.3 for all transport connections. The extender protocol uses TLS 1.3 with server name verification.

**Per-Peer Sequence Encryption (E2EE):** Each (client, provider) pair establishes an ephemeral TLS 1.3 session (`transfer_encrypt.go`). Both sides present self-signed certificates, then exchange post-handshake identity proofs signed with long-lived Ed25519 client keys. The derived AES-GCM cipher is only exposed after mutual identity verification.

**Post-Quantum Protection:** The protocol prefers the hybrid **X25519MLKEM768** key exchange (FIPS 203 / RFC 9794), falling back to X25519. This protects against "harvest now, decrypt later" quantum attacks.

**Encryption is opt-in:** `Encrypt` must be set to `true` in `EncryptionSettings`. Without it, traffic flows in plaintext between peers.

### 6. Tether (WireGuard Compatibility)
URnetwork supports standard WireGuard clients via the **Tether** feature. Tether creates a userspace WireGuard interface that routes traffic through the URnetwork marketplace rather than a direct peer.

**Tether Architecture:**
- Uses a modified `wireguard-go` userspace implementation
- Includes a local NAT to map tunnel IPs to public IPs
- Exposes an HTTP API for peer management (`add`, `remove`, `get-config`)
- Configuration files use standard WireGuard INI format with extensions (`Address`, `PreUp`, `PostUp`, `SaveConfig`)

*Source: `connect/tetherctl/` and [Tether changelog](https://docs.ur.io/changelog/2024-10-31-tether/tether)*

---

*Technical Reference:*
- `connect/transfer.go`: Core data transfer and contract logic.
- `connect/transport_p2p_webrtc.go`: WebRTC implementation details.
- `connect/ip_security.go`: Automated firewall and safety policies.
- `connect/net_extender.go`: Anti-censorship extender protocol.
- `connect/connect.go`: Protocol version and multi-hop constants.
