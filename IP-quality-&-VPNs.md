# IP quality & VPNs

### Can I provide using another VPN?
While it is technically possible, providing bandwidth through another VPN (e.g., Mullvad, Proton) is **strongly discouraged** and usually results in near-zero earnings.

### How the Network Ranks Your IP
The URnetwork protocol continuously tracks the health of every provider node. These metrics are stored in the `reliability_score` table and directly affect your "reputation" in the marketplace.

**Key Metrics:**
* **Latency:** How fast your node responds to packets.
* **Jitter:** The consistency of your latency. High jitter (unstable ping) is often a sign of a saturated or throttled connection.
* **Uptime:** Nodes with 24/7 stability are prioritized for long-term contracts.

### Why VPNs Fail as Providers
1. **IP Identification:** URnetwork uses reputation databases. IPs identified as VPN exit nodes or data center proxies are ranked at the bottom.
2. **Double Latency:** Routing traffic through a VPN *before* it reaches you adds a massive latency penalty.
3. **The Zero-Contract Trap:** Even if you are in a high-multiplier region (e.g., 10x), if your IP reputation is low, you will win **zero contracts**. 10x multiplied by zero is still zero earnings.

### IP Types & Reputation
* **Residential IPs:** The "Gold Standard." These have the highest demand and win the most contracts.
* **Business/ISP IPs:** Generally perform well and are considered reliable.
* **Datacenter IPs:** Mostly used for signaling and infrastructure. They rarely win high-value residential traffic contracts.

### Checking Your Reputation
You can get a rough idea of your IP's "cleanliness" using tools like [ipfighter.com](https://ipfighter.com) or [scamalytics.com](https://scamalytics.com). If your IP is flagged as "High Risk" or "Proxy," your win rate will be significantly lower.

> **Staff Advice:** Your raw residential IP—even if its "abuse score" isn't perfect—will almost always outperform a VPN or proxy in terms of real earnings. The network is built to reward **real residential connectivity**.
