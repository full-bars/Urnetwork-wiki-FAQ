# Multiple nodes

### Can I run more than one node?
Yes, URnetwork allows running multiple provider nodes on the same account and even on the same IP address. 

### Running on the same IP
You can run 2 or 3 nodes on a single public IP to improve your "Stability" score.
* **Redundancy:** If one node crashes or is throttled by the OS, the other nodes keep the connection alive.
* **Reliability Score (Technical):** The protocol calculates reliability per IP using a "split" model (`1.0 / active_client_count`).
* **Earnings:** Running multiple nodes on one IP does **not** earn "extra" reliability points. The single point available for that IP is simply divided among your active nodes. However, it ensures you are much more likely to hit the **99.9% stability bonus**.

### Running on different IPs
This is the most effective way to scale your earnings. 
* **Geo-Distribution:** Running nodes in different countries or on different residential ISPs allows you to tap into multiple **Country Multipliers**.
* **Independent Scores:** Each unique IP earns its own full reliability point per payout period.

### Client ID Management
Each node must have its own unique **Client ID**. When using the CLI, this is handled automatically. For Docker deployments, ensure each container has a unique identifier or is using a fresh JWT.

### Hardware & Port Management
* **Resource Usage:** Each node has a baseline overhead. For users on limited hardware (Raspberry Pi/Low-end VPS), it is recommended to monitor CPU and RAM closely when scaling node count.
* **Port Management:** The provider uses UPnP/NAT-PMP to attempt to manage ports automatically with your router. 

---

### High-Performance Fork (v3.23-fix)
If you are running many nodes on a single machine or on very weak hardware, the [v3.23-fix fork](https://github.com/full-bars/urnetwork-3.23-fix) provides specific enhancements:
* **`lowmem` Profile:** A specialized mode that reduces buffer sizes and enforces strict memory limits for high-density setups.
* **Automatic Port Coordination:** Enhanced logic to ensure multiple nodes on one machine coordinate different internal ports without conflict.

> **Staff Tip:** The healthiest way to grow the network is "one node per household." Mass-deploying hundreds of nodes on a single data center IP often leads to poor reputation scores and fewer won contracts.
