# Multiple nodes

### One Account, Many Locations
You do not need separate URnetwork accounts for different IP addresses or geographical locations. A single account can manage nodes across the entire world. All statistics and earnings are aggregated under your one login.

### Optimal Node Density
* **Per IP Address:** The recommended configuration is **2–3 nodes per unique IP address**.
* **Redundancy:** Multiple nodes on one IP provide redundancy (if one process fails, others stay online).
* **Diminishing Returns:** Running 4 or more nodes on the same IP address provides no additional benefit and does not increase your reliability score for that IP.

### Managing Nodes via Dashboard
All active provider instances are visible in the [Client Manager](https://app.ur.network/account). Each node is assigned a unique **Client ID**.

> **Note:** If a provider crashes and restarts, it may generate a new Client ID. If your dashboard is "bloated" with offline nodes, they will eventually be cleared by the system once they haven't announced for a significant period.

### Geo-Optimization for Proxies
If you are deploying a large number of IPs via proxies:
* **Latency Matters:** Deploy your provider binary on a server geographically close to where your proxies are located.
* **Example:** Use a Finland-based server for Finland proxies. This minimizes latency and improves your contract win rate.
* **Avoid Oversaturation:** Don't overload a single server with thousands of global proxies. Sort them by region and distribute them across multiple localized servers for best performance.