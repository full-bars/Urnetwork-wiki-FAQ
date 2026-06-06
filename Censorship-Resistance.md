# Censorship Resistance

URnetwork is designed to function even in heavily restricted environments (e.g., the Great Firewall of China). This guide covers built-in features and community workarounds for bypassing active blocking.

### 1. DNS Protection (DoH)
DNS poisoning is a common censorship technique. URnetwork mitigates this by strictly enforcing **DNS-over-HTTPS (DoH)**.
* **Default:** Resolves via Cloudflare (`1.1.1.1`) or Google (`8.8.8.8`) over an encrypted tunnel.
* **Bypassing Hijacking:** Because the lookups are encrypted, local ISP firewalls cannot see or alter the results.
* **Code Reference:** `connect/net_http_doh.go`

### 2. Extenders (SNI Spoofing)
For environments where the protocol itself is identified and blocked, URnetwork uses **Extenders**.
* **Protocol:** `UREXTENDER1`.
* **How it works:** Traffic is "bounced" through an intermediary node. The connection uses **SNI Spoofing** to make the traffic look like harmless web browsing to a common domain (e.g., a CDN or a large tech company).
* **Reference:** `connect/net_extender.go`

### 3. GFW Blocking Workarounds (Community Proposal)
If the central URnetwork API is blocked in your region, follow this "Bootstrap" process:

1. **One-Time VPN Bootstrap:** 
   * Temporarily connect via an auxiliary VPN (e.g., Proton VPN).
   * Launch URnetwork to sync your initial node list and exchange security keys with the central server.
2. **Node-Only Operation:** 
   * Once you have a cached node list, you can disconnect the auxiliary VPN. 
   * URnetwork peer nodes are rarely blocked compared to the central API. The software can often maintain its own peer links once it has passed the initial bootstrap phase.
3. **E2EE Relay:** 
   * The protocol supports routing signaling through other nodes (multi-hop) to reach the central server. This allows you to stay synced even while the central API remains blocked.

### 4. Custom DNS Strategies
Advanced users can route their DNS queries through URnetwork nodes to reach non-private resolvers. This prevents your local ISP from tracking you while potentially improving resolution speed in certain regions.

---
*Tip: If you are in a highly restricted region, ensure you are running the latest version of the provider, as anti-censorship logic is frequently updated in the core protocol.*
