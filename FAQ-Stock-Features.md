# URnetwork FAQ

Welcome to the URnetwork FAQ. Here, we address common questions about how the network operates under the hood, how it compares to traditional VPNs, and how it protects both users and providers.

## 1. What makes URnetwork different from a traditional VPN?

Traditional VPNs tunnel all your internet traffic through a single server. This creates a single point of failure and makes it easy for services to block you.

URnetwork utilizes a **Connection Marketplace** via an optimistic multi-IP auction model. For every single connection you make (like loading different images on a webpage), the client evaluates a "window of providers" and strictly uses the one that performs best. This allows for high availability, naturally circumvents blocked IPs, and means your traffic is distributed rather than centralized. 

## 2. How does URnetwork bypass restrictive firewalls?

Users in heavily restricted networks (like corporate firewalls or countries with strict censorship) often face issues with standard VPN protocols getting blocked.

URnetwork addresses this using **Extenders** (the `UREXTENDER1` protocol). The network employs N-TLS encryption combined with **SNI (Server Name Indication) spoofing** to disguise your traffic. It bounces traffic through intermediary IP extenders, masking the true destination and avoiding automated firewall detection.

The extender protocol uses standard TLS 1.3 over TCP, blending in with normal HTTPS traffic on common ports like 443 (HTTPS), 853 (DNS over TLS), 993 (IMAP), and 995 (POP). The `EnumerateExtenderProfiles()` function randomly selects from service/mail personas, optionally applying packet fragmentation and reordering to evade deep packet inspection.

*Reference: [`net_extender.go`](https://github.com/urnetwork/connect/blob/main/net_extender.go)*

## 3. Can I use my standard WireGuard app with URnetwork?

Yes! While URnetwork has its own native clients, you can connect using any standard WireGuard application (on iOS, Android, macOS, Windows, or Linux) using the **Tether** feature.

Tether creates a bridge on your local network: it acts as a standard WireGuard server that your app can connect to, but under the hood, it routes all that traffic through the decentralized URnetwork marketplace.

Tether runs fully in userspace (no kernel module needed) and includes:
- A CLI for managing devices and peers
- An HTTP API for peer configuration
- A `default-builder` workflow for single-command setup

*Reference: [`connect/tetherctl`](https://github.com/urnetwork/connect/tree/main/tetherctl)*

## 4. Do I need to configure Port Forwarding to be a Provider?

No. A common pain point for running a node on other networks is dealing with complicated router settings.

While having direct port forwarding (TCP/UDP) can improve performance, URnetwork natively utilizes **WebRTC** to negotiate peer-to-peer connections. This allows provider nodes to automatically traverse NATs and restrictive router setups without any manual configuration.

The protocol uses ICE (Interactive Connectivity Establishment) with multiple STUN servers to discover public IPs and negotiate direct connections. If a direct P2P link cannot be established, traffic falls back through an Exchange relay node.

*Reference: [`transport_p2p_webrtc.go`](https://github.com/urnetwork/connect/blob/main/transport_p2p_webrtc.go)*

## 5. Does URnetwork prevent DNS leaks?

Yes. URnetwork guarantees that your internet service provider (ISP) or local network administrator cannot see which websites you are visiting.

The client strictly enforces **DNS-over-HTTPS (DoH)** for all DNS lookups, with Cloudflare (`https://1.1.1.1/dns-query`) as the default resolver. Local DNS is also enabled as a fallback. The resolution priority is: remote DoH -> remote DNS -> local DNS. This secures the connection and resolves domains privately before the first data packet is ever routed.

*Reference: [`net_http_doh.go`](https://github.com/urnetwork/connect/blob/main/net_http_doh.go)*

## 6. Does URnetwork analyze my traffic?

We care deeply about privacy, which is why URnetwork avoids Deep Packet Inspection (DPI) entirely. 

However, users deserve transparency about which third parties are tracking them. We provide this via the **Inspect** feature. Inspect performs **100% on-device metadata clustering**. By analyzing only the timing and packet headers (without ever reading the payload), the app can group applications and identify third-party trackers locally. This data never leaves your device.

The clustering algorithm was developed with research into HDBSCAN and OPTICS, using either fixed-margin or Gaussian overlap functions to compare session timing patterns.

*Reference: [`connect/inspect`](https://github.com/urnetwork/connect/tree/main/inspect)*

## 7. Is it safe to be a Provider?

A common fear when running a public node (like a Tor exit node) is being held liable for abuse or malicious traffic.

URnetwork is built with a **symmetric safety model**. The protocol incorporates automated security intelligence directly into the client. By default, it restricts risky behavior (like specific file-sharing ports and unencrypted standards) via `DefaultEgressSecurityPolicy()`. This establishes a safe baseline, protecting the provider's hardware and legal standing without requiring manual intervention.

The security policy:
- Blocks BitTorrent ports (6881-6889, 6969, 1337, 9337, 2710)
- Blocks all system ports < 1024 (except explicitly allowed ones)
- Blocks RTP/P2P ranges (>= 11000)
- Allows HTTPS, HTTP, DNS, email, and Apple system services
- Checks against a pre-compiled IPv4 blocklist (~2.3 MB)

*Reference: [`ip_security.go`](https://github.com/urnetwork/connect/blob/main/ip_security.go)*

## 8. Is URnetwork protected against MITM attacks and Post-Quantum threats?

Yes. URnetwork implements **per-peer TLS encryption** between sequences (`transfer_encrypt.go`). Each (client, provider) pair establishes an ephemeral TLS 1.3 session with mutual TLS (mTLS). Client and provider verify each other's public keys via a post-handshake identity proof exchange, making it computationally impossible for even the Network Operator to MitM the connection.

The protocol includes **Post-Quantum Encryption (PQE)** via hybrid key exchange using **X25519MLKEM768** (FIPS 203 / RFC 9794) as the preferred curve, with **X25519** as fallback. This protects against "harvest now, decrypt later" attacks where encrypted data is captured today for future quantum decryption.

**How it works:**
1. Per-peer TLS sessions are created lazily when sequences need encryption
2. Both sides present ephemeral self-signed certificates
3. After the TLS handshake, both peers sign a shared TLS exporter value with their long-lived Ed25519 client keys
4. The signed proofs are exchanged as `EncryptedControl` messages
5. The derived AES-GCM cipher is only exposed after identity verification succeeds
6. Traffic flows in plaintext if the cipher is not available (graceful fallback)

When `Encrypt=true` is set in `EncryptionSettings`, the session is established automatically. Without it, encryption is disabled and traffic flows unprotected.

*Reference: `connect/transfer_encrypt.go`*

## 9. What transport protocols does URnetwork use?

URnetwork uses a multi-layered transport system with several modes, ranked by preference:

| Mode | Transport | Discovery |
| :--- | :--- | :--- |
| `h3dnspump` | QUIC (HTTP/3) | DNS + pre-connection "pump" |
| `h3dns` | QUIC (HTTP/3) | DNS |
| `h3` | QUIC (HTTP/3) | Direct |
| `h1` | HTTP/1.1 (TCP) | Direct |

When in `auto` mode (the default), the client starts all modes in parallel and selects the best performer based on measured latency and throughput.

## 10. Where can I get more support?

For additional help, technical troubleshooting, or simply to connect with other providers and users, join our community on Discord:

[Join the URnetwork Discord](https://discord.gg/urnetwork)
