# Checking service status

### Official Status Page
Check [status.ur.io](https://status.ur.io) for the official status of the network components.
* **Limitations:** This page often checks for server heartbeat rather than full protocol functionality. A "Green" status may still exist during periods of API degradation.
* **Latency Graph:** Watch for spikes in latency; this is often a better indicator that the service is struggling.

### Community & Third-Party Monitors
* **Uptime Monitor:** [uptime.urnetwork.nohost.me/status/urnetwork-api](https://uptime.urnetwork.nohost.me/status/urnetwork-api)
* **Discord Community:** The `#support` and `#general` channels in the URnetwork Discord are the fastest ways to confirm global outages. If multiple users report "NSSF" (No Successful Strategy Found) or "payout delays," it is likely a network-wide issue.

### Payout Status
If payouts are delayed, check the announcements in Discord. The team is small and occasionally manages the payout wallet manually, which can lead to delays during network upgrades or maintenance.

> **Tip:** If the dashboard shows your nodes as "Offline" but your local logs show successful announcements, trust the logs. The dashboard UI can sometimes lag behind the actual network state.