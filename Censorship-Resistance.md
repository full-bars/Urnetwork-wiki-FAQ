# Censorship Resistance

URnetwork is engineered to function in heavily restricted environments (e.g., the Great Firewall). This page explains the **live features** currently protecting the network and summarizes **community-proposed workarounds**.

### Current Network Features

#### Q: How does URnetwork prevent DNS hijacking or poisoning?
The client strictly enforces **DNS-over-HTTPS (DoH)** for all lookups. By default, it resolves via Cloudflare (`1.1.1.1`) or Google (`8.8.8.8`) over an encrypted tunnel. Because the lookups are encrypted, local ISP firewalls cannot see or alter the results.
* **Source:** `connect/net_http_doh.go`

#### Q: What are "Extenders" and how do they work?
For environments where the URnetwork protocol is identified and blocked, the system uses the `UREXTENDER1` protocol. It "bounces" traffic through an intermediary node using **SNI Spoofing**. This makes the VPN traffic look like harmless web browsing to a common domain (like a CDN).
* **Source:** `connect/net_extender.go`

---

### Community Proposals (Unimplemented)
*The following are high-priority suggestions from the community in restricted regions. They are not yet implemented in the official upstream repository.*

#### Proposal: The "One-Time VPN" Bootstrap
A user in China suggested a recovery flow for when the central API is blocked:
1. **Initial Sync:** Use an auxiliary VPN (e.g., Proton VPN) just once to exchange PGP public keys and download a cached node list from the central server.
2. **Persistence:** After the first sync, the client uses PGP-signed node updates routed through the nodes themselves (multi-hop) to stay synced without needing the auxiliary VPN again.

#### Proposal: Tiered Anti-Censorship Levels
Suggestions include adding user-selectable "Censorship Resistance Levels":
* **Level 1:** Standard multi-IP routing (Live).
* **Level 2:** Integrated Tor-bridge routing (Proposed).
* **Level 3:** Mixnet-style split-packet routing where data is fragmented across multiple paths simultaneously (Proposed).

---
*Tip: If you are in a highly restricted region, ensure you are running the latest version of the provider, as anti-censorship logic is frequently updated in the core protocol.*
