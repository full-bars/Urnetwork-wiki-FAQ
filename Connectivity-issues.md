# Connectivity issues

### Provider Troubleshooting Checklist
If your provider is online but you cannot connect to it as a client:
1. **Update:** Re-run the install script from `ur.io` or pull the latest Docker image (`g4-latest`).
2. **Client ID:** Verify you are connecting using the specific **Client ID** generated for that node, not just your account name.
3. **Logs:** Run `urnetwork provide` to see live output. Look for "announcing" messages.
4. **Service Status:** Check [status.ur.io](https://status.ur.io) and the community-run [uptime monitor](https://uptime.urnetwork.nohost.me/status/urnetwork-api).
5. **Port:** Ensure your firewall allows outbound traffic on the necessary ports (default is usually 8080 or the dynamic proxy port assigned in your dashboard).

### Dropping Reliability Score
If your node is running 24/7 but your score is falling:
* **Backend Issues:** Periodic backend instability can cause scores to drop across the entire network. Check Discord `#support` to see if others are reporting the same.
* **Network Stability:** Ensure your internet connection is hardwired. WiFi and cellular connections are prone to jitter, which negatively impacts reliability scores.
* **Linger (Linux):** If running on Linux, ensure you enable "linger" to keep the user session active:
  ```bash
  sudo loginctl enable-linger $USER
  ```

### Mobile Background Restrictions
Android and iOS have aggressive battery optimization that kills background processes.
* **Android:** Disable battery optimization for the URnetwork app. On **Android 16** (beta), additional permissions may be required for VPN functionality.
* **iOS:** Apple's design fundamentally limits apps from running 24/7 in the true background. For 100% uptime, use the **Seeker** phone or a dedicated computer/VPS.

### "Provide while disconnected"
In the mobile app, this toggle allows your device to act as a provider even when you are not using the VPN as a client. This is **required** for maintaining your reliability score when you aren't active.

> **Staff Advice:** URnetwork is highly sensitive to latency. A computer with a wired network connection will always outperform a mobile phone in terms of winning contracts and maintaining uptime.