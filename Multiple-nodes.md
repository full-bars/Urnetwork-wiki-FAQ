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

### Hardware Limitations
While you can run many nodes, keep an eye on your hardware:
* **CPU/RAM:** Each node has a baseline overhead. Use the `lowmem` profile for high-density setups on weak hardware.
* **Port Conflict:** The provider uses UPnP/NAT-PMP to manage ports. Multiple nodes on one machine will automatically coordinate different internal ports.

> **Staff Tip:** The healthiest way to grow the network is "one node per household." Mass-deploying hundreds of nodes on a single data center IP often leads to poor reputation scores and fewer won contracts.
